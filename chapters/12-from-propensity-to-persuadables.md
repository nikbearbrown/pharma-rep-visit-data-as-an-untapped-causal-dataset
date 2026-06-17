# Chapter 12: From Propensity to Persuadables

## 1. Overview

Every chapter before this one was about *estimating* an effect. This chapter is about *spending money* on it.

You have, by now, a per-physician causal effect — the CATE τ(x) the causal forest produced in Chapter 11, or the individual counterfactual the Living Model's SCM produces in Chapter 9. The question this chapter answers is the one the brand actually asks: *given a fixed rep budget, which physicians do we detail, with what message, at what intensity?*

The naive answer — "detail the physicians most likely to prescribe" — is the answer every Next-Best-Action (NBA) engine in pharma gives. It is wrong in a specific, expensive, and fixable way. The physicians most likely to prescribe are disproportionately the ones who *would prescribe anyway*. Spending on them buys conversions you already had. This chapter teaches you to rank physicians by **causal responsiveness** — by how much a message *causes* a change — rather than by baseline volume, and to turn that ranking into an allocation under real constraints.

By the end you will be able to separate sure-things from persuadables, critique a propensity-ranked target list and say precisely what it wastes, and produce a causal-responsiveness ranking with a constrained allocation that you can defend with a Qini curve. You will also have stated — and this is the load-bearing claim of the chapter — the *testable* hypothesis that the Living-Model target list differs systematically from the NBA list.

## 2. Learning objectives

- **(Analyze)** Distinguish sure-things (waste) from persuadables using the four-quadrant uplift taxonomy, and locate each quadrant on the (propensity, responsiveness) plane.
- **(Evaluate)** Critique a propensity-ranked NBA target list — name what fraction of its budget is structurally wasted and why a better-fitting propensity model wastes *more*.
- **(Create)** Produce a causal-responsiveness ranking from per-physician CATEs and solve the constrained allocation, then score it against the NBA list with a Qini curve.

## 3. Opening case (failure-first)

A brand team runs an NBA engine. It is a good one — a well-tuned gradient-boosted model that predicts, for each physician, the probability she writes the drug next quarter. The engine ranks physicians by that probability and hands the reps a call list topped by the highest-propensity names.

Dr. Alvarez is at the top. She is a high-volume specialist, already a loyal prescriber, decile 10. The reps detail her four times this quarter. She prescribes — a lot, as she always has. The engine's dashboard lights up green: *targeted physicians converted at 94%.* The team celebrates the model's accuracy.

Here is what actually happened. Dr. Alvarez would have prescribed at essentially the same rate with *zero* visits. The four details caused nothing. The budget that detailed her — rep hours, samples, a lunch — produced no incremental prescription. The 94% conversion rate is not evidence the model worked; it is evidence the model selected people who were going to convert regardless. The metric looks magnificent *precisely because the target was guaranteed*.

And the cruelty of it: the *better* the propensity model gets, the more efficiently it finds the people who need no contact. A perfect propensity model would spend the entire budget on the physicians for whom detailing is most pointless. The engine is optimizing itself into futility.

Meanwhile, Dr. Okafor — decile 5, mid-tier, with a specific unresolved concern about renal dosing that the new safety deck happens to address — never makes the list. She is exactly the physician for whom the right message at the right time would *cause* a switch. The engine cannot see her, because the engine ranks on the wrong quantity. This chapter is about not making that mistake.

## 4. Core sections

### 4.1 Propensity is a Rung-1 question; persuadability is a Rung-2/3 question

Define the two quantities precisely, because the whole chapter turns on their difference.

**Propensity** is the answer to *who is most likely to prescribe?* — formally `P(NRx | X)`, the conditional probability of prescribing given physician features X. It is an observational, Rung-1 quantity (Pearl's ladder, Chapter 3). It is exactly what an NBA engine ranks on, whether it predicts prescribing directly or a proxy like email opens.

**Persuadability** is the answer to *for whom does the message* ***cause*** *a change?* — formally the conditional average treatment effect, **τ(x) = E[Y(1) − Y(0) | X = x]**, the difference between a physician's prescribing if treated and if untreated, given her features. This is a Rung-2/3 quantity. It is the *same per-physician number* the Chapter 11 causal forest produces.

These are not two estimates of the same thing. They are **two different orderings of the same population.** A physician can be high-propensity and low-persuadability (the loyalist who would prescribe regardless), or mid-propensity and high-persuadability (Dr. Okafor). The correlation between the two orderings is weak — and that weakness *is the opportunity.*

### 4.2 The four-quadrant uplift taxonomy

Cross a physician's response-if-treated against her response-if-not-treated and you get four kinds of physician. This taxonomy is standard in the uplift-modeling literature (Gutierrez & Gérardy 2017; the four labels are documented across the field — see the scikit-uplift documentation's "types of customers" for a clean secondary statement).

- **Persuadables** — prescribe *only if* treated. τ(x) > 0. **The entire ROI of detailing lives here.** This is the population you want.
- **Sure Things** — prescribe regardless. τ(x) ≈ 0 despite high `P(NRx | X)`. Targeting them is pure waste. *This is the propensity trap* — they are exactly who a propensity model over-selects, because their baseline volume is high.
- **Lost Causes** — never prescribe, treated or not. τ(x) ≈ 0, low baseline. Wasted contact.
- **Sleeping Dogs** (also "do-not-disturbs") — respond *negatively* to treatment. τ(x) < 0. Detailing them *reduces* prescribing — the industry-skeptical physician for whom a fourth visit triggers reactance. Targeting them actively backfires. (Note for Chapter 13: these physicians also disproportionately opt out of logging, which is why Sleeping Dogs and the consent collider are the same people seen from two angles.)

An NBA engine ranks by propensity, so it preferentially loads its list with **Sure Things** (high baseline) and is *blind to the sign* of the effect, so it cannot tell a Persuadable from a Sleeping Dog. A Living Model ranks by τ(x), so it selects **Persuadables** and *excludes* Sleeping Dogs. The lists differ. **That difference is the product.**

### 4.3 The canonical warrant: Ascarza (2018)

The misconception that propensity tells you whom to treat is not new to pharma, and it has been demolished in print. The strongest academic statement is **Ascarza (2018), "Retention Futility: Targeting High-Risk Customers Might Be Ineffective"** (*Journal of Marketing Research* 55(1): 80–98; DOI 10.1509/jmr.16.0163; 2018 Paul E. Green Award).

Ascarza ran two field experiments and showed that targeting customers by *churn risk* — the propensity to leave — is ineffective at retention, because **risk and responsiveness to a retention offer are distinct and only weakly correlated.** The high-risk customers were not the ones the intervention could save. The right target is *heterogeneity in treatment responsiveness*, not the risk score.

The transfer to detailing is exact. Ranking physicians by prescribing *propensity* is the pharma analog of targeting high-churn-risk customers: you are ranking on the wrong quantity, and a better risk model does not fix it — it sharpens the error. Ascarza is the single strongest citation for this chapter's thesis. Assign it as the anchor reading; it carries cleanly from churn to detailing.

### 4.4 Allocation as constrained portfolio optimization

Knowing each physician's τ(x) is not yet a plan. Detailing is a **scarce resource** — rep hours, sample budget, meal budget, call-frequency caps — so the real problem is:

> Choose which physicians to treat, and at what intensity, to **maximize total caused prescribing subject to a budget/capacity constraint.**

This is a constrained optimization — knapsack-flavored — where the *value* of treating physician *i* is her CATE τ(x_i), the *caused* increment, not her predicted volume. Allocate budget to the highest *incremental* return per unit of resource, intensity proportional to marginal caused return, until capacity is exhausted.

Why call it "portfolio"? Because, as in asset allocation, you are distributing a fixed budget across units with heterogeneous expected returns and constraints, and you want the highest *risk-adjusted incremental* return, not the highest *level*. (The framing is borrowed from constrained-allocation methods in `finance-advances-in-financial-machine-learning.md` — HRP/Markowitz allocate by risk-adjusted financial return; here we allocate by causal return. Borrow the *intuition*, not the literal Markowitz math: financial portfolio theory assumes known returns and covariances, whereas our "returns" are *estimated causal effects with their own error and ethical constraints*.)

The constraints are not decorative. They include rep capacity, call-frequency caps, territory, channel preference, and — load-bearing, carried into Chapter 13 — the **patient-welfare gate** and the **educational-pathway-only** regulatory line. A high-CATE physician can be *excluded* if the effect that drives her τ runs through reciprocity (a meal) rather than education. So the objective is not "rank by CATE and detail the top." It is:

> Maximize caused, ***legally drivable*** prescribing under capacity.

### 4.5 The Qini curve: how you prove the list is better

A ranking is a claim. The way you *demonstrate* the claim — that your causal-responsiveness list beats the propensity list — is the **Qini curve** and its summary, the **Qini coefficient**.

The Qini curve plots cumulative *incremental* gain (the extra prescribing caused) as you treat the top-ranked fraction of physicians by predicted uplift, compared against a random-targeting diagonal. The Qini coefficient is the area between your curve and that diagonal — the uplift field's analog of AUC. (The measure traces to Radcliffe's direct-marketing uplift work; see Radcliffe 2007 `[verify exact title/vol/pages]`.)

A higher Qini means more caused prescribing per physician contacted. This is how you turn "my list is different" into "my list is *better*": run both lists against a held-out randomized slice, compute the Qini for each, and show the gap. **The metric is the argument.** Do not hand a partner a list; hand them a Qini curve.

## 5. Worked example on this dataset (process, including dead ends)

We work this on the synthetic rep-visit dataset (Chapter 2), which ships with *known ground truth* — we built the τ(x) structure, so we can check whether our ranking recovers the physicians who are actually persuadable. Where public data suffices, the meals project (Chapter 11, Open Payments) supplies real CATEs.

**Step 1 — Get the per-physician CATE.** Pull τ(x_i) for every physician from the Chapter 11 causal forest (meals, public data) or the Chapter 9 SCM counterfactual engine. Each physician now has two numbers attached: a predicted volume `P(NRx | X)` (propensity) and a τ(x_i) (persuadability).

**Step 2 — Rank by persuadability, not propensity.** Sort by τ(x_i). Watch the orderings diverge: the decile-10 loyalist sits near the *top* on propensity but near *zero* on τ (a Sure Thing); the decile-5 physician with a renal-dosing concern sits in the middle on propensity but *high* on τ for the safety message (a Persuadable). Same physicians, two rankings, low overlap.

**Step 3 — Solve the constrained allocation.** Select physicians by marginal caused return until rep capacity / budget is exhausted; set intensity proportional to marginal caused return; and *exclude* two groups outright — reciprocity-driven physicians (pathway gate) and negative-τ Sleeping Dogs.

**Step 4 — The first dead end.** A Fellow's instinct: rank by predicted NRx and "go where the volume is." On the synthetic data, do it, and measure caused prescribing at a fixed budget. The propensity list concentrates spend on Sure Things and produces *less* caused prescribing than the persuadables list at the *same* budget. You can see the waste because the ground truth tells you which physicians had τ ≈ 0. This is the chapter's central lesson made tangible.

**Step 5 — The second dead end.** A subtler error: rank by CATE *alone* and detail the top — ignoring the pathway gate. This ships a high-CATE target whose effect runs through a meal (reciprocity). The list looks optimal on caused prescribing and is *killed in Chapter 13* on compliance grounds. Ranking on τ is necessary, not sufficient; the gate is part of the objective.

**Step 6 — Compare the lists.** Quantify two things: the *overlap* between the propensity list and the persuadables list (it will be low), and the *Qini gap* (persuadables > propensity). The divergence is the research finding; the Qini gap is the proof.

**Lesson:** budget should chase the *increment*, not the *level* — and only the *legally drivable* increment. Propensity and persuadability are two different populations.

**Limit:** τ(x) is *estimated*, with uncertainty. Ranking on noisy CATEs can mis-rank physicians near the persuadable/sure-thing boundary, and the ranking step throws away the confidence intervals the causal forest gave you. Worse, Sleeping Dogs must be *excluded*, not merely deprioritized — and detecting a *negative* effect requires statistical power you may not have on thin public data. Full four-quadrant separation may only be reachable on richer (proprietary or synthetic) data, which ties directly to the firewall and Risk 1 in Chapter 13.

## 6. Common misconceptions

- **"A model that predicts prescribers well tells you who to detail."** No — propensity ≠ persuadability. The best-predicted prescribers are disproportionately Sure Things; ranking on them spends the most budget on physicians who needed no contact. (This is misconception #1 from the learner profile, and Ascarza is the warrant.)
- **"Higher conversion among targeted physicians means the targeting worked."** Conversion-among-targeted is confounded by selection: if you target Sure Things, conversion is high *because they were going to convert*. Only incremental gain over a control measures causation.
- **"Sleeping Dogs are just low-uplift physicians; deprioritize them."** No — they have *negative* τ. Detailing them reduces prescribing. They must be *excluded*, which requires a *signed* CATE; a propensity model (unsigned, baseline-only) is structurally blind to them.
- **"Rank by CATE and detail the top — that's the whole job."** No — the allocation is *constrained* (capacity, welfare, pathway). A high-CATE physician driven by reciprocity is excluded. Ranking is the input to optimization, not the output.

## 7. Evidence check + Consent/compliance & patient-welfare check

**Evidence check.**

- *Independent / peer-reviewed:* Ascarza (2018, *JMR*) — propensity is the wrong target; field-experimental. Gutierrez & Gérardy (2017, *PMLR* 67) — uplift framed as CATE estimation. The CATE machinery (Wager & Athey 2018; Athey-Tibshirani-Wager 2019, GRF; Chernozhukov et al. 2018, DML) is established and shared with Chapter 11.
- *Practitioner / secondary:* the four-quadrant taxonomy (scikit-uplift docs); Radcliffe & Surry (2011) on significance-based uplift trees and negative-uplift handling. The Qini measure (Radcliffe 2007) `[verify exact bibliographic details]`.
- *Hypothetical / contested:* **the claim that the Living-Model list differs *systematically* from the NBA list is a hypothesis, not a result.** It is *testable* (held-out Qini comparison) but unproven on this dataset. Present it as the research finding to be demonstrated, not as established fact. Whether HCP systems can *operationalize* causal responsiveness in production is emerging and lags prediction-based NBA workflows.
- *Do not cite as primary:* the Obama-2012 / Analyst Institute "persuadables" origin story is journalistic provenance, not a citable academic source. `[verify]`

**Consent/compliance & patient-welfare check.** This chapter *produces a target list*, which is the moment the analysis becomes an action with consequences for real patients. Three checks are mandatory, not optional:

1. **Patient-welfare gate.** The objective maximizes *caused, legally drivable* prescribing — but a caused prescription is only a good outcome if it is the *right drug for that patient*. The allocation must not optimize prescribing volume in a way that is indifferent to clinical appropriateness. The welfare gate sits inside the objective.
2. **The pathway gate previews Chapter 13.** Excluding reciprocity-driven physicians is not a marketing nicety — it is the educational-pathway-only line. Carry it as a hard constraint.
3. **Sleeping Dogs ↔ opt-outs.** The negative-τ population overlaps the consent-collider population (the industry-skeptical physician). Targeting them not only backfires commercially; it pushes them toward opt-out, degrading the data and raising an ethical flag. This is the bridge into Chapter 13's consent architecture.

## 8. Named Fellow artifact

**Produce a causal-responsiveness ranking + allocation, and critique a propensity-ranked NBA list.**

Deliver three linked objects on the synthetic dataset (and, for the meals slice, on public Open Payments data):

1. **The ranking.** A table of physicians with columns for propensity `P(NRx | X)`, persuadability τ(x_i), its confidence interval, the inferred dominant pathway (educational / relationship / reciprocity), and the assigned quadrant (persuadable / sure-thing / lost-cause / sleeping-dog).
2. **The allocation.** The constrained solution: who is selected, at what intensity, under a stated budget/capacity — with the *excluded* sets named explicitly (reciprocity-driven and negative-τ) and the exclusion *reason* logged for each.
3. **The NBA critique.** Take the propensity-ranked list, identify the fraction of its budget landing on Sure Things, compute the Qini for both lists on a held-out slice, and write two paragraphs: *what the NBA list wastes, and why a better propensity model would waste more.*

The artifact is not the list. The artifact is the **Qini gap plus the exclusion log** — the evidence the list is better and the record of what the welfare/pathway gates removed.

## 9. Research finding

**The targeting list a Living Model produces differs systematically from the NBA propensity list — and the difference is measurable, productizable, and (so far) untested on partner data.**

Concretely, persuadables tend to be (a) **mid-tier prescribers**, not the top-volume Sure Things the NBA over-selects; (b) physicians with a **specific clinical concern** the right message addresses; and (c) in **markets peer influence has not yet reached** — pre-saturation, where a message can still move the decision rather than arriving after it is locked.

This is *falsifiable*: hold out a randomized slice, score both lists by realized incremental NRx (Qini), and look for divergence *and* Living-Model superiority. If the lists overlap heavily, or the NBA list's Qini matches, the thesis fails — and that would itself be a publishable finding. The persuadables *concept* and the Ascarza result are stable; the list-divergence *magnitude* must be re-tested on current partner data (a Chapter 13 / Risk 1 dependency).

## 10. Exercises

1. **(Analyze)** Given a 200-physician table with `propensity` and `tau` columns (synthetic data), assign each physician to one of the four quadrants using sensible thresholds. Report the counts. How many high-propensity physicians are Sure Things? Write one paragraph on what an NBA engine would do with this table and what it would miss.

2. **(Evaluate)** Take a propensity-ranked NBA target list (top 50 by `P(NRx | X)`). Using the ground-truth τ from the synthetic data, compute the total *caused* prescribing this list buys at a fixed budget. Then build the persuadables list (top 50 by τ, excluding negative-τ) and compute the same. Report the ratio, and explain in two paragraphs why a *more accurate* propensity model would make the NBA number *worse*.

3. **(Apply+ / produces artifact)** Build both target lists, solve the constrained allocation (budget = 80 visits, max 3 visits/physician), exclude reciprocity-pathway and negative-τ physicians, and **plot the Qini curve for both lists against random.** Submit the plot, the overlap percentage between the two lists, and the exclusion log. This is the Chapter artifact.

4. **(Create / Apply+)** Using `causalml` or `scikit-uplift`, fit an uplift model on the synthetic treated/control data, compute the Qini coefficient, and compare it to a two-model (T-learner) baseline. Write a short note on which physicians the two methods *disagree* about near the persuadable/sure-thing boundary, and how CATE uncertainty should change your ranking there.

## 11. When to Use AI

**When to use AI.** Use an LLM to *explain* the four-quadrant taxonomy in your own dataset's terms, to draft the boilerplate around a `causalml`/`EconML` pipeline (data loading, Qini plotting, train/control splits), and to *stress-test your framing* — ask it to argue that propensity is fine, then rebut it. Use it to translate Ascarza's churn result into detailing language for a partner audience.

**When NOT to use AI.** Do *not* let an LLM choose your quadrant thresholds, decide which pathway drives a physician's effect, or set the welfare/compliance exclusions — those are domain and ethical judgments with real-patient consequences. Do not trust an LLM's recollection of a Qini formula or a citation; verify against source. And never let it generate the *list itself* as if a recommendation were a calculation.

**LLM exercise (ready to paste).**

> *You are a skeptical marketing-science reviewer. I claim that ranking physicians by predicted prescribing probability (propensity) is the wrong way to allocate a pharma rep budget, and that I should rank by causal responsiveness (CATE / uplift) instead. (1) State the strongest version of the counterargument — the case for ranking on propensity. (2) Then rebut it, using the four-quadrant uplift taxonomy and the Ascarza (2018) "Retention Futility" result. (3) Identify one situation where propensity and persuadability would in fact be highly correlated, making my reframe less valuable. Be specific and concise.*

**CLI exercise.** Using Claude Code (or your CLI agent) on the synthetic dataset, ask it to: load the physician table, compute both rankings, solve the constrained allocation, and emit the Qini plot and overlap percentage as files. Then *read its code* and find the place where it silently conditioned on a post-treatment variable or dropped negative-τ physicians without logging them — and fix it. The point is to catch the agent's plausible-but-wrong allocation, not to accept its output.

**AI Validation.** Validate any AI-produced ranking three ways: (1) recover known ground truth on the synthetic data — does the AI's top decile match the true high-τ physicians? (2) check the Qini against a hand-computed value on a small slice; (3) audit the exclusion log — every excluded physician must have a logged reason (negative τ, reciprocity pathway, or capacity), not silent dropping.

## 12. AI Use Disclosure

Per the series bonus rule, your AI Use Disclosure for this chapter's artifact must name **one decision the AI could not make for you**: a quadrant threshold you set on domain grounds, a pathway attribution that required rep/structural knowledge no dataset supplied, or a welfare/compliance exclusion you made over the AI's recommendation. State the model used, what it drafted, and where you overrode it.

## 13. What would change my mind

I would abandon the persuadables reframe if, on a properly held-out randomized slice with adequate power, the propensity-ranked list's Qini *matched or exceeded* the causal-responsiveness list's — i.e., if propensity and persuadability turned out to be strongly correlated in this domain. Ascarza found them weakly correlated in churn; pharma detailing is a different generative process and the result is not guaranteed to transfer. I would also weaken the claim if CATE estimates proved too noisy to rank reliably near the persuadable/sure-thing boundary, such that the "different list" was mostly estimation noise rather than real structure. Both are *empirical* questions this chapter sets up but cannot close.

## 14. Still puzzling

- How should ranking *honor* the CATE confidence intervals the forest produces? Ranking on the point estimate throws away uncertainty; ranking on a lower confidence bound is more conservative but may systematically demote genuinely persuadable physicians with noisier estimates. There is no clean source for ranking-on-noisy-CATE in this domain. `[verify / author judgment]`
- Detecting Sleeping Dogs (negative τ) needs power that thin public data may not provide. Is partial four-quadrant separation (persuadable vs everything-else) honest enough to ship, or does excluding Sleeping Dogs *require* the richer proprietary/synthetic data — making the full reframe contingent on Risk 1?

## 15. Summary

Propensity answers *who will prescribe*; persuadability answers *for whom does a message cause prescribing*. They are different orderings of the same population, and the NBA engine ranks on the wrong one — loading its list with Sure Things who would prescribe anyway, blind to the sign that distinguishes a Persuadable from a Sleeping Dog. The fix is to rank by τ(x), the per-physician CATE, and to allocate a fixed budget as a constrained portfolio optimization that maximizes *caused, legally drivable* prescribing under capacity and welfare/pathway gates. You prove the new list beats the old one with a Qini curve, not a dashboard. Ascarza (2018) is the academic warrant; the systematic divergence of the two lists is the chapter's testable, productizable claim — and the bridge to the consent and compliance constraints that decide what can actually ship.

## 16. Key terms

- **Propensity** — `P(NRx | X)`, probability of prescribing given features; a Rung-1 quantity; what NBA engines rank on.
- **Persuadability / CATE** — τ(x) = E[Y(1) − Y(0) | X = x], the per-physician caused effect of a message; a Rung-2/3 quantity.
- **Four-quadrant uplift taxonomy** — persuadables (treat-only responders), sure-things (respond regardless), lost-causes (never respond), sleeping-dogs (respond negatively).
- **Sure-things problem** — propensity ranking over-selects high-baseline physicians whose treatment effect is ≈ 0; the core waste.
- **Sleeping Dogs / negative uplift** — physicians with τ(x) < 0; must be *excluded*, not deprioritized; requires a signed CATE.
- **Constrained portfolio optimization** — allocate a fixed budget across physicians by marginal *caused* return under capacity and compliance constraints.
- **Qini curve / coefficient** — cumulative incremental gain vs random as a function of treated fraction; the uplift analog of AUC; how list superiority is demonstrated.
- **Educational-pathway gate** — exclusion of reciprocity-driven physicians from the drivable target set (carried into Chapter 13).

## 17. Bridge

You now have a target list ranked by causal responsiveness, with a Qini that says it beats the propensity list, and an exclusion log that already gestures at constraints we have not yet justified: *why* reciprocity-driven physicians are excluded, *why* the negative-τ population overlaps the people who opt out of being measured at all, and *what* the welfare gate actually requires. Those are not afterthoughts to a finished model — they are the conditions under which any of this is allowed to ship. Chapter 13 makes them load-bearing: the consent mechanism as a selection collider, the regulatory line that makes the educational pathway the only legally drivable one, the IP firewall that keeps the partner's data out of the public book, and the handoff brief that turns all of it into a defensible test/build/kill recommendation. The list is the easy part. What you are permitted to do with it is the rest of the book.

## 18. Further reading

- Ascarza, E. (2018). "Retention Futility: Targeting High-Risk Customers Might Be Ineffective." *Journal of Marketing Research* 55(1): 80–98. DOI 10.1509/jmr.16.0163. — the propensity-is-the-wrong-target result; the chapter's anchor.
- Gutierrez, P. & Gérardy, J.-Y. (2017). "Causal Inference and Uplift Modelling: A Review of the Literature." *PMLR* 67: 1–13. — uplift as CATE estimation; meta-learner families.
- Radcliffe, N. J. & Surry, P. D. (2011). "Real-World Uplift Modelling with Significance-Based Uplift Trees." Stochastic Solutions / Portrait Technical Report TR-2011-1. — negative-uplift ("Sleeping Dogs") handling.
- Radcliffe, N. J. (2007). "Using control groups to target on predicted lift." *Direct Marketing Analytics Journal* (DMA). — origin of the Qini measure. `[verify exact title/vol/pages]`
- Wager, S. & Athey, S. (2018). "Estimation and Inference of Heterogeneous Treatment Effects using Random Forests." *JASA* 113(523): 1228–1242. — the CATE machinery underlying persuadability.
- O'Neil, C. *Weapons of Math Destruction* (`_lib_math-weapons-of-math-destruction-...md`). — the critical lens on optimizing the wrong target and opaque scoring; the moral counterweight to a confident propensity model.
- Software: `causalml` (Uber; Chen et al. 2020, arXiv:2002.11631), `EconML` (py-why), `scikit-uplift` — uplift modeling and Qini/AUUC evaluation.
