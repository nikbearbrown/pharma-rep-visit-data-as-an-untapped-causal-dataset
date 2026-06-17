# Chapter 2 Notes — What the Data Actually Is

*Research-gathering notes for Week 2 (Act I). The Veeva CLM schema, the two data planes (passive
media-player telemetry + rep-entered post-call data), the warehouse layer (IQVIA/Symphony NRx,
CMS Open Payments, NPI linkage, cohort/tenure), how the physician-by-time panel is assembled, and the
field-role classification (treatment / outcome / instrument / covariate / collider) that seeds
Ch 4–6. Notes only — not chapter prose; gather material, never fabricate citations.*

Primary sources: `pantry/rep-visit-as-causal-dataset_source.md` ("What the data actually is");
TIKTOC Week 2 entry. Field names verified against Veeva CRM Help / Vault CRM Help mid-2026 unless
marked `[verify]`. **Schema-velocity caveat (Adoption Risk #6): treat every `*_vod` field name below as
a dated mid-2026 example — Veeva CRM → Vault CRM migration is renaming/restructuring the model
(see D).**

---

## A. Foundations

**The unit-of-analysis move (the chapter's core conceptual claim).** The raw data arrives as *event
logs* — one row per slide view, one row per call, one row per payment. The causal unit is almost never
the click or the slide; it is **physician-time** (NPI × week or NPI × month). Chapter 2 is the
transformation from session rows to *analysis rows*: a physician-by-time panel whose columns carry the
four ingredients of a causal study — a **treatment** (which message variant was delivered), an
**outcome** (downstream NRx by NPI), an **instrument** (training-cohort timing), and **covariates**
(baseline prescribing, specialty, territory). The TIKTOC "research finding" for this week: *all four
ingredients are already present in the assembled data* — the dataset has been causal-ready for fifteen
years and nobody assembled it that way.

**Common misconception (load-bearing #1).**
> "The raw clickstream *is* the dataset."
**Wrong.** The clickstream (`Call_Clickstream_vod`) is one table at one grain (slide-view events within
a session). It is an *ingredient*, not the panel. The estimand — P(NRx | do(message)) — lives at
physician-time, which requires aggregating session rows to NPI-week, lagging the outcome, and joining
the warehouse layer. Mistaking the log for the dataset is the row-grain error that makes every
downstream design unstable.

**The two data planes (the chapter's spine).** Veeva CRM runs >80% of pharma field sales (author's
source-essay claim; treat the exact share as `[verify]` against a current market figure). Its CLM
(Closed-Loop Marketing) module produces two structurally different planes:

*Plane 1 — passive media-player telemetry (the iPad logs it; the rep does nothing).* While CLM content
is presented, a `Call_Clickstream_vod` record tracks the presentation; at session end it is packaged
and stamped onto `Multichannel_Activity_vod` (the session header) and `Multichannel_Activity_Line_vod`
(one line per slide/key-message view). Verified fields:
- **`Duration_vod`** — seconds spent on a slide; computed as (current time − start time). *Verified*
  (Veeva CRM Help, "Tracking CLM Call Clickstream Activity"). Note: a known Veeva support article
  documents **negative `Duration_vod`** values (clock/timezone artifacts) — a real data-quality gotcha
  worth a footnote.
- **`Display_Order_vod` / `View_Order_vod`** — the order in which key messages were displayed; this is
  the navigation sequence (loops, skips, back-tracks). *Verified* — `Display_Order_vod` is the
  configured display order of the key message; `View_Order_vod` appears on
  `Multichannel_Activity_Line_vod` as the actual view sequence. [verify the exact split between the two
  in current docs]
- **`Reaction_vod`** — the rep's per-slide reaction button capturing the HCP's reaction. *Verified*
  picklist values: **positive / neutral / negative / none** (Veeva CRM Help, "Capturing Reactions to
  Presentations"; carried into Vault CRM Help). The source essay's "+/neutral/−" is correct.
- **`Call_Channel_vod`** — channel of the interaction (F2F / remote / etc.); lives on the `Call2_vod`
  record. *Verified as a field*; exact picklist values are org-configured `[verify]`.
- **`Call_Clickstream_vod`** — the per-session activity record that captures presentation activity and
  whose fields are stamped (by default, plus admin custom fields) onto `Multichannel_Activity_Line_vod`.
  *Verified.*
- **`CLM_Presentation_vod` / `Key_Message_vod`** — content objects: a Multichannel Presentation binder
  becomes a `CLM_Presentation_vod` (one product per record via `Product_vod`); an individual slide
  becomes a `Key_Message_vod` (one product per record). *Verified.* These are how a "message variant"
  is identified in the schema.
- **`Multichannel_Activity_vod` / `Multichannel_Activity_Line_vod`** — the activity header/line objects
  that store displayed-slide tracking. *Verified.* (Note Veeva docs also reference
  `Multichannel_Line_vod` in the anonymous-tracking topic — `[verify]` whether this is an alias or a
  distinct object in the current model.)

*Plane 2 — rep-entered post-call data (the rep types it after the visit).* On the call report:
- **`Call2_Key_Message_vod__c` (the `Call2_Key_Message_vod` object)** — tracks key messages *delivered*
  during the call and carries the per-message reaction. *Verified as the object.* Important constraint:
  Veeva states **custom fields on `Call2_Key_Message_vod` are not supported** and won't display on
  calls — a real limit on what analysts can capture here. (Veeva CRM Help, "Using Key Messages on the
  Call Report.")
- **products detailed + priority** — which products were detailed and in what order/priority on the call
  (`Call_Detail_vod` lines / product-detail priority). *Verified as a concept; exact field API names
  `[verify]`.*
- **samples** — samples/promotional items left (Sample Management objects). *Verified domain; field
  names `[verify]`.*
- **free-text call notes** — the qualitative gold: the objection, the colleague mentioned, the
  formulary issue, the switch signal. This is the "why" the telemetry can't capture (forward-link to
  Ch 3's epistemic-gap and the Causal Interview Bot). *Free-text notes field exists; exact API name
  `[verify]`.*

**Why the two-plane distinction is causal, not just descriptive.** Plane 1 is *behavioral and passive*
(what happened, machine-logged); Plane 2 is *interpretive and rep-authored* (what the rep thinks
happened). The reaction button sits in *both* — and that is exactly the 47-second-slide ambiguity
(Ch 3): the same `Duration_vod` is consistent with "compelled," "skeptical," or "polite." The rep's
`Reaction_vod` is a *human label on an ambiguous signal*, not ground truth. Naming which plane a field
comes from is the first discipline before you let it into a model.

**The consent layer (governs whether Plane 1 exists at all for a physician).** *Verified against Veeva
CRM Help.* CLM tracking is gated by consent:
- **`CLM_EXPLICIT_OPT_IN_vod`** — a Multichannel Setting (custom setting). If **false**, accounts are
  *implicitly* opted in; if **true**, an account needs an explicit Opt-In `Multichannel_Consent_vod`
  record or it is treated as opted out. *Verified* (Veeva CRM Help, "Capturing Consent for CLM
  Tracking"). The account-level driver is `Account.CLM_Opt_Type_vod` with values
  `Never_vod` / `Implicit_Opt_In_vod` / `Explicit_Opt_In_vod`. *Verified.*
- **`CLM_OPT_OUT_BEHAVIOR_vod`** — a Multichannel Setting defining what happens for opted-out / no-
  consent accounts: **0 = no tracking** (the session vanishes), **1 = anonymous tracking** to
  `Multichannel_Activity_vod` / `Multichannel_Activity_Line_vod` (the activity is saved but **cannot be
  associated with the account/NPI** — reactions are kept, identity is stripped). *Verified* (Veeva CRM
  Help, "Tracking CLM Activity Anonymously": opted-out HCP's "reactions to the slides are saved
  anonymously," not linked to the account). **Refinement of the source essay:** the essay says opt-out
  "strips NPI from `Multichannel_Activity_vod`" — verified-correct in spirit, but the precise mechanism
  is *value 1 → anonymous write*, and the NPI-bearing association is what's removed.
- **Causal consequence (forward-flag to Ch 6 and Ch 13):** whether a physician's Plane-1 data exists,
  and whether it carries an NPI, depends on consent — and consent is plausibly correlated with both
  treatment-side factors (engaged reps, high-contact physicians) and outcome-side factors (prescribing
  propensity, industry skepticism). That makes the **sampling frame itself a selection collider** (the
  "consent collider"). Chapter 2's job is only to *map* this field and flag it; Ch 6 makes it the
  structural-bias centerpiece and Ch 13 makes it the deployment/ethics centerpiece.

**Worked example (event log → panel).** Goal: total effect of message variant M2 vs M1 on NRx.
1. Start from `Multichannel_Activity_Line_vod` (slide-view events) + `Call2_Key_Message_vod` (delivered
   messages) joined to `Call2_vod` (call header: NPI, rep, date, `Call_Channel_vod`).
2. Aggregate to **NPI-week**: define treatment = first week of exposure to message variant M2
   (identified via `Key_Message_vod` / `CLM_Presentation_vod`).
3. Join the **warehouse layer** by NPI: IQVIA Xponent / Symphony NRx, Open Payments meals, territory,
   rep tenure, training-cohort timing.
4. **Lag the outcome**: NRx in the next 4 / 8 weeks (or 30/60/90 days per the IV chapter); **retain
   pre-period prescribing** as a covariate.
5. Result: one row per NPI-week with treatment, lagged outcome, instrument (cohort timing), covariates,
   and a *flagged* post-treatment block (`Duration_vod`, `Reaction_vod`) that must **not** be used as a
   control for the total effect (Ch 6).

---

## B. Examples + failure case

- **Case 1 — public-data fallback (reproducible teaching).** No reader has Veeva access. CMS Open
  Payments (every payment/transfer of value > $10/instance or > $100/year to a physician, with the
  recipient's **NPI**) + CMS Medicare Part D Prescribers (drug-level prescribing counts/costs by NPI)
  can approximate a public physician-year panel: payments, specialty, geography, brand/generic mix. It
  lacks message-level telemetry — which is *why* the book also needs synthetic data. (CMS Open Payments;
  CMS Part D Prescribers.)
- **Case 2 — the synthetic CLM panel (ships with the book).** A generated physician-by-time panel with
  **known ground truth**: training cohort (Z), message variant (D), a latent receptivity term,
  consent status, `Duration_vod`/`Reaction_vod`, and NRx (Y). Because the DGP is known, the book can
  *demonstrate* collider bias (Ch 6) and IV recovery (Ch 4) against the true effect — the only honest
  way to teach the mechanism without proprietary data.
- **Case 3 — message variant as a real schema object.** "Efficacy-first vs safety-first deck" is not an
  abstraction: it is two `CLM_Presentation_vod` binders (or two `Key_Message_vod` sets) rolled out by
  training cohort. Grounding the treatment in the actual content objects keeps the IV design concrete.
- **Failure case — merging after the outcome (the row-grain trap).** If a post-treatment engagement
  signal (`Duration_vod`, `Reaction_vod`, in-visit reaction) is folded in as if it were a *pre-treatment
  covariate* — e.g., joined without respecting the time index, or used to "describe the physician" — the
  effect estimate is biased. This is the same structural error as Ch 6's collider, surfaced here as a
  *data-assembly* mistake: the panel's time discipline is what prevents it. (Forward to Ch 6.)
- **Data-quality failure to mention — negative `Duration_vod`.** Veeva's own support portal documents
  negative dwell values from clock artifacts; a "clean the panel" step has to handle them, illustrating
  that telemetry is not pristine.

---

## C. Connections

- **→ Ch 3 (Stuck on Rung 1):** with the data mapped, the bridge is "why is nobody using it causally?"
  The free-text notes and the reaction-button ambiguity set up Ch 3's epistemic gap (telemetry =
  *what*, not *why*).
- **→ Ch 4 (IV / staggered DiD):** the instrument (training-cohort timing) and treatment (message
  variant) are *defined* in this chapter's panel; Ch 4 tests relevance/independence/exclusion on it.
  Ch 4 needs `Call2_Key_Message_vod__c`, cohort timing (HR), IQVIA Xponent NRx, territory FE — all
  introduced here.
- **→ Ch 6 (collider trap + consent collider):** the field-role table below is the seed. `Duration_vod`
  / `Reaction_vod` are flagged here as post-treatment; Ch 6 proves *why conditioning on them is a
  structural error*. The consent fields mapped here become Ch 6's selection-collider centerpiece.
- **→ Ch 13 (consent + deployment):** `CLM_EXPLICIT_OPT_IN_vod` / `CLM_OPT_OUT_BEHAVIOR_vod` and the
  opt-out-missingness problem are revisited at the handoff (FCI, regulatory line, drift monitoring on
  opt-out-rate shifts).
- **Prerequisites:** basic table joins and time indexing (the panel is a join + a lag).
- **Adjacent:** Ch 1 motivates the data ("a 15-year natural experiment"); this chapter is the schema
  reality check before any design.

---

## D. Current state / key refs / last-3-yr

**Veeva schema (verify-before-print, schema velocity is high):**
- Veeva CRM Help — "Tracking CLM Call Clickstream Activity"; "Capturing Reactions to Presentations"
  (`Reaction_vod` = positive/neutral/negative/none); "Tracking CLM Key Messages"; "Using Key Messages
  on the Call Report" (`Call2_Key_Message_vod`); "Capturing Consent for CLM Tracking"
  (`CLM_EXPLICIT_OPT_IN_vod`, `CLM_Opt_Type_vod`); "Tracking CLM Activity Anonymously"
  (`CLM_OPT_OUT_BEHAVIOR_vod` 0/1). Pages last updated through May 2026.
  - `crmhelp.veeva.com/doc/Content/CRM_topics/Multichannel/CLM/DefaultFunct/AnalyzingCLM/TrackCLM.htm`
  - `crmhelp.veeva.com/doc/Content/CRM_topics/Multichannel/CLM/DefaultFunct/DisplayingPres/CaptureReactions.htm`
  - `crmhelp.veeva.com/doc/Content/CRM_topics/Activities/Call_Reporting_2/DefaultFunct/KeyMessages.htm`
  - `crmhelp.veeva.com/doc/Content/CRM_topics/Multichannel/CLM/AdvFunct/TrackingCLM/TrackCLMConsent.htm`
  - `crmhelp.veeva.com/doc/Content/CRM_topics/Multichannel/CLM/AdvFunct/TrackingCLM/TrackCLMAnon.htm`
  - Negative dwell: `support.veeva.com/hc/en-us/articles/222668167` (Negative Duration_vod).
- **Veeva CRM → Vault CRM migration (the load-bearing caveat).** Veeva is moving off the Salesforce-
  based "Veeva CRM" to its proprietary **Vault CRM**. End-of-support for legacy Veeva CRM was pulled
  forward to **end of December 2029** (from Sept 2030); first migrations in 2025, the bulk in 2026–2029;
  Vault CRM is GA for all new customers and 125+ customers were live as of early 2026. **Implication for
  the book:** Vault CRM has its own help domain (`vaultcrmhelp.veeva.com`) and the data model is being
  restructured — field API names (`*_vod`) may be renamed/relocated. *Every field in these notes is a
  dated mid-2026 example; the chapter must say so and cite both `crmhelp` and `vaultcrmhelp` where they
  diverge.* (Veeva press releases; IntuitionLabs/Clarkston migration roadmaps, 2025–2026.)

**Warehouse / market data (settled definitions):**
- **NRx / TRx / NBRx** (IQVIA / Symphony): **NRx** = new prescriptions (new-to-brand, new script,
  renewals of expired scripts), **excludes refills**; **TRx** = NRx + refills (total dispensed);
  **NBRx** = new-to-brand, patient must not have previously used the product (longitudinal patient
  linkage). The book's outcome is typically NRx at 30/60/90 days; NBRx is the cleaner "source of new
  business" but needs patient-level linkage. (IQVIA blog on NBRx in IC plans; Definitive Healthcare
  glossary; IQVIA Xponent is the canonical NRx/TRx source.)
- **IQVIA Xponent** — prescriber-level (NPI) projected Rx data; the standard field-force NRx/TRx source.
- **Symphony Health (ICON)** — claims-based, ~80% of US population / ~300M lives, ~92% retail claims,
  patient-level via anonymized patient ID; integrates Rx/Mx/Hx. An alternative warehouse source.
  (symphonyhealth.com; ICON Integrated Dataverse.)
- **CMS Open Payments** — public; payments/transfers of value to physicians (general / research /
  ownership), threshold > $10/instance or > $100/year, includes recipient **NPI**; downloadable. The
  reciprocity-pathway (M3) data and the public-fallback panel. (cms.gov OpenPayments data dictionary.)
- **CMS Medicare Part D Prescribers** — public NPI-level drug prescribing counts/costs; the public
  outcome proxy. (cms.gov.)
- **NPI** — National Provider Identifier; the join key linking Veeva account ↔ IQVIA/Symphony NRx ↔ Open
  Payments ↔ Part D. The panel is *built on NPI*; consent-driven NPI stripping (above) is therefore a
  join-integrity problem, not just a privacy one.

**Causal-role framing (the field-status table):**
- Pearl (2009), *Causality*, 2nd ed., Cambridge UP (collider/d-separation; field roles). [verify pages]
- Hernán & Robins, *Causal Inference: What If* (treatment/outcome/confounder/collider taxonomy).
- Greenland, Pearl & Robins (1999), "Causal Diagrams for Epidemiologic Research," *Epidemiology*
  10(1):37–48 (what a covariate-vs-collider distinction *is*).

**Settled vs contested.**
- *Settled:* public Part D + Open Payments support physician-level descriptive/observational study; the
  NRx/TRx/NBRx definitions; the consent-setting behavior as documented.
- *Contested / implementation-dependent:* exact CLM field availability and naming vary by org config and
  by CRM-vs-Vault version; public data still lacks message-level rep telemetry (hence synthetic data).

---

## E. Teaching

- **The whole chapter is a unit-of-analysis lesson.** Students treat row grain as obvious; it isn't.
  Drill the question "*what is one row, and is that the causal unit?*" before any field talk.
- **Read the `Call_Clickstream_vod` payload field by field (the TIKTOC opening).** Make the abstract
  schema tangible: here is the session record written at "Submit," here is each field, here is which
  plane it came from, here is its causal role. The payload *is* the chapter's cold open.
- **Two-plane card sort.** Hand Fellows ~20 fields; have them sort into (1) passive telemetry vs
  rep-entered, then (2) treatment / outcome / instrument / covariate / collider. The two sorts are
  different axes — a field's *plane* (how it was captured) and its *role* (how it may be used) are
  independent, and confusing them is the bug.
- **Build a data dictionary + role label** for the 20 synthetic fields — the deliverable that seeds
  Ch 4–6. Force a `[verify]` column so Fellows internalize schema velocity.
- **Analogy:** raw logs are ingredients; the panel is recipe-ready *mise en place*. You don't cook from
  the delivery crate.
- **Flag the trap early, resolve later.** Mark `Duration_vod`/`Reaction_vod` as "post-treatment — do not
  control" *here*, but defer the *why* to Ch 6 so the payoff lands. Same for the consent collider
  (map here, weaponize in Ch 6/13).

### Field-role classification table (seeds Ch 4–6 — roles are *for the total message→NRx effect*)

| Field / quantity | Plane | Default causal role | Note |
|---|---|---|---|
| Message variant (`CLM_Presentation_vod` / `Key_Message_vod`, `Call2_Key_Message_vod`) | telemetry + rep | **Treatment (D)** | the do() target |
| NRx / TRx / NBRx at 30/60/90d (IQVIA/Symphony, lagged) | warehouse | **Outcome (Y)** | must be time-lagged |
| Training-cohort timing (HR / rollout calendar) | warehouse | **Instrument (Z)** | valid only if independence + exclusion hold (Ch 4) |
| Baseline/pre-period NRx | warehouse | **Covariate** | pre-treatment; safe control |
| Specialty, geography, territory | warehouse | **Covariate** | pre-treatment |
| Rep tenure / training timing | warehouse | **Covariate (exclusion defense)** | controls the "early training = better selling" threat to Z |
| Open Payments meals/gifts | warehouse | **Covariate / mediator (M3 reciprocity)** | pre- vs post- depends on timing |
| `Duration_vod` (slide dwell) | telemetry | **Collider / post-treatment** | do **not** control for total effect (Ch 6) |
| `Reaction_vod` (per-slide reaction) | telemetry + rep | **Collider / post-treatment** | downstream of D *and* of physician traits |
| `Display_Order_vod` / `View_Order_vod` (navigation) | telemetry | **Post-treatment** (or treatment in the DML slide-sequence study) | role flips by estimand — Ch 11 |
| Knowledge/attitude shift | (latent) | **Mediator (M1 educational)** | use only for pathway split (Ch 5), not as control |
| Consent status (`CLM_Opt_Type_vod` / opt-out) | consent | **Selection collider (sampling frame)** | governs whether the row exists; Ch 6/13 |

---

## F. Library files (in `pantry/`)

- `_lib_ai-applied-causal-inference-powered-by-ml-and-ai.md` — treatment/outcome/instrument/covariate/
  collider taxonomy; DML on clickstream (the slide-sequence study where navigation flips role); good for
  the field-role table's structural-position framing.
- `_lib_ai-ml-assisted-adjustment.md` — the expert-interview → graph-audit → estimability-classification
  pipeline; supports the "specify roles before modeling" discipline and the data-dictionary deliverable.
- `_lib_science-the-book-of-why-the-new-science-of-cause-and-effect.md` — the accessible canonical
  treatment of confounder/mediator/collider roles (Pearl/Mackenzie) for the misconception framing.
- `_lib_math-causal-inference.md` / `_lib_math-causal-inference-mit-press-essential-knowledge-series.md`
  — conceptual reference for the role taxonomy.
- `_lib_did-applied-causal-inference_excerpt.md` — staggered-DiD panel assembly (the time-index +
  lag-the-outcome discipline that defines the panel) → links to Ch 4/10.
- `_lib_math-the-art-of-statistics-how-to-learn-from-data.md` /
  `_lib_math-naked-statistics-stripping-the-dread-from-the-data.md` — accessible grain/aggregation and
  data-as-ingredients framing for the teaching section.

---

## G. Gaps / flags

- **[verify] Veeva field names/behavior at print time.** All `*_vod` names are mid-2026 examples;
  Veeva CRM → Vault CRM migration (EOS Dec 2029) is renaming/restructuring the model. Re-check each
  against both `crmhelp.veeva.com` and `vaultcrmhelp.veeva.com` before publication. Adoption Risk #6.
- **[verify]** Exact split between `Display_Order_vod` (configured order) and `View_Order_vod`
  (actual view sequence on `Multichannel_Activity_Line_vod`); and whether `Multichannel_Line_vod`
  (seen in the anonymous-tracking topic) is an alias of `Multichannel_Activity_Line_vod`.
- **[verify]** `Call_Channel_vod` picklist values (org-configured); product-detail priority,
  samples, and free-text call-note field API names (objects confirmed, exact field names not pinned).
- **[verify]** The ">80% of pharma field sales on Veeva CRM" claim — author/source-essay figure; cite a
  current market-share source or soften to "the dominant pharma CRM."
- **Source-essay refinement (resolve in chapter):** opt-out does **not** simply "strip NPI" — the
  precise mechanism is `CLM_OPT_OUT_BEHAVIOR_vod` = 0 (no tracking) vs 1 (anonymous write to
  `Multichannel_Activity_(Line_)vod` with reactions saved but no account association). Use the precise
  version; flag forward to Ch 6 (consent collider) and Ch 13 (deployment).
- **Empirical open question (flag, don't assert):** that opt-out missingness *is* correlated with
  physician characteristics is highly plausible but unproven; the synthetic panel can *demonstrate* the
  mechanism with known ground truth. (Shared with Ch 6.)
- **Data-quality reality:** negative `Duration_vod`, clock artifacts, and config-dependent tracking mean
  the panel needs a cleaning step — telemetry is not pristine; the chapter should not pretend otherwise.
- **[verify] Pearl (2009) edition/pages** — cited from bibliographic record, not re-checked this pass.
