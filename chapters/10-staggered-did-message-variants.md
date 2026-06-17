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

<!-- → [DIAGRAM: Goodman-Bacon decomposition visualization — three panels showing "newly treated vs not yet treated" (valid, positive weight), "earlier vs later treated" (valid, positive weight), and "later vs earlier treated" (forbidden — already-treated as control, can carry negative weight). Weights sum to the TWFE coefficient. Caption: "TWFE is a weighted average of 2×2 comparisons. Forbidden comparisons — already-treated units used as controls — can carry negative weights and flip the sign of a real positive effect."] -->

---

## What to use instead

The fix is to build comparisons by hand from clean 2×2 building blocks that never use already-treated units as controls.

**Callaway and Sant'Anna (2021, *Journal of Econometrics* 225(2):200–230) — the workhorse.** Define **group-time average treatment effects**, ATT(g, t): the effect for the cohort first treated at time *g*, evaluated at time *t*. Each ATT(g, t) is estimated by a clean 2×2 DiD against either never-treated or not-yet-treated units, using a doubly-robust estimand that allows parallel trends to hold conditionally on covariates. No forbidden comparisons, no negative weights by construction. You then aggregate into whatever summary you need: an overall ToT, a dynamic event-study path by exposure length, or an effect by cohort. Implemented in R's `did` package.

**Sun and Abraham (2021) — the interaction-weighted estimator.** Computes cohort-specific relative-time effects and aggregates with sample-share weights, recovering uncontaminated event-study coefficients. Use this when the dynamic path is the deliverable. Implemented as `fixest::sunab`.

**de Chaisemartin and D'Haultfœuille — `did_multiplegt` family.** Handles treatments that turn on *and off*. This matters here: message decks get rolled back and re-versioned. If your treatment is non-absorbing — a physician can revert to the old deck — Callaway–Sant'Anna's absorbing-treatment design no longer fits.

**Goodman-Bacon (2021) — as diagnostic.** Run the decomposition to *show* how much of a naive TWFE estimate comes from forbidden comparisons. It is your teaching exhibit: it makes the bug visible before you switch estimators.

<!-- → [TABLE: Four estimators side by side — Callaway–Sant'Anna, Sun–Abraham, de Chaisemartin–D'Haultfœuille, Goodman-Bacon. Columns: use case, key assumption, handles non-absorbing treatment, R package. Caption: "Choosing the right estimator. Goodman-Bacon is a diagnostic; the others are estimation strategies for different design structures."] -->

---

## Falsification first — the chapter's discipline

The cohort-rollout design is valid *only if* timing is uncorrelated with physician prescribing dynamics. You test this three ways, in order, and you do it *before* you look at any treatment effect.

**Test 1 — independence.** Regress cohort assignment on pre-rollout prescribing and pre-rollout prescribing growth. A significant coefficient means cohort timing tracks physician characteristics — the rollout was not as-good-as-random — and the design is dead. Stop. Report the failure. The result "this design does not identify the message effect on this rollout" is the deliverable.

**Test 2 — pre-trend leads.** Estimate ATT(g, t) for pre-treatment periods (relative time below zero). They should be approximately zero. Two cautions apply. Use the clean estimator — Callaway–Sant'Anna or Sun–Abraham — so an apparent pre-trend is not just TWFE heterogeneity contamination. And heed Roth (2022, *AER: Insights* 4(3):305–322): pre-testing for parallel trends and then conditioning your analysis on having passed the test distorts inference, because the test is underpowered and selection-biased. Report the leads; do not treat flat leads as proof.

**Test 3 — honest sensitivity.** Rather than assuming parallel trends holds exactly, use **Rambachan and Roth (2023, "Honest DiD," *Review of Economic Studies*)**: bound how large a post-period violation could plausibly be, relative to the violations you actually observe in the pre-period, and report which conclusions survive. This converts "trust me, trends are parallel" into "here is the effect, and here is how much trend-violation it can absorb before the conclusion breaks." Implemented as `HonestDiD`.

One more test, carried from Chapter 4: **exclusion**. Cohort timing should affect prescribing only through the message. The threat is that early-trained reps also got better at selling generally. Defend by controlling rep tenure and baseline rep performance so the cohort effect is not just an early-rep skill effect.

The practitioner synthesis for this whole discipline is Roth, Sant'Anna, Bilinski, and Poet (2023, *Journal of Econometrics* 235(2):2218–2244) — the single best current-state reference for anyone running a staggered DiD.

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

**LLM exercise (copy-paste prompt):**

> "I have a physician-by-month panel: columns npi, month, nrx, cohort_g (treatment-start month; 0 = never treated), territory, rep_tenure, baseline_nrx. Treatment is a message-deck rollout staggered across training cohorts; I am not yet sure whether decks ever get rolled back. Do four things: (1) explain in plain language a brand manager would understand why a naive TWFE regression of nrx on a post-treatment dummy could return a NEGATIVE coefficient even if the deck helped every cohort — name the mechanism; (2) give me R code using the `did` package to estimate group-time ATTs with a doubly-robust estimator, a not-yet-treated control group, and conditioning on rep_tenure plus baseline_nrx plus territory, then aggregate to an event study and an overall ToT; (3) tell me which TWO checks I must run BEFORE I interpret the ToT, and what result on each would kill the design; (4) tell me what to do differently if decks DO get rolled back. Do not tell me parallel trends is satisfied — I will test that myself. List any variable in my schema I should NOT condition on, and why."

**CLI exercise.** In a repo containing the synthetic panel, ask Claude Code to: (a) write the falsification-first pipeline as a runnable script with pytest checks that *assert the method recovers the synthetic ground-truth ToT within tolerance* and *assert that naive TWFE does not*; (b) wire a `make verify` target that runs both and prints a pass/fail table. The validation artifact — "CS recovers truth, TWFE fails" — is the test suite itself.

**AI Validation exercise.** The check is recovery, not plausibility. The synthetic dataset has a known ground-truth ToT; your estimate is validated by recovering that number within its confidence interval. An estimate that "looks reasonable" but misses the planted truth is a failed validation. Confirm also that TWFE *fails* on the identical data — the failure is not a bug, it is the teaching exhibit.

**AI Use Disclosure**

*Write two sentences naming exactly what an AI tool did in your work for this chapter and what judgment you supplied that the AI could not. The judgment most specific to this chapter: whether the falsification test passed on your rollout's data — a domain determination about whether cohort timing was actually driven by logistics rather than physician characteristics, which the model cannot assess and which you must defend from knowledge of how this rollout actually happened.*

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
