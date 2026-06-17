# Chapter 10 — Project 1: Staggered Difference-in-Differences on Message-Variant Transitions
*The variation has been sitting in the CRM for fifteen years. The method to read it honestly now exists. The discipline is falsification first, effect second.*

A brand team rolls out a new clinical deck — a shift from efficacy-first to safety-first messaging. The rollout is staggered: the West region's reps train in January, the Midwest in February, the Southeast in March, simply because that is how the training logistics fell. Six months later, an analyst runs the obvious regression. She builds a panel of physician-month prescribing, codes a treatment dummy that flips on once a physician's rep is delivering the new deck, adds physician fixed effects and month fixed effects, and reads the coefficient.

It comes back negative and significant. The new deck, the regression says, *reduced* prescribing.

The brand team is alarmed. Someone drafts a memo recommending a rollback.

Here is the problem: the deck worked. Every cohort that received it prescribed *more* afterward. The negative coefficient is an artifact — a known, named, mathematically guaranteed artifact of running two-way fixed effects on staggered data with effects that grow over time. The regression quietly used the already-treated January cohort as a "control" for the later-treated March cohort, and because the January effect was still growing during March's comparison window, that comparison entered with the wrong sign and a *negative weight*. The estimator averaged real positive effects into a fake negative total.

The analyst did nothing careless. She ran the textbook regression. The textbook was wrong. This chapter is about not being that analyst — and, more usefully, about being the analyst who can *show* the brand team exactly where the negative number came from and hand them the right one.

---

## The estimand: treatment-on-the-treated

Before any code, name what you are estimating. Difference-in-differences does not give you a population average treatment effect — the effect a message variant would have on a randomly chosen physician. It gives you the **average treatment effect on the treated** (ATT), also called treatment-on-the-treated (ToT): the average effect of the new message on the physicians whose reps actually delivered it.

That is the right estimand. The brand does not detail a random physician. It details the physicians its reps call on. The decision the budget funds is "what does our new deck do to the physicians we reach," and that is the ToT.

In the canonical 2×2 DiD:

```
ATT = E[ΔY | treated] − E[ΔY | control]
```

where ΔY is the change in outcome from before to after. Here ΔY is the change in NPI-level new prescriptions at 30, 60, and 90 days after the message transition. The treated are physicians of cohort reps already delivering the new deck; the controls are physicians of reps not yet trained.

---

## The load-bearing assumption: parallel trends

DiD buys you one thing for free and charges you for another. For free, it allows treated and control groups to sit at completely different *levels* — your treated physicians can be higher prescribers to begin with, and the design differences that out. What it does not allow is differing *trends*. The assumption is that, absent the new message, the treated physicians' prescribing *would have evolved the same way* as the control physicians'. That is **parallel trends**, and it is the entire identifying assumption.

Two riders matter operationally. Parallel trends differences out levels, not pre-existing trends — a gap at baseline is fine, but a pre-existing divergence will be misread as a treatment effect. And parallel trends is **functional-form dependent**: if trends are parallel in prescribing counts, they need not be parallel in log prescribing counts (Roth & Sant'Anna 2023, *Econometrica* 91(2):737–747). For prescribing data that grows multiplicatively, the linear and log specifications can disagree. The scale is a modeling choice you must make deliberately, not a default.

---

## Why two-way fixed effects fails here

The intuitive design is **two-way fixed effects (TWFE)**: regress prescribing on a treatment dummy plus unit fixed effects plus time fixed effects. For a single treatment time, this reduces to the clean 2×2 DiD and is fine. Under staggered adoption with heterogeneous or dynamic effects, it is biased, and the opening case is what that bias looks like in the wild.

Goodman-Bacon (2021, *Journal of Econometrics* 225(2):254–277) proved that the TWFE coefficient is a *weighted average of every possible 2×2 DiD comparison* between groups and time periods. Some comparisons are legitimate: newly treated versus not-yet-treated. But some use **already-treated units as controls for later-treated units** — the **forbidden comparisons**. When treatment effects change over time, those forbidden comparisons get the dynamic effect backwards, and the weights attached to them can be **negative**. You can have every single cohort's effect positive and a negative TWFE coefficient.

de Chaisemartin and D'Haultfœuille (2020, *AER* 110(9):2964–2996) proved the same pathology from a different angle: TWFE estimates a weighted sum of average treatment effects whose weights may be negative. And Sun and Abraham (2021, *Journal of Econometrics* 225(2):175–199) showed the event-study version of the bug — apparent pre-trends in a TWFE event study can be a pure artifact of treatment-effect heterogeneity, not evidence of a real parallel-trends violation. TWFE can not only flip the sign of your effect; it can manufacture a fake pre-trend that makes you abandon a valid design.

![Goodman-Bacon decomposition of a staggered TWFE estimate into three families of 2x2 difference-in-differences comparisons: newly-treated vs not-yet-treated (valid, positive weight), earlier vs later treated in the later cohort's pre-window (valid, positive weight), and later treated vs an already-treated control (forbidden, weight may go negative), which sum to the contaminated TWFE coefficient.](../images/10-staggered-did-message-variants-fig-01.png)

*Figure 10.1 — TWFE is a weighted average of 2×2 comparisons. The forbidden family — already-treated units used as controls — can carry negative weights and flip the sign of a real positive effect (Goodman-Bacon). The fix is Callaway–Sant'Anna, not a better TWFE.*

<!-- → [DIAGRAM: Goodman-Bacon decomposition visualization — three panels showing "newly treated vs not yet treated" (valid, positive weight), "earlier vs later treated" (valid, positive weight), and "later vs earlier treated" (forbidden — already-treated as control, can carry negative weight). Weights sum to the TWFE coefficient. Caption: "TWFE is a weighted average of 2×2 comparisons. Forbidden comparisons — already-treated units used as controls — can carry negative weights and flip the sign of a real positive effect."] -->

---

## What to use instead

The fix is to build comparisons by hand from clean 2×2 building blocks that never use already-treated units as controls.

**Callaway and Sant'Anna (2021, *Journal of Econometrics* 225(2):200–230) — the workhorse.** Define **group-time average treatment effects**, ATT(g, t): the effect for the cohort first treated at time *g*, evaluated at time *t*. Each ATT(g, t) is estimated by a clean 2×2 DiD against either never-treated or not-yet-treated units, using a doubly-robust estimand that allows parallel trends to hold conditionally on covariates. No forbidden comparisons, no negative weights by construction. You then aggregate into whatever summary you need: an overall ToT, a dynamic event-study path by exposure length, or an effect by cohort. Implemented in R's `did` package.

**Sun and Abraham (2021) — the interaction-weighted estimator.** Computes cohort-specific relative-time effects and aggregates with sample-share weights, recovering uncontaminated event-study coefficients. Use this when the dynamic path is the deliverable. Implemented as `fixest::sunab`.

**de Chaisemartin and D'Haultfœuille — `did_multiplegt` family.** Handles treatments that turn on *and off*. This matters here: message decks get rolled back and re-versioned. If your treatment is non-absorbing — a physician can revert to the old deck — Callaway–Sant'Anna's absorbing-treatment design no longer fits.

**Goodman-Bacon (2021) — as diagnostic.** Run the decomposition to *show* how much of a naive TWFE estimate comes from forbidden comparisons. It is your teaching exhibit: it makes the bug visible before you switch estimators.

| Estimator | Use case | Key assumption | Handles non-absorbing treatment | R package |
| --- | --- | --- | --- | --- |
| Callaway–Sant'Anna | Workhorse ATT(g, t) estimation; aggregate to event study, cohort, or overall ToT | Conditional parallel trends; absorbing treatment | No (absorbing design) | `did` |
| Sun–Abraham | Uncontaminated event-study / dynamic path is the deliverable | Parallel trends; no anticipation | No (absorbing design) | `fixest::sunab` |
| de Chaisemartin–D'Haultfœuille | Treatments that turn on *and off* (re-versioned, rolled-back decks) | Parallel trends per switch | Yes | `did_multiplegt` |
| Goodman-Bacon | *Diagnostic only* — decompose a naive TWFE into its 2×2 comparisons and expose forbidden-comparison weight | (Decomposition identity, not an estimator) | n/a | `bacondecomp` |

*Table 10.1 — Choosing the right estimator. Goodman-Bacon is a diagnostic; the others are estimation strategies for different design structures. Naive TWFE on staggered adoption with heterogeneous effects is biased (Goodman-Bacon negative weights) — none of these columns is "run TWFE."*

---

## Falsification first — the chapter's discipline

The cohort-rollout design is valid *only if* timing is uncorrelated with physician prescribing dynamics. You test this three ways, in order, and you do it *before* you look at any treatment effect.

**Test 1 — independence.** Regress cohort assignment on pre-rollout prescribing and pre-rollout prescribing growth. A significant coefficient means cohort timing tracks physician characteristics — the rollout was not as-good-as-random — and the design is dead. Stop. Report the failure. The result "this design does not identify the message effect on this rollout" is the deliverable.

**Test 2 — pre-trend leads.** Estimate ATT(g, t) for pre-treatment periods (relative time below zero). They should be approximately zero. Two cautions apply. Use the clean estimator — Callaway–Sant'Anna or Sun–Abraham — so an apparent pre-trend is not just TWFE heterogeneity contamination. And heed Roth (2022, *AER: Insights* 4(3):305–322): pre-testing for parallel trends and then conditioning your analysis on having passed the test distorts inference, because the test is underpowered and selection-biased. Report the leads; do not treat flat leads as proof.

**Test 3 — honest sensitivity.** Rather than assuming parallel trends holds exactly, use **Rambachan and Roth (2023, "A More Credible Approach to Parallel Trends," *Review of Economic Studies* 90(5):2555–2591)**: bound how large a post-period violation could plausibly be, relative to the violations you actually observe in the pre-period, and report which conclusions survive. This converts "trust me, trends are parallel" into "here is the effect, and here is how much trend-violation it can absorb before the conclusion breaks." Implemented as `HonestDiD`.

One more test, carried from Chapter 4: **exclusion**. Cohort timing should affect prescribing only through the message. The threat is that early-trained reps also got better at selling generally. Defend by controlling rep tenure and baseline rep performance so the cohort effect is not just an early-rep skill effect.

The practitioner synthesis for this whole discipline is Roth, Sant'Anna, Bilinski, and Poe (2023, "What's Trending in Difference-in-Differences? A Synthesis of the Recent Econometrics Literature," *Journal of Econometrics* 235(2):2218–2244) — the single best current-state reference for anyone running a staggered DiD.

---

## The full run, including the dead ends

We run Project 1 on the synthetic rep-visit panel. The synthetic generator plants a known ground truth: three cohorts adopt the safety-first deck; reps not trained in the window are the never-treated control; the true ToT is a positive, dynamically growing lift in prescribing — small at 30 days, larger at 60 and 90. The dynamics are deliberate. That is exactly the condition under which TWFE breaks, so we can watch it break and fix it.

**Step 1 — Run TWFE on purpose.** With the known synthetic ground truth, run naive TWFE first. On data where every cohort's effect is positive, the TWFE coefficient returns attenuated, near zero, or outright negative. Do not believe it. Now you know what the opening-case analyst saw.

**Step 2 — Show why: the Bacon decomposition.** The honest move is not just to switch estimators but to exhibit the bug. Run the Goodman-Bacon decomposition (cleanest in R's `bacondecomp`) and report the share of total weight sitting on "Later vs Earlier Treated" — the forbidden comparisons. When the synthetic effects grow over time, those comparisons carry the negative sign that dragged the TWFE coefficient down. Now you can tell the brand team exactly where the negative number came from: "X percent of your estimate was built from comparisons that used already-treated physicians as a control, and those comparisons run backwards when the effect is still growing."

**Step 3 — Falsify before you estimate.** With the panel in hand, run the falsification regression: regress cohort assignment on pre-rollout prescribing and pre-rollout prescribing slope, controlling for specialty and territory. On the synthetic data (built clean), these coefficients should be non-significant — but you must check, every time, on every dataset. A significant slope coefficient means stop. If the falsification passes, inspect the pre-treatment event-study leads from the clean estimator in step 4. Only then proceed.

**Step 4 — Estimate correctly: Callaway–Sant'Anna group-time ATTs.**

```r
library(did)
att <- att_gt(
  yname        = "nrx",
  tname        = "month",
  idname       = "npi",
  gname        = "cohort_g",            # 0 = never-treated
  xformla      = ~ rep_tenure + baseline_nrx + territory,
  data         = panel,
  control_group = "notyettreated",
  est_method   = "dr"                   # doubly robust
)
es      <- aggte(att, type = "dynamic")  # event-study path
overall <- aggte(att, type = "group")    # overall ToT
```

The event study now shows flat pre-treatment leads and positive, growing lags — recovering the planted ground truth that TWFE destroyed. [verify magnitudes against synthetic data before publication]

**Step 5 — Attach the Honest-DiD sensitivity band.**

```r
library(HonestDiD)
sensitivity <- honest_did(es, type = "relative_magnitude", Mbarvec = c(0.5, 1, 1.5, 2))
plot_sensitivity(sensitivity)
```

The output: the ToT remains positive and significant as long as any post-period parallel-trends violation is no more than M times the largest violation observed before treatment. That is a number a biostatistician can argue with — which is the point.

**Step 6 — The ToT readout, stated honestly.**

Under conditional parallel trends — conditioning on rep tenure, baseline prescribing, and territory — and using never-treated reps as controls, the safety-first deck raised treated physicians' prescribing by [effect at 90 days, to be generated on synthetic data — verify], with effects growing over the 30/60/90-day windows. The estimate survives Honest-DiD relative-magnitude violations up to M = [value]. Naive TWFE returned [the attenuated or negative number]; a Bacon decomposition attributes [X percent] of that estimate to forbidden already-treated-as-control comparisons.

**The lesson.** The right estimator did not just improve precision — it changed the sign of the conclusion. The discipline that saved you was running the falsification and the decomposition before believing the headline number.

**The limit.** The ToT is the effect on the treated physicians under parallel trends on this scale for the consenting, logged sampling frame. It is not a population effect, it is not assumption-free, and it is not a license to maximize prescribing volume. The sampling frame is shaped by consent opt-out — privacy-conscious physicians who opt out may prescribe differently, creating a selected sample whose parallel trends must be separately defended. The pathway question — whether the effect ran through education, reciprocity, or relationship — is not answered here and is Chapter 11's job. A total-effect ToT without pathway decomposition should not drive targeting decisions whose regulatory defensibility depends on which pathway moved the needle.

**One dead end to name explicitly.** A tempting "improvement" is to add in-visit engagement — dwell time, reaction score — to the conditioning set "to control for how engaged the physician was." Do not. Those variables are caused by the very message you are studying: they are post-treatment colliders. Conditioning on them inside the DiD reopens a closed path and corrupts the message-to-prescribing estimate. This is the collider discipline from Chapter 6, now operating inside Project 1. Chapter 11 makes it the central worked example.

![Event-study coefficient plot over relative event time with a visible zero line. The naive TWFE path (dashed) shows spurious sloping pre-trend leads and artificially negative early lags; the Callaway-Sant'Anna path (solid) shows flat leads near zero and a positive growing staircase of lags that recovers the planted synthetic ground truth shown as a horizontal reference.](../images/10-staggered-did-message-variants-fig-02.png)

*Figure 10.2 — The same data, two estimators. TWFE reverses the sign of a real positive effect; the clean estimator recovers the planted ground truth. This is an event-study plot of signed coefficients around a zero line, not a zero-baseline bar chart — and the falsification test runs before any of these coefficients is believed.*

<!-- → [DIAGRAM: Event-study plot with two overlapping lines — TWFE (dashed, showing artificially negative early lags and spurious pre-trends) and Callaway–Sant'Anna (solid, showing flat pre-treatment leads and growing positive post-treatment lags). Known synthetic ground truth marked as horizontal reference. Caption: "The same data, two estimators. TWFE reverses the sign of a real positive effect. The clean estimator recovers the planted ground truth."] -->

---

## The named artifact

Produce a notebook (or Python plus R pair) that, on the synthetic rep-visit panel:

1. Builds the physician-by-time panel with cohort time *g*, relative time, and exclusion controls (tenure, baseline prescribing, territory).
2. Runs naive TWFE and a Bacon decomposition; reports the forbidden-comparison weight and explains the resulting bias in one paragraph.
3. Runs the **falsification test first** — cohort-on-pre-prescribing regression and pre-treatment event-study leads — and states explicitly that the effect is interpreted only because falsification passed.
4. Estimates ATT(g, t) via Callaway–Sant'Anna (doubly robust, not-yet/never-treated control), aggregates to a dynamic event study and an overall ToT.
5. Attaches an Honest-DiD sensitivity band.
6. Closes with the ToT readout paragraph, with assumptions and sensitivity stated, and confirms the method recovered the known synthetic ground truth while TWFE did not.

---

## What would change my mind

I would weaken the claim that Project 1 is the most feasible, highest-value study if, on real partner data, the cohort-on-pre-prescribing falsification fails routinely — if training-cohort timing turns out to be systematically correlated with physician prescribing dynamics because high-value territories are staffed by senior reps trained first. That would not kill DiD as a tool; it would demote this particular natural experiment from "flagship" to "conditional on finding a brand with logistics-driven rollout timing." A second revision: if message decks are revised so frequently and non-absorbingly that no clean absorbing-treatment definition survives, the Callaway–Sant'Anna design no longer fits, and the de Chaisemartin–D'Haultfœuille family becomes the primary estimator throughout.

## Still puzzling

How non-absorbing is a real message rollout in practice? Decks get re-versioned. Reps informally revert to old slides on difficult calls. The "treatment is on" assumption that Callaway–Sant'Anna requires is a modeling choice, and it may be a poor one for real field data. What is the right treatment representation for "deck version 3.2 versus 3.1 versus 2.0"?

What is the correct outcome scale? Prescribing counts are bounded and skewed. Parallel trends in counts and parallel trends in log counts are different assumptions, and for a drug entering a market they can disagree sharply. The functional-form choice is load-bearing and under-discussed in pharma analytics.

---

## Exercises

**Warm-up**

1. *(Recall — tests the estimand)* Explain the difference between the average treatment effect (ATE) and the average treatment effect on the treated (ATT). Why is the ATT the right estimand for a message-variant study on rep-visit data? *What this tests: whether you can state the estimand precisely rather than defaulting to "the effect of the message."*

2. *(Recall — tests the TWFE bias)* Explain, in plain language a brand manager would follow, how a staggered TWFE regression can return a negative coefficient when every cohort's effect is positive. Name the specific mechanism — what comparison creates the negative weight? *What this tests: whether you understand the forbidden-comparison problem mechanically, not just by name.*

3. *(Recall — tests the three falsification steps)* Name the three falsification checks the chapter requires before interpreting any treatment effect, and for each state the specific result that would kill the design. *What this tests: whether you can recite the discipline in order.*

**Application**

4. *(Apply — TWFE and decomposition)* On the synthetic panel, run the naive TWFE regression and the Bacon decomposition. Report: (a) the TWFE coefficient; (b) the share of total weight on forbidden comparisons; (c) two sentences explaining, mechanically, why that share produced the coefficient you observed. *What this tests: whether you can exhibit the bias, not just assert it.*

5. *(Apply — Callaway–Sant'Anna)* Re-estimate with Callaway–Sant'Anna (doubly robust, never-treated control, conditioning on rep tenure, baseline prescribing, and territory). Produce the event-study plot. Compare the overall ToT to the TWFE coefficient and to the planted synthetic ground truth. By how much and in which direction did TWFE mislead? *What this tests: whether the method recovers truth on data whose answer is known.*

6. *(Apply — collider trap)* Deliberately add `Duration_vod` (dwell time) and `Reaction_vod` to the conditioning set, re-estimate, and observe how the ToT moves away from the synthetic ground truth. Write the one-paragraph memo explaining why "controlling for engagement" corrupted the estimate, in language that connects back to Chapter 6. *What this tests: whether you can apply the collider discipline inside a live estimation context.*

**Synthesis**

7. *(Synthesize — falsification + estimation)* Connect the falsification discipline to the estimand. Explain why a passed falsification test does not *prove* parallel trends holds, and why an Honest-DiD sensitivity band is the right response to that limitation rather than a stronger pre-test. Your explanation should reference Roth (2022) and Rambachan–Roth (2023). *What this tests: whether you understand the limits of falsification as well as its necessity.*

8. *(Synthesize — non-absorbing treatment)* A colleague discovers that two of the three training cohorts partially reverted to the old deck after three months because reps found the new deck harder to deliver on short calls. Explain why the Callaway–Sant'Anna design no longer fits, which alternative estimator is appropriate and why, and what additional data you would need to re-estimate cleanly. *What this tests: whether you can diagnose a design mismatch and prescribe the right fix.*

**Challenge**

9. *(Challenge — produce the named artifact)* Build the full falsification-first Project 1 pipeline on the synthetic panel: the Bacon decomposition showing TWFE bias, the falsification regression and event-study leads with explicit pass/kill reporting, the Callaway–Sant'Anna estimation with doubly-robust conditioning, the Honest-DiD sensitivity band, and the ToT readout paragraph with assumptions and sensitivity stated. Confirm that the method recovers the planted ground truth and TWFE does not. *What this tests: whether you can execute the full discipline end to end as a complete, reproducible artifact.*

---

## Prompts

### Figure 10.1 — Goodman-Bacon decomposition of TWFE into 2×2 comparisons

Generate a single self-contained HTML file (inline CSS, D3 7.9.0 from the cdnjs CDN) rendering a three-panel process/comparison diagram. Each panel is a horizontal pre/post timeline pair: Panel A "newly treated vs not-yet-treated" (valid, positive weight); Panel B "earlier vs later treated, later's pre-window" (valid, positive weight); Panel C "later treated vs ALREADY-treated control" (forbidden, weight may go negative), with the already-treated control row colored in the single red data accent and a blocked terminator. Treated rows split gray (pre) then ink (post) at a dashed onset line; each panel carries a small +w / -w weight tag. Three convergence arrows feed a summation node, then one arrow to a "combined TWFE / contaminated mix" box. Brutalist style: white background, ink #2a1a0e, one red series #C8102E, gray hairlines, no gradients, rx=0 rects, EB Garamond title / Inter body / JetBrains Mono ticks (full font chains, CSS variables, light+dark). Marks: rects for timeline blocks, lines with an arrowhead marker for flow. Sort panels valid-then-forbidden top to bottom. Not a quantitative chart — no axes. Annotate the negative-weight caveat in the caption. Deliverable: one HTML file.

### Figure 10.2 — Two-estimator event-study overlay (TWFE vs Callaway–Sant'Anna)

Generate a single self-contained HTML file (inline CSS, D3 7.9.0 from the cdnjs CDN) rendering an event-study line chart of ATT coefficients over relative event time (x: ordered points -4,-3,-1,1,3,5; y: signed coefficient with an explicit zero line — NOT a zero-baseline bar chart). Two series: naive TWFE (dashed gray, spurious sloping leads and artificially negative early lags) and Callaway–Sant'Anna (solid red, flat leads near zero, growing positive lags). Add a horizontal dashed "planted truth" reference line and a vertical dashed onset guide at relative time 0. Channels: x=relative time (point scale), y=coefficient (linear, includes zero), color/dash=estimator. Square point markers, interactive (tabindex, aria-label, Enter/Space, tooltip position:absolute pointer-events:none), gridlines aria-hidden. Brutalist palette and full font chains via CSS variables (light+dark), margins top48/right40/bottom56/left64, ResizeObserver redraw, reduced-motion guard, inline FALLBACK_DATA literal. Caption notes magnitudes are inferred and the falsification test precedes belief. Deliverable: one HTML file.

---

## Chapter 10 Exercises: Staggered Difference-in-Differences on Message-Variant Transitions

**Project:** The Causal Interview Bot
**This chapter adds:** The bot's elicited priors inform the DiD design itself — which message variants to compare, which controls satisfy exclusion, and what to falsify first — before a single ATT(g, t) is estimated.

### Exercise 1 — When to Use AI

You have a physician-by-month panel and a staggered message-deck rollout, and you have already run the Causal Interview Bot from Chapter 8 to elicit rep priors. Three places where reaching for an LLM is the right move, and the discipline that makes each one safe.

- **Task A.** Have the model scaffold the `did` package call — `att_gt` with a doubly-robust estimator, not-yet-treated control, conditioning on rep tenure, baseline prescribing, and territory — and the `aggte` aggregations to a dynamic event study and overall ToT. *Why AI works here:* the API surface of `did` is stable and documented; you can run the code on the synthetic panel and confirm it recovers the planted ground truth, so a wrong call fails loudly rather than silently.
- **Task B.** Ask the model to translate the Goodman-Bacon decomposition output into one plain-language paragraph a brand manager would follow — "X percent of your estimate came from already-treated-as-control comparisons that run backwards while the effect is still growing." *Why AI works here:* you hold the decomposition numbers; the model only rewords them, and you can check the rewording against the table line by line.
- **Task C.** Feed the model the bot's elicited prior — the ranked list of which variant transitions reps believe moved prescribing — and ask it to draft the *falsification order*: which cohort-on-pre-prescribing regression to run first and which exclusion threat (early-trained reps being better sellers generally) to defend first. *Why AI works here:* the prior is the input you supply and own; the model is sequencing checks you will still execute and judge.

**The tell:** in every case you can independently evaluate the output — recover ground truth, check a number against a table, run a regression you ordered. If you could not verify it, it would not belong here.

### Exercise 2 — When NOT to Use AI

Three judgments the model must not make, no matter how fluent it sounds.

- **Task A.** Do not ask the model whether parallel trends holds on *your* rollout. *Why AI fails here:* this is a causal-ID judgment about whether cohort timing was driven by training logistics or by physician prescribing dynamics — a ground-truth fact about how the rollout actually happened that lives in the field, not in the schema. The model will pattern-match to "trends look parallel" from the flat leads, which Roth (2022) shows is exactly the underpowered, selection-biased inference you must not lean on.
- **Task B.** Do not let the model "improve" the conditioning set by adding in-visit engagement — dwell time, reaction score. *Why AI fails here:* it ranks variables by predictiveness, and these are post-treatment colliders caused by the very message under study. Including them reopens a closed path. This is a values-and-structure call the SCM decides, not a feature-importance ranking. (LLM-suggestibility: the model will offer the collider as a "useful control" because it correlates with the outcome.)
- **Task C.** Do not let the model decide whether the recovered ToT licenses a targeting decision. *Why AI fails here:* the ToT is a total effect with no pathway decomposition; whether it should drive volume is a values/consent judgment the model cannot weigh.

**The tell:** if the AI is functioning as the *reason* you believe the design is valid, stop — it must be a *tool* that drafts and rewords while you supply the domain determination. **Series connection:** Tier **T4 (Bounded Reliability)** — the DiD machinery is reliable to scaffold and verify against synthetic ground truth, but the load-bearing identification judgment (did logistics or physician characteristics drive cohort timing) is yours and cannot be delegated.

### Exercise 3 — LLM Exercise

**What you're building:** a falsification-ordered DiD design memo for the message-variant rollout, with the Causal Interview Bot's elicited prior wired in as the input that decides which variants and controls to test first.

**Tool:** Claude, run as a **Claude Project**. Use a Project (not a one-off chat) because the bot you built in Chapter 8 — its elicited priors, its variant taxonomy, its rep-knowledge structure — is persistent context you will carry across Chapters 10–13; loading it once into the Project knowledge base means every later chapter's prompt inherits it without re-pasting.

**The Prompt:**

```
You are helping me design a staggered difference-in-differences study on a
pharma message-deck rollout. I have a physician-by-month panel with columns:
npi, month, nrx, cohort_g (treatment-start month; 0 = never treated),
territory, rep_tenure, baseline_nrx. The deck shifted from efficacy-first to
safety-first messaging, rolled out across three training cohorts (West Jan,
Midwest Feb, Southeast Mar) because of training logistics.

From my Causal Interview Bot I have elicited this rep prior: reps believe the
safety-first deck moved prescribing most for mid-tier physicians with an
unresolved renal-dosing concern, moved nothing for high-volume loyalists, and
may have BACKFIRED with two industry-skeptical accounts. Reps also flagged that
early-trained (West) reps were the most senior and best sellers overall.

Do five things:
1. Using the rep prior, tell me which cohort comparison and which subgroup I
   should be most suspicious of for an exclusion violation, and why.
2. Give me the falsification ORDER: which check to run first, second, third,
   and for each the specific result that kills the design. Include the
   cohort-on-pre-prescribing-slope regression and the pre-treatment event-study
   leads.
3. Give me R code using the `did` package: att_gt with a doubly-robust
   estimator, a not-yet-treated control group, conditioning on rep_tenure +
   baseline_nrx + territory, then aggte to a dynamic event study and an overall
   ToT.
4. List every variable I should NOT condition on and label each as a
   pre-treatment confounder I kept or a post-treatment collider I excluded.
5. Tell me what the rep "backfire" prior implies for how I should read a
   negative lag for those two accounts — and why I must not treat that prior as
   evidence the design is valid.

Do not tell me parallel trends is satisfied. Do not adjudicate whether the
effect should drive targeting. Flag anything you are unsure of as [verify].
```

**What this produces:** a design memo that orders falsification before estimation, names the exclusion threat the rep prior surfaced, gives runnable `did` code, and an explicit kept/excluded variable ledger — the front half of the Project 1 named artifact.

**How to adapt:** swap in your own synthetic panel's column names; if you use ChatGPT or Gemini instead, paste the bot's elicited prior inline at the top of the prompt rather than relying on Project knowledge (those tools lack the persistent bot context, so you re-supply it each session). Keep a Claude Project if you are doing Chapters 10–13 in sequence.

**Connection to previous chapters:** the elicited prior comes straight from the Chapter 8 Causal Interview Bot; the collider-exclusion ledger applies Chapter 6's discipline inside a live DiD; the estimand (ToT, not ATE) is Chapter 4's.

**Preview of next chapter:** Chapter 11 takes the same kept/excluded ledger and makes it the central worked example — you will specify the full SCM *before* any DML or causal forest runs, so the bot's prior stops being advice and becomes the graph that chooses the controls.

### Exercise 4 — CLI Exercise

**What you're building:** a falsification-first DiD pipeline as a runnable script with a test suite that asserts the method recovers the synthetic ground-truth ToT and that naive TWFE does not.

**Tool:** Claude Code — because this is a read-write-test loop across a repo (data, script, tests, a `make verify` target), which is exactly what an agentic CLI does well and a chat cannot. · **Skill level:** intermediate (you should be able to read R/Python and a pytest assertion).

**Setup:**
- [ ] A repo containing the synthetic rep-visit panel (synthetic only — real CRM telemetry is partner-proprietary and behind the firewall).
- [ ] R with the `did`, `bacondecomp`, and `HonestDiD` packages, or the Python equivalents, installed.
- [ ] The known planted ground-truth ToT recorded somewhere the test can read it.

**The Task:**

```
Read the synthetic physician-by-month panel in data/synthetic_panel.csv and the
ground-truth ToT in data/ground_truth.json. Do NOT touch anything in private/ or
any file matching *_real_* — those are proprietary and out of scope.

Write a single runnable pipeline script that, in order:
1. runs naive TWFE and a Goodman-Bacon decomposition, printing the
   forbidden-comparison weight share;
2. runs the falsification regression (cohort on pre-rollout prescribing slope,
   controlling for specialty and territory) and prints PASS/KILL;
3. estimates ATT(g, t) via Callaway-Sant'Anna (doubly robust, not-yet-treated
   control, conditioning on rep_tenure + baseline_nrx + territory) and
   aggregates to an event study and overall ToT;
4. attaches an Honest-DiD relative-magnitude sensitivity band.

Then write pytest tests that ASSERT (a) Callaway-Sant'Anna recovers the
ground-truth ToT within its confidence interval, and (b) naive TWFE does NOT
recover it (the failure is the teaching exhibit, not a bug). Wire a `make verify`
target that runs the pipeline and the tests and prints a pass/fail table.

Stopping condition: `make verify` runs green with both assertions passing. Do not
hard-code the estimate to match ground truth — read it from the estimator output.
After it passes, print which lines of the script choose the conditioning set, so
I can confirm no post-treatment variable entered.
```

**Expected output:** a pipeline script, a passing test file, a `make verify` target, and a printed pass/fail table showing CS recovers truth and TWFE fails.

**What to inspect:** the conditioning-set lines (confirm dwell/reaction never entered), and that the ground-truth value is *read*, not pasted into the assertion to force a pass.

**If it goes wrong:** the agent may make the TWFE assertion pass trivially by loosening tolerance until everything "recovers." Recovery: require the TWFE test to assert a *signed* miss (TWFE estimate falls outside the CI on the wrong side), so a loose tolerance cannot satisfy both tests at once.

**CLAUDE.md note:** add a line — "Synthetic data only; never read or write `private/` or `*_real_*`. Conditioning sets are chosen by the SCM, never by feature importance; dwell time and reaction score are post-treatment colliders and must never enter a DiD or DML control set."

### Exercise 5 — AI Validation Exercise

**What you're validating:** an AI-produced DiD analysis of the message-variant rollout — either your Exercise 3/4 output, or a deliberately flawed pre-generated artifact that ran naive TWFE on the staggered data, reported the attenuated/negative coefficient as the headline, and skipped the pre-trends check.

**Validation type:** ground-truth recovery plus design-discipline audit. **Risk level:** **high** — because a fluent TWFE readout can ship a sign-flipped conclusion (a recommendation to roll back a deck that actually worked) and nothing on the dashboard flags it.

**Setup:** take the Exercise 3 memo or Exercise 4 pipeline output (or the flawed naive-TWFE artifact) and run it against the checklist below on the synthetic panel where the planted ToT is known.

**The Validation Task:**

```
Validation Checklist — Chapter 10 (Staggered DiD)
Mark each Pass / Fail / Cannot-determine.

[ ] Correctness: does the reported ToT recover the planted synthetic
    ground-truth ToT within its confidence interval?
[ ] Completeness: are all three falsification checks present (independence,
    pre-trend leads, Honest-DiD sensitivity) and reported BEFORE the effect?
[ ] Scope: is the estimand stated as ToT (not ATE), and is the readout limited
    to the consenting, logged sampling frame?
[ ] Chapter-specific 1 — estimator: was Callaway-Sant'Anna (or an appropriate
    clean estimator) used, and is TWFE shown only as a diagnostic exhibit?
[ ] Chapter-specific 2 — conditioning set: is every control pre-treatment, with
    no dwell time or reaction score in the adjustment set?
[ ] Failure-mode check (fluent-but-wrong): does the artifact present a naive-TWFE
    coefficient as the headline, OR treat flat pre-trend leads as proof that
    parallel trends holds (failed-pre-trends reasoning)? Either is a FAIL even if
    the writeup reads cleanly.
[ ] Ground truth: is the conclusion anchored to recovery of the planted truth,
    not to plausibility?
```

**What to do with findings:** if everything passes, the analysis is shippable as a Level-3 (quasi-experimentally identified) finding pending real-data replication. One fail — fix it and re-run before believing the number. Multiple fails, especially Correctness plus the failure-mode check — discard the headline; the artifact is confidently wrong, which is the chapter's whole warning.

**AI Use Disclosure prompt:** *Write two sentences naming exactly what an AI tool did in your DiD work and the one judgment you supplied that it could not — specifically whether the falsification test passed on your rollout, a determination about whether cohort timing was driven by logistics rather than physician characteristics that the model cannot assess and you must defend from how this rollout actually happened.* (Mandatory.)

**Series connection:** the failure mode here is **naive-TWFE / failed pre-trends** — a fluent estimate with the wrong sign and no ground-truth anchor. Tier **T4 (Bounded Reliability)**: the estimator is reliable to run and verify, but the identifying judgment behind it is not the model's to make.
