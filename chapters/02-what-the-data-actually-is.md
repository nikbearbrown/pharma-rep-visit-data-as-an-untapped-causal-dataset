# Chapter 2: What the Data Actually Is

## Overview

In Chapter 1 you learned to see rep-visit telemetry as causal-grade behavioral data and you wrote down your do-question. Now you have to be precise. This chapter takes you from the abstract claim "there's an experiment in here" to a concrete, assembled, analysis-ready dataset — and to the discipline that prevents you from corrupting it before you ever fit a model.

By the end, you will be able to: read the two structurally different *planes* of the closed-loop-marketing (CLM) schema — the passive media-player telemetry the iPad logs, and the rep-entered post-call data — plus the warehouse layer (prescribing, payments, cohort timing) that gets joined on. You will be able to perform the unit-of-analysis move that turns event logs into a **physician-by-time panel**. And — the load-bearing skill — you will be able to classify each field by its *causal role*: treatment, outcome, instrument, covariate, or collider. That classification is the seed of every design in Chapters 4 through 6, and getting it wrong is how analyses quietly fail.

A standing caveat, stated once and meant throughout: every `*_vod` field name below is a dated mid-2026 example. Veeva is migrating from its Salesforce-based "Veeva CRM" to a proprietary "Vault CRM" (end-of-support for legacy Veeva CRM was pulled forward to end of December 2029; Vault CRM is generally available and had 125+ live customers in early 2026). The data model is being restructured and API names may be renamed or relocated. Learn the *roles*; verify the *names* against both `crmhelp.veeva.com` and `vaultcrmhelp.veeva.com` before you trust any one of them.

## Learning objectives

After this chapter, you should be able to:

- **(Understand)** Describe the two CLM data planes (passive media-player telemetry vs. rep-entered post-call data) and the warehouse layer, and explain why the distinction is causal and not merely descriptive.
- **(Apply)** Assemble a (synthetic) physician-by-time panel from event logs: aggregate to NPI-time, lag the outcome, join the warehouse layer, and quarantine the post-treatment block.
- **(Analyze)** Classify each field as treatment, outcome, instrument, covariate, or collider for the total message→prescribing effect.

## Opening case: the join that ran after the outcome

A Fellow, eager and competent, builds the first version of the panel. She has session-level telemetry, she has prescribing data, she has payment data. She joins them all on physician NPI into one wide table — one row per physician — and starts modeling the effect of the message on prescribing, "controlling for engagement" by including average slide dwell time and the average reaction score as covariates. The regression runs. The coefficients look plausible. She reports a message effect.

The estimate is biased, and the bug is not in the model. It is in the *assembly*.

She folded slide dwell and reaction — signals produced *during and after* the message was delivered — into the same flat row as the message itself, with no time index separating cause from consequence, and then "controlled for" them. She conditioned on variables that sit *downstream* of the treatment. We have not yet proven why that is fatal (Chapter 6 does, formally — these are colliders, and conditioning on them opens a spurious path). But notice where the damage was done: not at the modeling step, at the *data-assembly* step. The panel's time discipline — what is pre-treatment, what is treatment, what is post-treatment, and what must never be used as a control — is the thing that protects the estimate. Lose the time index and you have built a trap that no model can climb out of.

This chapter is, at bottom, a lesson in not building that trap.

## Core sections

### The unit-of-analysis move: from event logs to a panel

The raw data arrives as **event logs**: one row per slide view, one row per call, one row per payment. The instinct is to treat the most granular log — the clickstream — as "the dataset." That instinct is the first bug.

> **Load-bearing misconception #1:** *"The raw clickstream is the dataset."* Wrong. The clickstream (`Call_Clickstream_vod`) is one table at one grain — slide-view events within a session. It is an *ingredient*, not the panel. Your estimand, `P(NRx | do(message))`, lives at a different grain entirely: **physician-time** (NPI × week, or NPI × month). Mistaking the log for the dataset is the row-grain error that destabilizes every downstream design.

So the central conceptual claim of this chapter is a *unit-of-analysis* move. The causal unit is almost never the click or the slide. It is **physician-time**: one row per physician per time bucket. Building the panel means transforming session rows into analysis rows whose columns carry the four ingredients of a causal study:

- a **treatment** — which message variant was delivered (the do-target),
- an **outcome** — downstream new prescriptions for that physician (time-lagged),
- an **instrument** — training-cohort timing (the source of quasi-random variation; Chapter 4),
- **covariates** — pre-treatment characteristics: baseline prescribing, specialty, territory, rep tenure.

The research finding for this week, which we will earn by the end: **all four ingredients are already present** in the assembled data. The dataset has been causal-ready for fifteen years. Nobody assembled it that way.

A useful analogy: raw logs are *ingredients*; the panel is recipe-ready *mise en place*. You do not cook from the delivery crate. Aggregation, time-indexing, lagging, and joining are the prep work, and they are where causal correctness is won or lost.

### Plane 1 — passive media-player telemetry (the iPad logs it)

The first plane is *behavioral and passive*: machine-logged, recording what happened, no deliberate data entry. While CLM content is presented, a clickstream record tracks the presentation; at session end it is packaged and stamped onto activity records. The fields (verified against Veeva CRM Help, mid-2026):

- **`Duration_vod`** — seconds spent on a slide, computed as (current time − start time). *A real gotcha:* Veeva's own support portal documents **negative `Duration_vod`** values arising from clock or timezone artifacts. Your cleaning step has to handle them. Telemetry is not pristine, and a chapter that pretended otherwise would be lying to you.
- **`Display_Order_vod` / `View_Order_vod`** — the order in which key messages were displayed: loops, skips, back-tracks. `Display_Order_vod` is the *configured* display order; `View_Order_vod` (on the activity-line object) appears to be the *actual* view sequence. (The exact split between the two in current docs is `[verify]`.)
- **`Reaction_vod`** — the per-slide reaction button, capturing the rep's reading of the physician's reaction. Verified picklist values: **positive / neutral / negative / none**. The source essay's "+/neutral/−" is correct.
- **`Call_Channel_vod`** — the channel of the interaction (face-to-face / remote / etc.), on the call record. The field is verified; exact picklist values are org-configured (`[verify]`).
- **`Call_Clickstream_vod`** — the per-session activity record capturing presentation activity, whose fields are stamped onto the activity-line object.
- **`CLM_Presentation_vod` / `Key_Message_vod`** — the content objects. A multichannel presentation binder becomes a `CLM_Presentation_vod` (one product per record); an individual slide becomes a `Key_Message_vod`. These are *how a "message variant" is identified in the schema* — your treatment lives here.
- **`Multichannel_Activity_vod` / `Multichannel_Activity_Line_vod`** — the activity header/line objects that store displayed-slide tracking. (Veeva docs also reference `Multichannel_Line_vod` in the anonymous-tracking topic; whether that is an alias or a distinct object is `[verify]`.)

### Plane 2 — rep-entered post-call data (the rep types it)

The second plane is *interpretive and rep-authored*: what the rep thinks happened, entered after the visit. On the call report:

- **`Call2_Key_Message_vod`** (the object; the field `Call2_Key_Message_vod__c`) — tracks key messages *delivered* during the call and carries the per-message reaction. A real constraint: Veeva states **custom fields on `Call2_Key_Message_vod` are not supported** and will not display on calls — a hard limit on what analysts can capture here.
- **products detailed + priority** — which products were detailed, in what order (`Call_Detail_vod` lines / product-detail priority). Concept verified; exact field API names `[verify]`.
- **samples** — samples or promotional items left (Sample Management objects). Domain verified; field names `[verify]`.
- **free-text call notes** — the qualitative gold: the objection raised, the colleague mentioned, the formulary issue, the switch signal. This is the "why" the telemetry cannot capture — and it forward-links directly to Chapter 3's epistemic gap and to the Causal Interview Bot (Chapter 8). (Field exists; exact API name `[verify]`.)

### Why two planes is a causal distinction, not a filing system

Here is why we insist on the two-plane split. Plane 1 is behavioral and passive (machine-logged: *what happened*). Plane 2 is interpretive and rep-authored (*what the rep thinks happened*). And the reaction button sits in *both* — which is exactly the 47-second-slide ambiguity from Chapter 1. The same `Duration_vod` is consistent with "compelled," "skeptical," or "polite," and the rep's `Reaction_vod` is a *human label on an ambiguous signal*, not ground truth. Naming which plane a field came from — was this measured or interpreted? — is the first discipline before you let any field into a model. A field's *plane* (how it was captured) and its *role* (how it may be used) are independent axes, and confusing them is a recurring bug.

### The consent layer (whether Plane 1 exists at all)

Before you can use a physician's telemetry, you have to know whether it exists — and that is governed by consent. (We *map* the fields here; Chapter 6 weaponizes them as a structural bias and Chapter 13 makes them a deployment constraint.)

- **`CLM_EXPLICIT_OPT_IN_vod`** — a multichannel setting. If **false**, accounts are *implicitly* opted in; if **true**, an account needs an explicit opt-in consent record or it is treated as opted out. The account-level driver is `Account.CLM_Opt_Type_vod` with values `Never_vod` / `Implicit_Opt_In_vod` / `Explicit_Opt_In_vod`. (Verified against Veeva CRM Help.)
- **`CLM_OPT_OUT_BEHAVIOR_vod`** — defines what happens for opted-out accounts: **0 = no tracking** (the session simply vanishes); **1 = anonymous tracking** (the activity is written to the activity objects, reactions are saved, but it *cannot be associated with the account/NPI* — identity is stripped).

A precision correction the notes flag: the source essay says opt-out "strips NPI from `Multichannel_Activity_vod`." That is right *in spirit*, but the precise mechanism is `CLM_OPT_OUT_BEHAVIOR_vod` = 1 → an anonymous write with reactions kept and the NPI-bearing association removed (or = 0 → no write at all). Use the precise version.

The causal consequence — flag it now, do not develop it: whether a physician's Plane-1 data exists, and whether it carries an NPI, depends on consent. And consent is plausibly correlated with both treatment-side factors (engaged reps, high-contact physicians) and outcome-side factors (prescribing propensity, industry skepticism). That makes the **sampling frame itself a selection collider** — the "consent collider." Because the NPI is the join key for the entire panel, consent-driven NPI stripping is not only a privacy issue; it is a *join-integrity* issue. (Whether opt-out missingness *is* in fact correlated with physician characteristics is highly plausible but unproven — an empirical open question. The synthetic panel below can *demonstrate* the mechanism with known ground truth; the chapter does not assert the correlation as fact.)

## Worked example: event log → panel (with the dead end)

Goal: estimate the total effect of message variant M2 versus M1 on new prescriptions. Here is the process, including the wrong turn.

**The dead end (the opening case, generalized).** The fast move is to join everything on NPI into one wide row per physician and throw it at a model, including dwell and reaction as "engagement controls." We saw why this fails: it collapses the time dimension, putting cause and consequence in the same flat row, and then conditions on post-treatment signals. The estimate is corrupted at assembly. The fix is not a fancier estimator. It is *respecting time*.

**The disciplined build.**

1. **Start from the right grain.** Begin with `Multichannel_Activity_Line_vod` (slide-view events) and `Call2_Key_Message_vod` (delivered messages), joined to `Call2_vod` (the call header: NPI, rep, date, `Call_Channel_vod`).
2. **Aggregate to NPI-week.** Define the treatment as the first week of exposure to message variant M2, identified via `Key_Message_vod` / `CLM_Presentation_vod`. Now one row is one physician-week — the causal unit.
3. **Join the warehouse layer by NPI.** Append prescribing (IQVIA Xponent or Symphony NRx), Open Payments meals, territory, rep tenure, and training-cohort timing.
4. **Lag the outcome.** Define the outcome as new prescriptions in the *next* 4 / 8 weeks (or 30 / 60 / 90 days, per the IV chapter). Crucially, **retain pre-period prescribing as a covariate** — it is pre-treatment and a safe, valuable control.
5. **Quarantine the post-treatment block.** The result is one row per NPI-week with treatment, lagged outcome, instrument (cohort timing), and covariates — plus a *flagged* block (`Duration_vod`, `Reaction_vod`) that is marked **do not use as a control for the total effect**. Quarantine, not delete: those fields are colliders for *this* estimand but become the treatment itself in the slide-sequencing study (Chapter 11). Role flips by estimand.

**The lesson.** The panel's correctness is a property of its *time index*, not its column count. Pre-treatment covariates (baseline prescribing, specialty) help; post-treatment signals (dwell, reaction) hurt, when the estimand is the total message effect. The discipline is to decide each field's temporal position *relative to the treatment* before you decide whether you are allowed to control for it.

**The limit.** This build assumes you can cleanly identify "first week of exposure to M2" — which assumes the message variant is faithfully recorded (`Call2_Key_Message_vod`'s custom-field limits bite here) and that consent has not silently removed a non-random slice of physicians from the frame. Both assumptions are real and both are revisited (Chapter 6, Chapter 13).

## The field-role classification table

This table seeds Chapters 4 through 6. Roles are stated *for the total message→NRx effect* — and several flip under a different estimand, which is the point.

| Field / quantity | Plane | Default causal role | Note |
|---|---|---|---|
| Message variant (`CLM_Presentation_vod` / `Key_Message_vod`, `Call2_Key_Message_vod`) | telemetry + rep | **Treatment (D)** | the do() target |
| NRx / TRx / NBRx at 30/60/90d (IQVIA/Symphony, lagged) | warehouse | **Outcome (Y)** | must be time-lagged |
| Training-cohort timing (HR / rollout calendar) | warehouse | **Instrument (Z)** | valid only if independence + exclusion hold (Ch 4) |
| Baseline / pre-period NRx | warehouse | **Covariate** | pre-treatment; safe control |
| Specialty, geography, territory | warehouse | **Covariate** | pre-treatment |
| Rep tenure / training timing | warehouse | **Covariate (exclusion defense)** | controls the "early training = better selling" threat to Z |
| Open Payments meals / gifts | warehouse | **Covariate / mediator (M3 reciprocity)** | pre- vs post- depends on timing |
| `Duration_vod` (slide dwell) | telemetry | **Collider / post-treatment** | do **not** control for total effect (Ch 6) |
| `Reaction_vod` (per-slide reaction) | telemetry + rep | **Collider / post-treatment** | downstream of D *and* of physician traits |
| `Display_Order_vod` / `View_Order_vod` (navigation) | telemetry | **Post-treatment** (or *treatment* in the DML slide-sequence study) | role flips by estimand (Ch 11) |
| Knowledge / attitude shift | (latent) | **Mediator (M1 educational)** | for pathway split (Ch 5), not as control |
| Consent status (`CLM_Opt_Type_vod` / opt-out) | consent | **Selection collider (sampling frame)** | governs whether the row exists; Ch 6/13 |

Two sorts, two axes. First sort each field by *plane* (passive telemetry vs. rep-entered vs. warehouse vs. consent). Then sort by *role* (treatment / outcome / instrument / covariate / collider). They are different questions. A field captured passively (`Reaction_vod`) can still be a forbidden control. A field a human typed (`Call2_Key_Message_vod`) can be your treatment. Keep the axes separate.

### Defining the warehouse outcomes

The outcome deserves its own precision, because "prescribing" hides three different measures (from IQVIA / Symphony):

- **NRx** = new prescriptions (new-to-brand, new scripts, renewals of expired scripts); **excludes refills**.
- **TRx** = NRx + refills (total dispensed).
- **NBRx** = new-to-brand: the patient must not have previously used the product (requires longitudinal patient linkage).

The book's default outcome is NRx at 30/60/90 days. NBRx is the cleaner "source of new business" but needs patient-level linkage you may not have. **IQVIA Xponent** is the canonical prescriber-level (NPI) projected NRx/TRx source; **Symphony Health (ICON)** is a claims-based alternative. The **NPI** is the join key linking the Veeva account to NRx, to Open Payments, and to Part D.

## Common misconceptions

**"The raw clickstream is the dataset."** It is one table at one grain — an ingredient. The dataset is the assembled physician-time panel.

**"More columns is more control, so include dwell and reaction."** Including post-treatment signals as controls biases the total effect. Column count is not rigor; temporal position is.

**"A field's plane tells you its role."** No. How a field was captured (plane) and how it may be used (role) are independent. Passive `Reaction_vod` is a forbidden control; rep-typed `Call2_Key_Message_vod` is your treatment.

**"Consent missingness is just a privacy footnote."** It is a join-integrity and selection problem: opt-out can remove a non-random slice of physicians from the frame, which is a collider on the sampling frame (Chapter 6).

## Evidence check

**Independent vs. vendor vs. hypothetical vs. contested.**

- *Independent / verified:* The CLM field behaviors (`Duration_vod`, `Reaction_vod` picklist, consent settings, anonymous-tracking mechanism, negative-duration gotcha) are documented in Veeva's own CRM Help and support portal. The NRx/TRx/NBRx definitions are standard IQVIA/Symphony usage. Open Payments and Part D are public CMS datasets with documented dictionaries.
- *Vendor:* The "above 80% of pharma field sales" figure is the source essay's; carried `[verify]` — soften to "the dominant pharma CRM" absent an independent market citation.
- *Hypothetical:* The opening Fellow and the synthetic panel are illustrative; the synthetic panel is *deliberately* synthetic (known ground truth) precisely so the book can teach without proprietary data.
- *Contested / implementation-dependent:* Exact field availability and naming vary by org config and by CRM-vs-Vault version. Public data lacks message-level telemetry — which is *why* the book also ships synthetic data.

**Consent / compliance & patient-welfare check.** This chapter *maps* the consent architecture (`CLM_EXPLICIT_OPT_IN_vod`, `CLM_OPT_OUT_BEHAVIOR_vod`, `CLM_Opt_Type_vod`) without yet exploiting it. The welfare point: the entire panel is built on linking real prescribers' behavior to their prescribing, and the only regulatorily defensible target to optimize is the educational pathway (Chapter 5, Chapter 13). The public-data/IP firewall is operative here: nothing proprietary belongs in the panel you build for the book — public (Open Payments, Part D) and synthetic data only; the partner replicates on real CRM data internally.

## Named Fellow artifact: assemble a synthetic physician-by-time panel + classify every field

Your artifact for this chapter has two parts.

**Part 1 — assemble.** Using the synthetic CLM panel that ships with the book (training cohort Z, message variant D, a latent receptivity term, consent status, `Duration_vod`/`Reaction_vod`, and NRx Y, with *known ground truth*), perform the five-step build: aggregate session rows to NPI-week, define the treatment, join the warehouse fields, lag the outcome, and quarantine the post-treatment block. Add a cleaning step that handles negative `Duration_vod`. Output: one row per NPI-week.

**Part 2 — classify.** Produce a data dictionary for ~20 fields with three columns: **plane**, **causal role** (treatment / outcome / instrument / covariate / collider), and a **`[verify]` flag** column (so schema velocity is internalized). For each field marked collider, write one sentence on *why* its structural position makes it post-treatment. This dictionary is the direct input to the IV design (Chapter 4) and the collider audit (Chapter 6).

## Research finding for Fellows

The finding for this week is concrete and verifiable on the synthetic panel: **all four ingredients of a causal study — treatment, outcome, instrument, covariates — are already present in the assembled rep-visit data.** The message variant is a real schema object. The outcome is a standard NRx panel join. The instrument is the training-cohort calendar. The covariates are warehouse fields. Nothing is missing. What has been missing is the *assembly discipline* and the *causal frame* — not the data. The dataset has been causal-ready for fifteen years, and the synthetic panel lets you prove your assembly recovers the known truth before you ever touch real data.

## Exercises

1. **(Understand)** Explain, using the two-plane distinction, why `Reaction_vod` is dangerous as a model feature even though it is recorded automatically. Reference the 47-second-slide ambiguity.

2. **(Apply — produces an artifact)** Assemble the synthetic physician-by-time panel (Part 1 of the named artifact). Verify your build by checking that your treatment-timing column matches the synthetic cohort rollout. Document your cleaning step for negative `Duration_vod`.

3. **(Analyze — produces an artifact)** Complete the field-role classification dictionary (Part 2) for 20 fields, including the `[verify]` column. Identify at least one field whose role *flips* under a different estimand and name both estimands.

4. **(Apply)** Take the public-data fallback (Open Payments + Part D, joined on NPI) and assemble a public physician-year panel of payments, specialty, geography, and brand/generic mix. Then state precisely what this public panel *cannot* contain that the synthetic panel can — and why that gap is the reason synthetic data exists.

## Five-part AI block

**When to use AI here.** Use an LLM to draft the data-dictionary scaffold and to propose, for each field, a candidate plane and role — *then audit it*. Use Claude Code to write the panel-assembly script (group-by to NPI-week, the lag, the join), which is mechanical and well-suited to code generation.

**When NOT to use AI here.** Do not trust an LLM's claim that a given Veeva field "exists" or "means X" — schema velocity (CRM → Vault) makes its training data stale, and it will confidently emit deprecated or invented field names. Verify field names against `crmhelp.veeva.com` / `vaultcrmhelp.veeva.com`, not against the model. And never let the model decide a *causal role* for you: plane is a documentation question, role is a structural-judgment question, and the role is where bias enters.

**LLM exercise.** Paste this prompt:

```
I am assembling a physician-by-time panel from pharma rep-visit data to estimate
the TOTAL effect of a message variant on new prescriptions (NRx).

Here is my field list: [paste your 20 fields].

For EACH field, propose: (1) which plane it likely belongs to — passive telemetry,
rep-entered, warehouse, or consent; (2) its causal role for the TOTAL message effect
— treatment, outcome, instrument, covariate, or collider; (3) whether it is
pre-treatment or post-treatment in time.

Then flag any field where you are GUESSING the role, and explain the structural
reason a field would be a collider rather than a covariate. Do not invent field
definitions; if you are unsure what a field captures, say so.
```

**CLI exercise.** In Claude Code, have it write `assemble_panel.py` against the synthetic data: a function that aggregates slide-view rows to NPI-week, defines treatment as first-exposure week, lags NRx by a configurable horizon, joins warehouse fields, drops/flags negative `Duration_vod`, and outputs the panel with the post-treatment block in a clearly named, *separate* set of columns (e.g., prefixed `posttx_`). Read the function before running it.

**AI validation.** Validate against ground truth: because the synthetic panel has a known data-generating process, fit a naive model that *includes* the post-treatment block and one that *excludes* it, and check which recovers the known message effect. If the AI-written assembly produced a panel where "controlling for engagement" moves you *away* from the true effect, the assembly correctly preserved the collider — and you have just demonstrated Chapter 6's lesson a chapter early. Also re-verify two of the AI's proposed field definitions against the live Veeva docs; if either is wrong, treat the whole list as suspect.

## AI Use Disclosure

This chapter was drafted by a human author from research notes; an AI assistant helped structure the field tables and prose. Every Veeva field name and behavior is carried at the verification status the notes assigned it (`[verify]` where flagged), and no field definition was accepted from the model in place of the documentation.

## What would change my mind

The chapter claims the panel can be assembled cleanly from these planes and that the four causal ingredients are all present. I would revise if: (1) the message variant turns out *not* to be reliably reconstructable from the schema in practice — e.g., `Call2_Key_Message_vod`'s custom-field limits or org-config variation make "which variant was delivered" too noisy to serve as a treatment; (2) consent opt-out removes such a large or such a skewed slice of physicians that no usable, representative panel survives (Chapter 6, Chapter 13); or (3) the Veeva → Vault migration restructures the model so deeply that the field-role mapping here does not transfer at all (in which case the *roles* survive but the *names* must be wholly redone).

## Still puzzling

What fraction of physicians sit in the anonymous-tracking (`CLM_OPT_OUT_BEHAVIOR_vod` = 1) state, and is their stripped-NPI data ever recoverable for analysis or simply lost? Is opt-out missingness actually correlated with prescribing — plausible but unproven, and the synthetic panel can only *demonstrate* the mechanism, not measure the real correlation? And the unresolved `[verify]`s themselves: the exact `Display_Order_vod` vs. `View_Order_vod` split, the `Call_Channel_vod` picklist, the free-text-note API name — small now, but each is a place where a real build could stumble.

## Summary

The raw rep-visit data is event logs at three grains — slide views, calls, payments — and the dataset you actually analyze is none of them: it is a physician-by-time panel you assemble. The CLM schema has two structurally different planes (passive media-player telemetry vs. rep-entered post-call data) plus a consent layer that decides whether a physician's data exists at all, and a warehouse layer (NRx, Open Payments, cohort timing) joined on NPI. Assembly is where causal correctness is won: aggregate to NPI-time, lag the outcome, keep pre-treatment covariates, and quarantine the post-treatment block. Classify every field twice — by plane and by role — and never let the first axis decide the second. Do it right and you discover the week's finding: all four ingredients of a causal study were here the whole time.

## Key terms

- **Data plane:** a structurally distinct source within the schema — passive media-player telemetry (machine-logged) vs. rep-entered post-call data (human-authored).
- **Physician-time panel:** the analysis-ready table at one-row-per-physician-per-time-bucket (NPI × week/month); the causal unit of analysis.
- **Estimand:** the precise causal quantity you intend to estimate (e.g., the total effect of the message on NRx) — it determines each field's role.
- **NRx / TRx / NBRx:** new prescriptions (excl. refills) / total dispensed / new-to-brand (patient-naïve); the warehouse outcome measures.
- **Treatment / outcome / instrument / covariate / collider:** the five causal roles a field can play; the role, not the plane, governs whether a field may be used as a control.
- **Collider (preview):** a variable downstream of both the treatment and other causes; conditioning on it induces bias (Chapter 6).
- **Consent collider:** the selection structure created because consent governs whether a physician's row exists and is plausibly correlated with treatment and outcome.
- **IP firewall:** the discipline that keeps proprietary data out of the public book/repo; the book uses public and synthetic data, the partner replicates internally.

## Bridge

With the data mapped, classified, and assembled, the obvious question is the uncomfortable one: it has all been here for fifteen years — so why is nobody using it causally? Chapter 3 answers with Pearl's ladder, showing that content optimization, rep-performance management, and next-best-action engines are all stuck on Rung 1, and naming the Rung-2 question the budget actually needs.

## Further reading

- Veeva CRM Help: "Tracking CLM Call Clickstream Activity"; "Capturing Reactions to Presentations"; "Using Key Messages on the Call Report"; "Capturing Consent for CLM Tracking"; "Tracking CLM Activity Anonymously." (`crmhelp.veeva.com` — the primary documentation for every field in this chapter; cross-check `vaultcrmhelp.veeva.com` for the Vault CRM migration.)
- CMS Open Payments data dictionary and CMS Medicare Part D Prescribers. (`cms.gov` — the public datasets behind the public-fallback panel.)
- Greenland, S., Pearl, J., & Robins, J. M. (1999). "Causal Diagrams for Epidemiologic Research." *Epidemiology*, 10(1), 37–48. (The covariate-vs-collider distinction that the field-role table operationalizes.)
- Hernán, M. A., & Robins, J. M. *Causal Inference: What If.* (The treatment/outcome/confounder/collider taxonomy.)
