# Chapter 12 — From Propensity to Persuadables
*Why the best-fitting model in pharma finds the physicians who needed no help — and what to rank on instead.*

Dr. Alvarez sits at the top of the list. She is a high-volume specialist, decile 10, a loyal prescriber who has been writing the drug for three years. The next-best-action engine ranked her first. The reps detailed her four times this quarter. She prescribed — a lot, as she always has. The campaign dashboard reports 94% conversion among targeted physicians. The team celebrates.

Here is what actually happened. Dr. Alvarez would have prescribed at essentially the same rate with zero visits. The four details caused nothing. The rep hours, the samples, the lunch — all of it produced no prescriptions that would not have been written anyway. The 94% conversion rate is not evidence the model worked. It is evidence the model found people who were going to convert regardless. The metric looks magnificent *precisely because the target was guaranteed*.

And the cruelty runs deeper than wasted budget. The better the propensity model gets — the more accurately it predicts who will prescribe — the more efficiently it concentrates the campaign on physicians who do not need it. A perfect propensity predictor would spend the entire detailing budget on physicians for whom detailing is completely pointless. The engine is optimizing itself into futility, and every improvement to the model accelerates the waste.

Dr. Okafor, meanwhile, does not make the list. She is decile 5, mid-tier volume, with a specific unresolved concern about renal dosing that the new safety deck happens to address precisely. She is the physician for whom the right message at the right moment would *cause* a switch. The engine cannot find her, because the engine ranks on the wrong quantity.

This chapter is about that distinction — and what to do about it.

---

Define the two quantities precisely, because the whole chapter turns on their difference.

**Propensity** answers: *who is most likely to prescribe?* Formally it is $P(\text{NRx} \mid X)$ — the conditional probability of prescribing given physician features. It is an observational quantity, Rung-1 on Pearl's ladder. It is what every next-best-action engine ranks on, whether it predicts prescribing directly or uses an engagement proxy. It tells you where the volume already is.

**Persuadability** answers: *for whom does the message cause a change?* Formally it is $\tau(x) = \mathbb{E}[Y(1) - Y(0) \mid X = x]$ — the conditional average treatment effect, the difference between a physician's prescribing if treated and if untreated, given her characteristics. This is a Rung-2 quantity. It is the same per-physician number the causal forest in Chapter 11 produces. It tells you where the increment lives.

These are not two estimates of the same thing. They are two different orderings of the same population. A physician can be high-propensity and low-persuadability — Dr. Alvarez, the loyalist who would prescribe regardless. Or mid-propensity and high-persuadability — Dr. Okafor, the physician with a specific concern the right content addresses. The correlation between the two orderings is weak. That weakness is the opportunity, and it has been demonstrated empirically in a setting close enough to detailing that the transfer is direct.

---

The strongest academic statement of what propensity-ranking costs comes from a 2018 paper in the *Journal of Marketing Research* by Eva Ascarza: "Retention Futility: Targeting High-Risk Customers Might Be Ineffective." Ascarza ran two field experiments and showed that targeting customers by churn risk — their propensity to leave — is ineffective at retention, because risk and responsiveness to a retention offer are distinct quantities and only weakly correlated. The high-risk customers are not the ones the intervention can save. The right target is heterogeneity in treatment responsiveness, not the risk score.

The transfer to pharmaceutical detailing is exact. Ranking physicians by prescribing propensity is the pharma analog of targeting high-churn-risk customers: you are ranking on baseline behavior, which tells you where the volume already lives, not where the message can cause a change. And a better propensity model does not fix this — it sharpens the error by more efficiently finding the Sure Things.

Ascarza won the Paul E. Green Award for methodological contribution to marketing research. Assign it as reading. The churn context is different from detailing; the statistical structure is identical.

<!-- → [DIAGRAM: Two-axis scatter plot. Horizontal axis: propensity P(NRx | X), low to high. Vertical axis: persuadability τ(x), negative to positive. Four quadrants labeled: upper-right "Persuadables (treat only if messaged)" — target; upper-left "Hidden Persuadables — missed by propensity ranking"; lower-right "Sure Things — the propensity trap, high baseline, zero increment"; lower-left "Lost Causes"; below horizontal axis (negative τ): "Sleeping Dogs — detailing reduces prescribing." Annotation: "NBA engine selects along the propensity axis. Living Model selects along the persuadability axis. Overlap is low." Caption: "The propensity axis and the persuadability axis order the same physicians differently. The gap between them is where budget is wasted."] -->

---

Cross a physician's response-if-treated against her response-if-not-treated and you get four kinds of physician. This taxonomy is standard in the uplift-modeling literature.

**Persuadables** prescribe only if treated — $\tau(x) > 0$ and baseline low. The entire return on detailing investment lives here. This is the population the campaign should find.

**Sure Things** prescribe regardless. Their $\tau(x) \approx 0$ despite high $P(\text{NRx} \mid X)$. Targeting them is pure waste. They are also, because their baseline volume is high, exactly who a propensity model preferentially selects. This is the propensity trap: the engine finds the people who need no help and calls it success.

**Lost Causes** never prescribe, treated or not. Low baseline, near-zero effect. Wasted contact, but at least benign.

**Sleeping Dogs** respond *negatively* to treatment — $\tau(x) < 0$. Detailing them reduces prescribing. The mechanism is reactance: the industry-skeptical physician for whom a fourth unsolicited visit triggers active resistance. Targeting them does not merely waste budget; it actively backfires. They must be *excluded* from the target list, not merely deprioritized — and detecting them requires a signed estimate of $\tau(x)$, not a propensity score, which is unsigned and cannot see the sign of the effect at all.

One structural note that bridges to the previous chapter: the Sleeping Dog population overlaps substantially with the consent-collider population. The industry-skeptical physician who reacts negatively to detailing is also the physician most likely to opt out of having her visits logged. The same person, seen from two different angles — the commercial problem and the measurement problem — is the same person.

An NBA engine ranks by propensity: it preferentially loads the list with Sure Things and is structurally blind to the sign of the effect. It cannot tell a Persuadable from a Sleeping Dog. A causal-responsiveness ranking sorts by $\tau(x)$: it selects Persuadables and excludes Sleeping Dogs. The lists differ. That difference is the product.

<!-- → [TABLE: Four-quadrant uplift taxonomy. Columns: quadrant, response if treated, response if untreated, τ(x), propensity, what happens if targeted, what NBA engine does. Rows: Persuadables (yes, no, >0, mid-tier, prescribing increases — this is ROI, misses them), Sure Things (yes, yes, ≈0, high, prescribing unchanged — pure waste, selects them preferentially), Lost Causes (no, no, ≈0, low, nothing, ignores), Sleeping Dogs (no, yes treated→reduced, <0, variable, prescribing decreases — backfires, cannot detect — propensity is unsigned). Caption: "The NBA engine is blind to the sign of the effect and over-selects Sure Things. Both failures have the same cause: ranking on the wrong quantity."] -->

---

Knowing each physician's $\tau(x)$ is not yet a plan. Detailing is scarce — rep hours, sample budget, call-frequency caps, travel time. The real problem is:

Choose which physicians to detail, and at what intensity, to maximize total *caused* prescribing subject to a budget and capacity constraint.

This is a constrained optimization. The value of treating physician $i$ is her CATE $\tau(x_i)$ — the increment she contributes — not her predicted volume. Allocate budget to the highest incremental return per unit of resource, set intensity proportional to marginal caused return, and continue until capacity is exhausted.

The framing borrows from portfolio theory: distributing a fixed budget across units with heterogeneous expected returns, under constraints, seeking the highest risk-adjusted incremental return rather than the highest level. The analogy is intuitive but the math is not Markowitz — our "returns" are estimated causal effects with their own uncertainty, and the constraints are not just capacity but also ethics and compliance.

Those constraints are not decorative. They are load-bearing, and they modify the objective in a specific way.

The first constraint is rep capacity and call-frequency caps — the logistical floor.

The second constraint is the **patient-welfare gate**. The objective maximizes caused prescribing, but a caused prescription is only a good outcome if it is the right drug for that patient. The allocation must not optimize prescribing volume in a way that is indifferent to clinical appropriateness. Whether the additional prescribing that the high-CATE physicians contribute represents genuine patient benefit — rather than extending prescribing beyond appropriate clinical indications — is not a question the CATE estimate answers. It is a question that requires clinical judgment, and it sits inside the objective function, not as an afterthought.

The third constraint is the **educational-pathway gate**. A physician's CATE might be high because the safety message genuinely updates her clinical knowledge — she was uncertain about a safety signal and the data resolved it. That effect runs through education. Or her CATE might be high because the fourth visit with a meal generates reciprocity, and the food creates an obligation to prescribe. That effect runs through relationship maintenance and social influence. The first pathway is legally drivable and regulatorily defensible. The second is not. A physician whose high $\tau(x)$ is driven by the reciprocity pathway is excluded from the deployable target list. Not deprioritized. Excluded.

The full objective is therefore: maximize caused, *educationally drivable*, *clinically appropriate* prescribing under capacity. That is a harder problem than ranking by CATE and detailing the top. It is also the only version of the problem that can actually ship.

---

Work through the mechanics on the synthetic dataset, where the ground truth is known.

Pull $\tau(x_i)$ for every physician from the causal forest produced in Chapter 11. Each physician now has two numbers attached: a predicted volume $P(\text{NRx} \mid X)$ and a persuadability $\tau(x_i)$.

Sort by $\tau(x_i)$. Watch the orderings diverge. The decile-10 loyalist sits near the top on propensity and near zero on $\tau$ — a confirmed Sure Thing. The decile-5 physician with a renal-dosing concern sits in the middle on propensity and high on $\tau$ for the safety message — a Persuadable. Same physicians. Two rankings. Low overlap.

The first dead end an analyst hits: rank by predicted NRx and "go where the volume is." On the synthetic data, do it and measure caused prescribing at a fixed budget. The propensity list concentrates spend on Sure Things and produces less caused prescribing than the persuadables list at the same budget. You can see the waste because the ground truth tells you which physicians had $\tau \approx 0$. The lesson is tangible: the propensity list bought a lot of conversions and caused very few of them.

The second dead end is subtler: rank by CATE alone and detail the top, ignoring the pathway gate. This produces a list that looks optimal on caused prescribing and gets killed on compliance grounds when the pathway analysis reveals that several of the high-$\tau$ physicians are responsive through relationship maintenance rather than clinical education. Ranking on $\tau$ is necessary, not sufficient. The gate is part of the objective.

<!-- → [DIAGRAM: Side-by-side comparison of two target lists for 200 physicians. Left panel: propensity-ranked NBA list. Physicians sorted by P(NRx | X). Background shading by τ(x) — most of the top-ranked physicians are in the "low τ / Sure Thing" region. Budget allocation annotated on top decile: "84% of budget, 3% of caused increment." Right panel: τ(x)-ranked persuadables list. Same population, sorted by CATE. Budget allocation annotated on top decile: "84% of budget, 61% of caused increment." Overlap percentage between the two lists labeled. Caption: "Same budget. Same physicians. Two rankings. The propensity list buys conversions. The persuadables list causes them."] -->

---

A ranking is a claim. You prove it with a **Qini curve**.

The Qini curve plots cumulative incremental gain — the additional prescribing caused — as you treat the top-ranked fraction of physicians by predicted uplift, compared against a random-targeting diagonal. It is the uplift analog of the ROC curve: where ROC measures discrimination between positive and negative outcomes, the Qini measures the efficiency of a causal ranking at concentrating caused prescribing in the physicians contacted first.

The Qini coefficient is the area between your curve and the random diagonal — summary statistic for how much caused prescribing your ranking concentrates. A higher Qini means more caused prescribing per physician contacted. A Qini of zero means your ranking is no better than random selection. A propensity list's Qini on *caused* prescribing is typically low — it is good at finding converters but those converters do not need the contact.

This is how you turn "my list is different from the NBA list" into "my list is better": run both lists against a held-out randomized slice, compute the Qini for each, and show the gap. The Qini gap is the argument. Do not hand a partner a list; hand them a Qini curve with the NBA list plotted alongside.

The limit of this demonstration is worth stating plainly. $\tau(x)$ is estimated, with uncertainty. Ranking on noisy CATEs can mis-rank physicians near the persuadable/sure-thing boundary — physicians with $\tau \approx 0$ who appear high or low by chance. The causal forest returns confidence intervals; the ranking throws them away. More seriously, detecting Sleeping Dogs — physicians with negative $\tau$ — requires statistical power that may not be available in thin public data. On public data, full four-quadrant separation may not be achievable; partial separation (persuadable versus everything else) may be the honest frontier. Whether the full reframe requires the richer proprietary data across the firewall is an open question this chapter sets up but cannot close.

---

The claim this chapter leaves on the table — the one that must be tested, not asserted — is that the Living Model's target list differs *systematically* from the NBA propensity list, and that the difference is detectable with a held-out Qini comparison.

Ascarza found propensity and persuadability weakly correlated in churn. Pharma detailing is a different generative process: the treatment is a human interaction rather than a discount offer, the outcome is a professional clinical decision rather than a consumer subscription, and the selection pressures that correlate baseline volume with effect size may run differently here. The weak-correlation result is not guaranteed to transfer. It might be stronger than churn; it might be weaker. The Qini experiment on partner data answers the question, and the answer belongs in the deliverable.

Persuadables tend to be mid-tier prescribers, not the top-volume Sure Things the NBA over-selects. They tend to be physicians with a specific unresolved clinical question that the right message addresses. They tend to be in markets that peer influence has not yet saturated — pre-saturation, where a message can still move a decision rather than arriving after it is locked. These are hypotheses about the structure of the persuadable population, and each is falsifiable on data.

If the Qini gap turns out to be small — if propensity and persuadability are more correlated in detailing than in churn — that would itself be a publishable finding. The persuadable concept and the Ascarza result are stable; the list-divergence magnitude must be measured on current partner data, and the book's claim is not that the magnitude is large, but that the question has not been asked and deserves to be.

**Five-part AI exercise block**

**When to use AI here.** Use an LLM to explain the four-quadrant taxonomy in the language of your specific dataset, to draft the boilerplate around a `causalml` or `EconML` pipeline — data loading, train/control splits, Qini plotting — and to stress-test the propensity-versus-persuadability argument. Ask it to make the strongest case for propensity ranking, then rebut it. Use it to translate Ascarza's churn result into detailing language for a partner audience.

**When NOT to use AI here.** Do not let an LLM choose your quadrant thresholds, decide which pathway drives a physician's effect, or set the welfare and compliance exclusions. Those are domain and ethical judgments with real-patient consequences that require clinical and regulatory judgment the model does not have. Do not trust an LLM's recollection of the Qini formula or any citation without verifying it against the source. Never let it generate the target list itself as if a recommendation were a calculation.

**LLM exercise (copy-paste prompt):**
> "You are a skeptical marketing-science reviewer. I claim that ranking physicians by predicted prescribing probability (propensity) is the wrong way to allocate a pharma rep budget, and that I should rank by causal responsiveness (CATE / uplift) instead. (1) State the strongest version of the counterargument — the case for ranking on propensity. (2) Then rebut it, using the four-quadrant uplift taxonomy and the Ascarza (2018) 'Retention Futility' result. (3) Identify one situation where propensity and persuadability would in fact be highly correlated, making my reframe less valuable. Be specific and concise."

**CLI exercise.** Using Claude Code on the synthetic dataset, ask it to: load the physician table with propensity and CATE columns, solve the constrained allocation (budget = 80 visits, max 3 per physician), exclude reciprocity-pathway and negative-$\tau$ physicians, and emit a Qini curve plot and an overlap percentage. Then read the code and find the place where it silently dropped negative-$\tau$ physicians without logging the exclusion reason — and fix it. The point is to catch the agent's plausible-but-wrong allocation, not to accept its output.

**AI validation.** Validate any AI-produced ranking three ways: (1) on the synthetic data with known ground truth, does the AI's top decile match the true high-$\tau$ physicians? (2) check the Qini against a hand-computed value on a small slice; (3) audit the exclusion log — every excluded physician must have a logged reason (negative $\tau$, reciprocity pathway, or capacity), with no silent dropping.

## AI Use Disclosure

*Write two sentences naming what an AI tool did in your work for this chapter and the one judgment it could not make. For example: "I used an LLM to scaffold the causalml pipeline and draft the Qini plotting code; I decided myself which physicians to exclude on pathway grounds, because that judgment requires knowing whether a physician's high CATE is driven by clinical education or by relationship maintenance — a structural distinction that requires the rep elicitation data from Chapter 8, not a calculation the model can perform."*

## What Would Change My Mind

I would abandon the persuadables reframe if, on a properly held-out randomized slice with adequate statistical power, the propensity-ranked list's Qini matched or exceeded the causal-responsiveness list's — if propensity and persuadability turned out to be strongly correlated in pharmaceutical detailing. Ascarza found them weakly correlated in churn; that result does not automatically transfer. I would also weaken the claim if CATE estimates proved too noisy to rank reliably near the persuadable/sure-thing boundary — if the "different list" was mostly estimation noise rather than real structure. Both are empirical questions this chapter sets up and cannot close. The Qini experiment on partner data is the test.

## Still Puzzling

How should a ranking honor the confidence intervals the causal forest returns? Ranking on the CATE point estimate throws away uncertainty; ranking on a lower confidence bound is more conservative but may systematically demote genuinely persuadable physicians with noisier estimates. There is no clean solution to ranking-under-CATE-uncertainty in this domain, and the literature does not provide one. Detecting Sleeping Dogs — physicians with negative $\tau$ — requires power that thin public data may not provide, which raises the question of whether full four-quadrant separation is honest on public data or only achievable with the richer proprietary panel across the firewall.

---

## Exercises

**Warm-up**

1. *(Recall — low difficulty) What this tests: whether you can state the four quadrants and their implications.* Name the four quadrant types in the uplift taxonomy (Persuadables, Sure Things, Lost Causes, Sleeping Dogs), state the sign of $\tau(x)$ for each, and give one sentence explaining what happens commercially if you target each quadrant. Then explain in one sentence why a propensity-ranked list over-selects Sure Things specifically.

2. *(Recall — low difficulty) What this tests: whether you understand why a better propensity model wastes more.* A brand team improves their propensity model from AUC 0.78 to AUC 0.91. They claim this should improve campaign efficiency. Explain in two sentences why, if propensity and persuadability are weakly correlated, a better propensity model will spend the budget *more* efficiently on the wrong population — and what metric would actually show whether campaign efficiency improved.

3. *(Recall — low difficulty) What this tests: whether you can state the constrained allocation objective correctly.* Write out the campaign allocation objective in plain English — not "maximize prescribing" but the full version that includes the capacity constraint, the welfare gate, and the educational-pathway gate. Then explain in one sentence why each of the three constraints is load-bearing rather than decorative.

**Application**

4. *(Apply — medium difficulty) What this tests: assigning quadrant labels to a real physician table.* Given a 200-physician synthetic dataset with `propensity` and `tau` columns, assign each physician to one of the four quadrants using thresholds you justify explicitly (not assumed from context). Report the counts in each quadrant. How many of the top-50-by-propensity physicians are Sure Things? Write one paragraph on what the NBA engine would do with this table and what it would spend its budget on.

5. *(Apply — medium difficulty) What this tests: computing the cost of propensity ranking in caused prescribing.* Take the top-50-by-propensity NBA list and the top-50-by-$\tau$ persuadables list from the synthetic dataset. Using the ground-truth $\tau$ for each physician, compute the total caused prescribing each list produces at a fixed budget of 80 visits (max 3 per physician). Report the ratio of caused prescribing (persuadables list / NBA list) and explain in two paragraphs why a more accurate propensity model would make the NBA number worse, not better.

6. *(Apply — medium difficulty) What this tests: excluding Sleeping Dogs correctly.* From the physician table, identify the physicians with $\tau(x) < 0$ (Sleeping Dogs). Explain in one sentence why they must be *excluded* rather than deprioritized. Then explain why a propensity model cannot identify them and what type of estimate is required. Finally, state one practical consequence if a Sleeping Dog is targeted accidentally — what happens commercially, and what happens to the consent and logging data.

**Synthesis**

7. *(Synthesis — high difficulty) What this tests: producing the full artifact.* Build both target lists on the synthetic dataset — the propensity-ranked NBA list and the causal-responsiveness list (excluding negative-$\tau$ and reciprocity-pathway physicians). Solve the constrained allocation for both (budget = 80 visits, max 3 per physician). Produce: (a) a Qini curve plotting both lists against random; (b) the overlap percentage between the two lists; (c) an exclusion log naming every excluded physician, the exclusion reason (negative $\tau$, reciprocity pathway, or capacity), and the CATE estimate that triggered the exclusion. State the Qini coefficient for each list and explain what the gap means for campaign ROI.

8. *(Synthesis — high difficulty) What this tests: connecting the allocation to the compliance and welfare constraints.* A physician has the highest $\tau(x)$ in your dataset — 0.82. Pathway analysis (using the rep elicitation and structural model) indicates the effect runs primarily through relationship maintenance: the physician prescribes more after meals, not after clinical education content. Write a two-paragraph analysis: (a) why this physician is excluded from the deployable target list despite having the highest CATE, (b) what this says about the relationship between causal-responsiveness ranking and the educational-pathway constraint, and (c) what you would do with this physician in the analysis layer — what the counterfactual that includes her tells you, even though she cannot be targeted.

**Challenge**

9. *(Challenge — open-ended) What this tests: designing the Qini validation experiment.* Design the held-out experiment that would test whether the causal-responsiveness list's Qini significantly exceeds the propensity list's Qini in a real partner dataset. Specify: the randomization design (how you create a holdout), the sample size calculation (what power you need to detect a meaningful Qini gap), the outcome variable (what you measure and over what window), the confounds you would need to control for, and the result that would cause you to abandon the persuadables reframe entirely. State honestly what this experiment requires that public data cannot supply and what the firewall implies for where it can be run.
