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

These are not two estimates of the same thing. They are two different orderings of the same population. A physician can be high-propensity and low-persuadability — Dr. Alvarez, the loyalist who would prescribe regardless. Or mid-propensity and high-persuadability — Dr. Okafor, the physician with a specific concern the right content addresses. The correlation between the two orderings is weak — at least in the closest well-identified setting, customer retention (Ascarza 2018). That weakness is the opportunity; whether its *magnitude* carries over to pharmaceutical detailing is precisely the question this chapter hands to partner data (see the abandon-condition and the Qini experiment below), not something the public evidence settles.

---

The strongest academic statement of what propensity-ranking costs comes from a 2018 paper in the *Journal of Marketing Research* (55(1):80–98) by Eva Ascarza: "Retention Futility: Targeting High-Risk Customers Might Be Ineffective." Ascarza ran two field experiments and showed that targeting customers by churn risk — their propensity to leave — is ineffective at retention, because risk and responsiveness to a retention offer are distinct quantities and only weakly correlated. The high-risk customers are not the ones the intervention can save. The right target is heterogeneity in treatment responsiveness, not the risk score.

The transfer to pharmaceutical detailing is exact. Ranking physicians by prescribing propensity is the pharma analog of targeting high-churn-risk customers: you are ranking on baseline behavior, which tells you where the volume already lives, not where the message can cause a change. And a better propensity model does not fix this — it sharpens the error by more efficiently finding the Sure Things.

Ascarza won the Paul E. Green Award for methodological contribution to marketing research. Assign it as reading. The churn context is different from detailing; the statistical structure is identical.

![Two-axis grid. The horizontal axis is prescribing propensity P(NRx|X), low to high; the vertical axis is persuadability tau(x), negative through an explicit zero line to positive. Five named populations occupy the zones: Persuadables (high propensity, positive — the target), Hidden Persuadables (low propensity, positive — missed by propensity ranking), Sure Things (high propensity, near zero — the propensity trap), Lost Causes (low propensity, near zero), and Sleeping Dogs (the negative band, where detailing reduces prescribing). One arrow runs along the propensity axis for NBA selection; a second runs along the persuadability axis for Living-Model selection.](../images/12-from-propensity-to-persuadables-fig-01.png)

*Figure 12.1 — The propensity axis and the persuadability axis order the same physicians differently, and the two selection directions are roughly orthogonal. The NBA engine ranks sure-things (waste); the Living Model ranks persuadables. That the two lists differ systematically is a testable hypothesis — a held-out Qini comparison — not a settled fact (Ascarza 2018).*

<!-- → [DIAGRAM: Two-axis scatter plot. Horizontal axis: propensity P(NRx | X), low to high. Vertical axis: persuadability τ(x), negative to positive. Four quadrants labeled: upper-right "Persuadables (treat only if messaged)" — target; upper-left "Hidden Persuadables — missed by propensity ranking"; lower-right "Sure Things — the propensity trap, high baseline, zero increment"; lower-left "Lost Causes"; below horizontal axis (negative τ): "Sleeping Dogs — detailing reduces prescribing." Annotation: "NBA engine selects along the propensity axis. Living Model selects along the persuadability axis. Overlap is low." Caption: "The propensity axis and the persuadability axis order the same physicians differently. The gap between them is where budget is wasted."] -->

---

Cross a physician's response-if-treated against her response-if-not-treated and you get four kinds of physician. This taxonomy is standard in the uplift-modeling literature.

**Persuadables** prescribe only if treated — $\tau(x) > 0$ and baseline low. The entire return on detailing investment lives here. This is the population the campaign should find.

**Sure Things** prescribe regardless. Their $\tau(x) \approx 0$ despite high $P(\text{NRx} \mid X)$. Targeting them is pure waste. They are also, because their baseline volume is high, exactly who a propensity model preferentially selects. This is the propensity trap: the engine finds the people who need no help and calls it success.

**Lost Causes** never prescribe, treated or not. Low baseline, near-zero effect. Wasted contact, but at least benign.

**Sleeping Dogs** respond *negatively* to treatment — $\tau(x) < 0$. Detailing them reduces prescribing. The mechanism is reactance: the industry-skeptical physician for whom a fourth unsolicited visit triggers active resistance. Targeting them does not merely waste budget; it actively backfires. They must be *excluded* from the target list, not merely deprioritized — and detecting them requires a signed estimate of $\tau(x)$, not a propensity score, which is unsigned and cannot see the sign of the effect at all.

One structural note that bridges to the previous chapter: the Sleeping Dog population overlaps substantially with the consent-collider population. The industry-skeptical physician who reacts negatively to detailing is also the physician most likely to opt out of having her visits logged. The same person, seen from two different angles — the commercial problem and the measurement problem — is the same person.

An NBA engine ranks by propensity: it preferentially loads the list with Sure Things and is structurally blind to the sign of the effect. It cannot tell a Persuadable from a Sleeping Dog. A causal-responsiveness ranking sorts by $\tau(x)$: it selects Persuadables and excludes Sleeping Dogs. The lists differ. That difference is the product.

| Quadrant | Response if treated | Response if untreated | τ(x) | Propensity | If targeted | What the NBA engine does |
| --- | --- | --- | --- | --- | --- | --- |
| Persuadables | Yes | No | > 0 | Mid-tier | Prescribing increases — this is the ROI | Misses them (mid-tier propensity) |
| Sure Things | Yes | Yes | ≈ 0 | High | Prescribing unchanged — pure waste | Selects them preferentially (the propensity trap) |
| Lost Causes | No | No | ≈ 0 | Low | Nothing | Ignores |
| Sleeping Dogs | No | Yes (treated → reduced) | < 0 | Variable | Prescribing decreases — backfires | Cannot detect — propensity is unsigned |

*Table 12.1 — The NBA engine is blind to the sign of the effect and over-selects Sure Things. Both failures have the same cause: ranking on the wrong quantity. Detecting Sleeping Dogs requires a signed CATE estimate, not a propensity score; that the Living Model's list differs systematically from the NBA list is a testable hypothesis (held-out Qini), not a fact (Ascarza 2018).*

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

![Two side-by-side panels for the same 200-physician population. The left panel is the propensity-ranked NBA list, sorted by P(NRx|X); its top decile is shaded as low-persuadability Sure Things and annotated as receiving 84% of budget while causing only about 3% of the increment. The right panel is the persuadability-ranked list, sorted by CATE; its top decile is shaded as Persuadables and annotated as receiving the same 84% of budget while causing about 61% of the increment. A label between them notes the low overlap of the two top deciles.](../images/12-from-propensity-to-persuadables-fig-02.png)

*Figure 12.2 — Same budget, same physicians, two rankings. The propensity list buys conversions; the persuadables list causes them. The budget and increment shares are illustrative — only the direction of the contrast is claimed; whether the two lists differ this sharply is the testable Qini hypothesis (Ascarza 2018), not a measured magnitude.*

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

---

## Prompts

### Figure 12.1 — Persuadables vs Sure-Things (two-axis grid)

Generate a single self-contained HTML file (inline CSS, D3 7.9.0 from the cdnjs CDN) rendering a conceptual two-axis grid (not scattered data points). X axis: prescribing propensity P(NRx|X), low to high (split into low/high columns). Y axis: persuadability tau(x), negative through an emphasized zero line to positive (three bands: positive, zero, negative). Fill five named zones with grayscale-distinct rectangles plus the single red accent on the target: Persuadables (high propensity, positive — red, the target), Hidden Persuadables (low propensity, positive — gray), Sure Things (high propensity, zero band — mid gray, the trap), Lost Causes (low propensity, zero band — white outlined), Sleeping Dogs (full-width negative band — ink). Two selection-direction arrows: NBA along the horizontal propensity axis, Living Model along the vertical persuadability axis. Brutalist palette, full font chains via CSS variables (light+dark), rx=0 rects, no gradients, arrowhead marker, gridlines aria-hidden, reduced-motion guard. Caption: the two selection directions are orthogonal; "lists differ systematically" is a testable Qini hypothesis, not fact (Ascarza 2018). Deliverable: one HTML file.

### Figure 12.2 — Two target lists for the same 200 physicians

Generate a single self-contained HTML file (inline CSS, D3 7.9.0 from the cdnjs CDN) rendering a two-panel side-by-side comparison. Left panel "Propensity-ranked NBA list" (sorted by P(NRx|X)): a stacked column of decile rows with the top decile shaded mid-gray as "mostly Sure Things", annotated "84% of budget, ~3% of caused increment". Right panel "Persuadability-ranked list" (sorted by CATE): same column with the top decile shaded in the single red accent as "Persuadables", annotated "84% of budget, ~61% of caused increment". A double-headed arrow between the two top deciles labels "top-decile overlap: low". Brutalist: white bg, ink, one red series #C8102E, gray hairlines for decile separators, rx=0 rects, no gradients; EB Garamond title / Inter body / JetBrains Mono labels (full chains, CSS variables, light+dark); arrowhead marker, reduced-motion guard. Not a quantitative axis chart — shares are illustrative; caption: only the direction of the contrast is claimed (Ascarza 2018, testable via held-out Qini). Deliverable: one HTML file.

---

## Chapter 12 Exercises: From Propensity to Persuadables

**Project:** The Causal Interview Bot
**This chapter adds:** The bot's elicited susceptibility indices — reps' structural sense of who is genuinely movable — are used to identify persuadables, because reps often know which physicians a message can shift before any CATE is estimated.

### Exercise 1 — When to Use AI

You have per-physician CATEs from the Chapter 11 forest and the Causal Interview Bot's elicited susceptibility indices in hand. Three sound uses of an LLM.

- **Task A.** Have the model scaffold the `causalml` or `econml` Qini-curve pipeline — train/control split, uplift ranking, Qini coefficient, the random-targeting diagonal. *Why AI works here:* the plotting and ranking code is mechanical and you validate it by hand-computing the Qini on a small slice and confirming the AI's top decile matches the true high-τ physicians on synthetic data.
- **Task B.** Ask the model to make the strongest case for propensity ranking, then rebut it using the four-quadrant taxonomy and Ascarza (2018). *Why AI works here:* steel-manning then rebutting is an exposition task; you can check both halves against the chapter and the Ascarza result.
- **Task C.** Ask the model to translate the bot's elicited susceptibility index into a feature you can compare against the CATE ranking — e.g., "for each physician, does the rep's stated movability agree with the estimated τ?" *Why AI works here:* the index is your input; the model only joins and tabulates two columns you supplied, and the agreement table is something you read and judge.

**The tell:** in each case you can independently evaluate the output — recompute a Qini, check a rebuttal against a source, read a join you specified. The model accelerates; it does not decide who is movable.

### Exercise 2 — When NOT to Use AI

Three judgments the model must not own.

- **Task A.** Do not let the model decide which pathway drives a physician's effect, or set the welfare and compliance exclusions. *Why AI fails here:* whether a high-τ physician is movable through clinical education or through reciprocity, and whether the caused prescribing is clinically appropriate, are values judgments with real-patient consequences — they require the bot's elicited structural knowledge and clinical/regulatory judgment, not a calculation.
- **Task B.** Do not let the model generate the target list as if a recommendation were a calculation. *Why AI fails here:* targeting a physician is a decision about whom to influence and to what end; the values/consent dimension (Sleeping Dogs who backfire, opt-out-correlated skeptics) cannot be reduced to a τ-sort. The Sleeping Dog overlaps the consent-collider population — excluding them is a welfare call, not an arithmetic one.
- **Task C.** Do not trust the model's recollection of the Qini formula or any citation. *Why AI fails here:* there is a ground-truth definition it can misremember fluently; verify against the source.

**The tell:** if the AI is the *reason* a physician is on or off the target list, you have let a tool make a value judgment. It must draft and tabulate while *you* — using the bot's elicited indices and clinical/regulatory judgment — decide who is persuadable, who is excluded, and to what end. **Series connection:** Tier **T7 (Wisdom)** — persuadable targeting is fundamentally a value judgment about whom to move and whether the resulting prescribing serves the patient, not just a question of estimator reliability. The model cannot supply the wisdom; it can only sharpen the ranking you then govern.

### Exercise 3 — LLM Exercise

**What you're building:** a persuadables target list that fuses the estimated CATE ranking with the Causal Interview Bot's elicited susceptibility indices, gated to the educational pathway, with a full exclusion log.

**Tool:** Claude, as a **Claude Project** — the bot's elicited susceptibility indices and the Chapter 5 pathway graph live in Project knowledge from earlier chapters, so this prompt can fuse the rep prior with the CATE ranking without re-supplying either; the same Project carries into the Chapter 13 capstone.

**The Prompt:**

```
You are helping me build a persuadables target list for a pharma detailing
budget. I have a 200-physician synthetic table with columns:
  npi, propensity (P(NRx|X)), tau (estimated CATE), pathway
  (educational / relationship / reciprocity), rep_susceptibility_index
  (0-1, elicited from my Causal Interview Bot: the rep's structural sense of how
  movable this physician is).

Do five things:
1. Assign each physician to one of the four uplift quadrants (Persuadables, Sure
   Things, Lost Causes, Sleeping Dogs) using thresholds you state explicitly.
2. Build the persuadables list: rank by tau, then EXCLUDE every physician with
   negative tau (Sleeping Dog) and every physician whose pathway is reciprocity.
   Produce an exclusion log naming each excluded physician, the reason, and the
   tau that triggered it.
3. Compare the rep_susceptibility_index against the estimated tau ranking: where
   do reps and the model AGREE on who is movable, and where do they diverge?
   Flag the divergences for my review — do NOT auto-resolve them.
4. Solve the constrained allocation: budget = 80 visits, max 3 per physician,
   over the gated persuadables list only.
5. Give me causalml/econml code to produce a Qini curve plotting the
   persuadables list and the propensity list against random.

Do not choose the quadrant thresholds for me without stating them. Do not decide
which divergences to resolve. Do not target any reciprocity-pathway or negative-
tau physician. Flag anything uncertain as [verify].
```

**What this produces:** a quadrant-labeled table, a gated persuadables list, an exclusion log, an explicit rep-vs-model agreement table (divergences flagged, not resolved), a constrained allocation, and Qini-curve code — the Project 1-style persuadables artifact with the bot's index fused in.

**How to adapt:** swap in your synthetic table; if the bot's susceptibility index is on a different scale, normalize before joining. With ChatGPT or Gemini, paste the bot's index column and the pathway definitions inline. Keep the Claude Project so the gated list and exclusion log persist into Chapter 13's handoff.

**Connection to previous chapters:** the CATEs are the Chapter 11 forest's output; the susceptibility index is the Chapter 8 bot's; the pathway gate is the Chapter 5 three-pathway graph made operational; the rep-vs-model agreement check uses the bot exactly as Chapter 7 intended — structural knowledge to orient what the data leaves Markov-equivalent.

**Preview of next chapter:** Chapter 13 is the capstone — you will wrap this gated persuadables list inside the handoff brief, add the consent-collider guardrails the bot must respect, and ship the working bot plus an annotated prior DAG plus the handoff brief as the book's terminal artifact.

### Exercise 4 — CLI Exercise

**What you're building:** a constrained-allocation pipeline that ranks persuadables, excludes Sleeping Dogs and reciprocity-pathway physicians *with a logged reason for every exclusion*, and emits a Qini curve and overlap percentage.

**Tool:** Claude Code — this is a repo-level loop (data, allocation solver, exclusion logger, Qini plot, test) best run by an agentic CLI. · **Skill level:** intermediate.

**Setup:**
- [ ] A repo with the synthetic physician table (synthetic only — real targeting data and CRM telemetry are partner-proprietary, behind the firewall).
- [ ] Python with `causalml` or `econml` and a solver/plotting stack installed.
- [ ] The synthetic ground-truth τ recorded where a test can read it.

**The Task:**

```
Read data/physicians_synthetic.csv (columns: npi, propensity, tau, pathway,
rep_susceptibility_index) and data/ground_truth.json. Do NOT touch private/ or
*_real_* — proprietary, out of scope.

1. Build two target lists: a propensity-ranked NBA list and a tau-ranked
   persuadables list.
2. For the persuadables list, exclude every negative-tau physician (Sleeping
   Dog) and every reciprocity-pathway physician. EVERY exclusion must be written
   to data/exclusion_log.csv with npi, reason, and triggering tau. Silent drops
   are forbidden.
3. Solve the constrained allocation (budget = 80 visits, max 3 per physician)
   over the gated persuadables list.
4. Emit a Qini curve plotting both lists against random, and print the
   top-decile overlap percentage between the two lists.
5. Write a pytest asserting (a) on synthetic ground truth the persuadables
   top decile matches the true high-tau physicians within tolerance, and (b) the
   exclusion log row-count equals the number of physicians dropped (no silent
   drops).

Stopping condition: tests pass, Qini plot and exclusion log written. After
passing, print any place in the code that drops a physician without writing a log
row, so I can confirm there are none.
```

**Expected output:** two lists, a complete exclusion log, a constrained allocation, a Qini plot, the overlap percentage, and a passing test suite.

**What to inspect:** the exclusion log — every dropped physician must have a reason; and the agent's silent-drop check, because the chapter's specific trap is code that quietly removes negative-τ physicians without logging why.

**If it goes wrong:** the agent commonly drops negative-τ physicians inside a filter without logging the reason. Recovery: make the no-silent-drops assertion fail loudly (log row-count must equal drop count), then have the agent route every drop through a single logged exclusion function.

**CLAUDE.md note:** add — "Synthetic data only; never touch `private/` or `*_real_*`. Every physician exclusion is logged with a reason (negative τ, reciprocity pathway, or capacity); silent drops are a bug. Targeting decisions are value judgments — code ranks and gates, humans decide who ships."

### Exercise 5 — AI Validation Exercise

**What you're validating:** an AI-produced target list — your Exercise 3/4 output, or a pre-generated flawed artifact that ranked physicians by propensity, called the 94%-conversion result a success, and never computed a τ-based Qini.

**Validation type:** ground-truth recovery plus governance audit (exclusion log + pathway gate). **Risk level:** **high** — because a propensity-mistaken-for-persuadability list spends the entire budget on Sure Things, looks magnificent on conversion, and the better the propensity model the worse the waste.

**Setup:** run the artifact against the checklist on the synthetic table where the true τ is known; audit the exclusion log and pathway gate for the governance criteria.

**The Validation Task:**

```
Validation Checklist — Chapter 12 (Persuadables)
Mark each Pass / Fail / Cannot-determine.

[ ] Correctness: on synthetic ground truth, does the list's top decile match the
    true high-tau physicians (not the high-propensity ones)?
[ ] Completeness: is there a Qini curve comparing the list to the propensity list
    AND to random, with a stated Qini coefficient for each?
[ ] Scope: are physicians excluded on pathway/welfare grounds excluded (not just
    deprioritized), and is the analysis limited to the educationally drivable set?
[ ] Chapter-specific 1 — exclusion log: does every excluded physician have a
    logged reason (negative tau, reciprocity pathway, capacity), with no silent
    drops?
[ ] Chapter-specific 2 — sign awareness: are Sleeping Dogs (negative tau) detected
    and excluded, using a SIGNED CATE rather than an unsigned propensity score?
[ ] Failure-mode check (fluent-but-wrong): does the artifact rank on propensity
    and present a high conversion rate as success — propensity mistaken for
    persuadability? That is a FAIL even if the dashboard looks excellent.
[ ] Ground truth: is the conclusion anchored to caused prescribing on known tau,
    not to conversion rate?
```

**What to do with findings:** all pass — the list is shippable as a gated, governed persuadables recommendation. One fail — fix and re-run. Multiple fails, especially propensity-mistaken-for-persuadability plus a missing exclusion log, discard the list; it is the chapter's central error wearing a good dashboard.

**AI Use Disclosure prompt:** *Write two sentences naming what an AI tool did in your work and the one judgment it could not make — for example, that you decided which physicians to exclude on pathway grounds because that requires knowing whether a high CATE runs through clinical education or relationship maintenance, a structural distinction drawn from the Chapter 8 bot's elicitation, not a calculation the model can perform.* (Mandatory.)

**Series connection:** the failure mode is **propensity mistaken for persuadability** — a list that buys conversions it did not cause. Tier **T7 (Wisdom)**: whom to move, and whether the resulting prescribing serves the patient, are value judgments the model cannot make; it can rank and gate, but the targeting decision is governed by human wisdom.

---

## References (fact-check pass)

The chapter's single load-bearing external citation was verified CONFIRMED; the propensity-vs-persuadability list-divergence magnitude is correctly labeled a testable Qini hypothesis, not a measured fact. See `factchecks/12-from-propensity-to-persuadables-assertions.md`.

1. Ascarza, Eva. "Retention Futility: Targeting High-Risk Customers Might Be Ineffective." *Journal of Marketing Research* 55, no. 1 (2018): 80–98. Winner, 2018 Paul E. Green Award. [CONFIRMED]
