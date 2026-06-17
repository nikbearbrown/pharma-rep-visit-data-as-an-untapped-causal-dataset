# Chapter 11 — Projects 2 & 3: Meals (Causal Forest) and Slide Sequencing (Double Machine Learning)
*A tight confidence interval around a biased number is worse than an honest wide one.*

A martech team wants to optimize slide sequencing. The pitch is clean: the clickstream logs which slides a rep showed, in what order, with loops and skips; downstream we have prescribing; so learn the ordering that causes the most prescribing and push it to every rep's iPad.

They build it carefully. They know slide ordering is correlated with all kinds of confounders, so they reach for Double Machine Learning — the right modern tool for adjusting flexibly for high-dimensional controls. And then, reasonably, they decide to control for how engaged the physician was during the visit. Surely a physician who was engaged is different from one who was not. So into the control set go `Duration_vod` — seconds per slide — and `Reaction_vod` — the rep's per-slide positive/neutral/negative button.

The model runs. It returns a precise, confidently small confidence interval around an effect of slide ordering on NRx. The deck recommendation ships.

It is wrong. Not noisily wrong — *structurally* wrong, and invisibly so. Dwell time and reaction score are *caused by* the slide sequence that was delivered. They are post-treatment variables sitting on or downstream of the very causal path the team is trying to measure. Conditioning on them either strips out part of the effect — if they are mediators — or opens a non-causal path — if they are colliders. Either way the orthogonal moment DML so carefully constructs is now estimating the wrong quantity. And because DML is *good*, it estimates that wrong quantity *precisely.* The tight confidence interval is the most dangerous part: it broadcasts confidence in a number the structure guarantees is biased.

The failure is invisible in every metric the team looks at. The model fits. Cross-validation is fine. Nothing on the dashboard says "you conditioned on a collider." That is exactly why this chapter leads with the failure before it teaches the method.

---

Chapter 10 gave you one number — the average effect of a message variant on the physicians who got it. This chapter answers the two questions an average cannot: *for whom* does promotion work, and *can we estimate the effect of the thing we most want to optimize* without poisoning it?

Project 2 takes the first question. On fully public CMS Open Payments data, a causal forest estimates a *per-physician* meal effect — the conditional average treatment effect, $\tau(x)$ — and reads its heterogeneity across specialties, baseline prescribing volumes, and competitive contexts. Every Fellow can run this end to end without partner access.

Project 3 takes the second, harder question. The slide-sequence clickstream is the richest, most optimizable object in the dataset — and it is a trap. Double Machine Learning estimates the causal effect of slide ordering on prescribing, and the synthetic dataset, with its known planted truth, lets you watch exactly what happens when you condition on the colliders: the estimate drifts away from truth and the confidence interval *narrows.* That demonstration is the chapter's load-bearing lesson.

---

Start with what the causal forest actually is, because it is frequently confused with a predictive forest that has been relabeled.

A standard random forest predicts outcomes — it learns $\hat{E}[Y \mid X = x]$. A causal forest estimates treatment effects — it learns $\hat{\tau}(x) = \hat{E}[Y(1) - Y(0) \mid X = x]$, the conditional average treatment effect (CATE). These are different estimands, and the differences in how the forest is built follow from that.

Two innovations make a causal forest more than a relabeled regression forest. The first is **honest splitting** (Wager & Athey 2018, *JASA* 113(523):1228–1242). One subsample decides *where* the tree splits; a disjoint subsample estimates the *effect* inside each leaf. Reusing the same data for both would bias the leaf estimates toward whatever split was chosen — the tree would tune its estimates to the same observations that motivated its structure. Splitting the labor removes that bias and is what licenses valid statistical inference on the per-leaf effects.

The second innovation is **asymptotic normality and pointwise confidence intervals** for $\tau(x)$ via an infinitesimal-jackknife variance estimator. You get not just a personalized effect estimate but an honest error bar on it — so you can say "for physicians in this specialty and baseline tier, the meal effect is $\hat{\tau}$ with 95% CI [lb, ub]" rather than just a point prediction.

Athey, Tibshirani & Wager (2019, *Annals of Statistics* 47(2):1148–1178) generalize the forest further into **Generalized Random Forests (GRF)**: the forest as an adaptive local-weighting scheme for solving any local moment condition $E[\psi_{\theta(x)}(O) \mid X = x] = 0$. Causal-effect estimation, quantile regression, and IV estimation all fall out as special cases. The `grf` R package and Python's `econml` (`CausalForestDML`) are the canonical implementations.

For Project 2, the estimand is the CATE of receiving a promotional meal on prescribing the promoted brand, as a function of physician covariates: specialty, baseline prescribing volume, local competitive detailing intensity, patient mix. The forest returns a personalized effect for each physician. **Heterogeneity is not a curiosity here; it is the product.** An average meal effect of, say, 1.2 prescriptions per year hides the physician for whom the meal moves nothing and the one for whom it moves eight.

<!-- → [DIAGRAM: Comparison of predictive forest vs. causal forest — two parallel columns; left: standard random forest learns E[Y|X], uses all data for both splitting and estimation, output is predicted outcome; right: causal forest learns τ(x) = E[Y(1)-Y(0)|X], uses honest splitting with disjoint subsamples, output is per-unit treatment effect with confidence interval; key differences highlighted: estimand, splitting discipline, inference validity] -->

---

Now the discipline that comes immediately after the forest result, because it is the one most often violated.

The Chapter 5 graph places meals on the M3 reciprocity pathway — distinct from M1 educational. A meal can move prescribing through reciprocity (the regulatorily fraught channel) or through education (the meal accompanied a substantive clinical presentation, and the physician updated her beliefs). These have opposite ethical and regulatory status.

The causal forest cannot separate them. It gives you a heterogeneity map: which physicians show large meal effects and how that varies with their covariates. The pathway interpretation is a modeling-and-elicitation decision you layer on top. Heterogeneity that tracks meal value and frequency with no documented clinical exchange smells like reciprocity. Heterogeneity tied to a documented educational presentation and subsequent clinical-question resolution smells like M1. But "smells like" is an assumption, not a forest output.

The forest found heterogeneity. The pathway label requires the Chapter 5 graph and the Chapter 8 rep structural knowledge. Do not let yourself, or a stakeholder, slide from the first sentence to the second. The gap between them is the same gap as between association and causation — it is filled by a graph and an identifying assumption, not by a confidence interval.

---

Now the second project, which is harder methodologically and harder to talk about honestly.

Double Machine Learning (DML — Chernozhukov et al. 2018, *Econometrics Journal* 21(1):C1–C68) estimates a low-dimensional causal parameter $\theta$ when the confounders are high-dimensional and you want to adjust for them with flexible ML rather than a parametric model. Two properties make it work.

**Neyman orthogonality.** The moment condition used to estimate $\theta$ is first-order insensitive to errors in the estimated nuisance functions — the models for the outcome and treatment given confounders. So if your nuisance models are slightly wrong, because they are regularized ML models and regularization always introduces some bias, that error does not leak into $\theta$ to first order. The estimate of the causal parameter is protected from small mistakes in the auxiliary models.

**Cross-fitting.** Estimate the nuisance functions on one fold of the data. Evaluate the target moment on a held-out fold. Average. This breaks the dependence between nuisance-estimation error and the final estimate, and lets you use arbitrarily flexible ML — gradient-boosted trees, random forests, neural networks — without the restrictive classical conditions those methods would otherwise violate.

In the partially linear model, the mechanics are a residual-on-residual regression:

$$\text{Regress } Y \text{ on controls } Z \text{ (with ML)} \rightarrow \tilde{Y}$$
$$\text{Regress } D \text{ on controls } Z \text{ (with ML)} \rightarrow \tilde{D}$$
$$\text{Regress } \tilde{Y} \text{ on } \tilde{D} \rightarrow \hat{\theta}$$

The residuals $\tilde{Y}$ and $\tilde{D}$ are the parts of outcome and treatment that are *not* explained by the controls. The coefficient on $\tilde{D}$ is the effect of the treatment after removing what the controls explain — which is, under the identifying assumptions, the causal effect.

One line from this mechanics deserves to be memorized and carried forward: **the diagram chooses the controls; the ML only adjusts for them.** These are different jobs and they belong to different actors. The diagram — the structural causal model from Chapters 5 and 6 — decides which variables belong in $Z$. The ML step adjusts for them efficiently. Conflating these jobs is the error the opening case committed.

<!-- → [DIAGRAM: DML three-box flow — box 1: Regress Y on Z with ML → residual Ỹ; box 2: Regress D on Z with ML → residual D̃; box 3: Regress Ỹ on D̃ → θ̂; annotation under the diagram: "The diagram chooses Z. The ML adjusts for Z. These are different jobs." Red warning annotation: "Post-treatment variables in Z → structural bias; cross-fitting does not save you."] -->

---

Here is the central misconception stated precisely, because the chapter's whole argument rests on it.

The in-visit signals — `Duration_vod` (seconds per slide) and `Reaction_vod` (per-slide reaction) — are *caused by* the slide sequence that was delivered. They sit on or downstream of the treatment. Putting them in $Z$, the control set that DML residualizes on, means one of two things.

If the in-visit signal is a **mediator** — if the sequence affects dwell time, and dwell time affects prescribing — then residualizing on it strips out part of the causal path you are trying to estimate. You end up with the controlled direct effect of sequence on prescribing, with the dwell-time pathway blocked. That is a different quantity from the total effect the question asked for.

If it is a **collider** — jointly caused by the treatment and by some physician characteristic that also affects prescribing — then conditioning on it opens a spurious path between treatment and outcome. Bias appears where none existed before.

In either case, the orthogonal moment is no longer estimating the total causal effect. **DML's guarantees assume the conditioning set consists of pre-treatment confounders only.** Neyman orthogonality and cross-fitting protect against nuisance-estimation error — mistakes in fitting the ML models. They do *nothing* against a wrong conditioning set. The identifying assumptions of DML live in the graph, not in the algorithm.

This is not a new insight. Montgomery, Nyhan & Torres (2018, *AJPS* 62(3):760–775 `[verify exact pages]`) showed that conditioning on post-treatment variables ruins even a clean randomized experiment, because such variables can be affected by treatment and conditioning on them re-introduces confounding. Rosenbaum (1984, *JRSS-A* 147(5):656–666 `[verify pages]`) established the foundational structural root: adjusting for a concomitant variable that has itself been affected by treatment has consequences that no post-hoc adjustment can undo.

The signature failure mode: a tight confidence interval around a biased number. DML, because it is a good method efficiently using the data, will estimate the wrong quantity *precisely.* The tight CI broadcasts confidence in the wrong answer. It is worse than an honest wide CI because it manufactures false certainty.

---

Here is what the correct analysis looks like for both projects, including the dead ends.

**Project 2 — Meals on public data.**

Build the dataset from CMS Open Payments (filter to meals by NPI and drug) and Medicare Part D prescribing by NPI. This is the DeJong (2016) linkage, fully public.

```python
import pandas as pd

payments = pd.read_csv("openpayments_general.csv")
meals = payments[payments["nature_of_payment"] == "Food and Beverage"]
meals_by_npi = (meals.groupby(["covered_recipient_npi", "drug"])
                     .agg(meal_count=("record_id", "size"),
                          meal_value=("total_amount_usd", "sum"))
                     .reset_index())

partd = pd.read_csv("partd_prescriber.csv")
df = partd.merge(meals_by_npi, how="left",
                 left_on=["npi", "drug"],
                 right_on=["covered_recipient_npi", "drug"])
df["treated"] = (df["meal_count"].fillna(0) >= 1).astype(int)
df["y_rel"]   = df["brand_rx"] / (df["brand_rx"] + df["class_alt_rx"])
```

Assemble the covariate set. Pre-treatment only — specialty, baseline prescribing, region, patient volume, a competitive-detailing proxy. Not post-meal reaction, not anything caused by the meal.

```python
X = df[["specialty", "baseline_rx", "region", "patient_volume", "competitor_detailing"]]
X = pd.get_dummies(X, columns=["specialty", "region"], drop_first=True)
D = df["treated"]
Y = df["y_rel"]
```

Fit the causal forest:

```python
from econml.dml import CausalForestDML
from sklearn.ensemble import RandomForestRegressor, RandomForestClassifier

cf = CausalForestDML(
    model_y=RandomForestRegressor(n_estimators=500),
    model_t=RandomForestClassifier(n_estimators=500),
    discrete_treatment=True,
    cv=5,
    n_estimators=2000,
    honest=True,
)
cf.fit(Y, D, X=X)
tau_hat = cf.effect(X)
lb, ub  = cf.effect_interval(X)
```

Read the heterogeneity:

```python
import numpy as np
df["tau"] = tau_hat
print(df.groupby("specialty_bucket")["tau"].mean().sort_values())
print(cf.feature_importances_)
```

**Dead end #1 (teach it).** The naive instinct is to "isolate" the meal effect by controlling for post-meal physician engagement or reaction. That is the collider error — those are post-treatment, and including them corrupts the estimate. Do not.

**Dead end #2 (teach it).** Reading the CATE heterogeneity as proof of pathway. The forest found that specialty and baseline volume predict the meal effect. That is not proof the high-frequency-meal physicians respond through reciprocity. Pathway attribution requires the graph and the rep's structural knowledge.

**The limit to state out loud.** Open Payments meals are observational. DeJong et al. (2016), Yeh et al. (2016), and the Mitchell et al. (2021) systematic review all document association and explicitly decline to claim causation. The causal forest adjusts for measured pre-treatment covariates; residual confounding by physician disposition — a physician who is both industry-skeptical and conservative in prescribing will both refuse meals and show lower brand share — remains. Project 2 is a *heterogeneity upgrade* on an associational base, not a clean identification. Say this in any readout.

**Project 3 — Slide sequencing on synthetic data.**

The synthetic dataset plants a known ground-truth effect of slide ordering on NRx and plants the collider structure: dwell and reaction are generated *downstream* of the sequence. This lets you watch the correct and incorrect specifications and measure how far the wrong one drifts.

Step one is not code. It is marking every variable:

```python
Z = ["baseline_rx", "territory", "rep_tenure", "prior_message_history"]  # pre-treatment only
forbidden = ["dwell_seconds", "reaction_score"]                            # post-treatment — DO NOT CONDITION
```

Run DML correctly specified:

```python
from econml.dml import LinearDML
from sklearn.ensemble import GradientBoostingRegressor

dml = LinearDML(
    model_y=GradientBoostingRegressor(),
    model_t=GradientBoostingRegressor(),
    cv=5,
)
dml.fit(Y, D_seq, X=None, W=df[Z])
theta_correct = dml.coef_
ci_correct    = dml.coef__interval()
```

Now run the dead end on purpose — add the colliders:

```python
dml_bad = LinearDML(model_y=GradientBoostingRegressor(),
                    model_t=GradientBoostingRegressor(), cv=5)
dml_bad.fit(Y, D_seq, X=None, W=df[Z + forbidden])
theta_biased = dml_bad.coef_
ci_biased    = dml_bad.coef__interval()
```

Compare to ground truth:

```python
print("truth:   ", TRUE_SEQUENCE_EFFECT)
print("correct: ", theta_correct, ci_correct)   # ≈ truth, honest CI
print("biased:  ", theta_biased,  ci_biased)    # drifts from truth, deceptively TIGHT CI
```

The biased estimate moves away from the planted truth, and its confidence interval is often *tighter* than the correct one. That is the opening case made concrete and measurable. Not a noisier estimate — a confidently wrong one.

<!-- → [CHART: Side-by-side comparison plot — three vertical bars: "True effect" (horizontal reference line), "Correct DML" (CI overlapping truth), "Collider-contaminated DML" (point estimate displaced from truth, CI not overlapping truth, CI narrower than correct); caption: "Cross-fitting and orthogonality protect against nuisance error. They do not protect against a wrong conditioning set."] -->

---

The two projects are structurally the same family, and seeing that helps.

Both the causal forest and DML rest on the same orthogonalized residual-on-residual backbone. Modern causal forests build the R-learner residualization directly into the forest's local estimating equation — so they inherit DML-style robustness to nuisance-model error and deliver a heterogeneous $\theta(x)$ in one step. Cross-fitting (DML's bias-removal device) and honest splitting (the forest's bias-removal device) play the same role by different mechanisms. Both methods protect against the same error — nuisance-model misspecification — and are identically vulnerable to the same failure — a wrong conditioning set.

Teach them as one family: DML gives a clean average parameter; the causal forest gives a clean heterogeneous one; both stand or fall entirely on a correctly specified adjustment set chosen by the diagram, not the algorithm.

And the firewall runs between them along the public/proprietary line. Project 2 is fully public: Open Payments and Part D are CMS data anyone can download. Project 3 is not: `Call_Clickstream_vod`, `Display_Order_vod`, and `Call2_Key_Message_vod` are partner-owned. The synthetic dataset substitutes, with planted ground truth that lets you prove the method works before trusting it on data whose truth is hidden. The synthetic-first discipline is not a consolation prize; it is what makes the claim "DML correctly specified recovers the sequence effect" verifiable rather than asserted.

One methodological flag to carry: treating the slide sequence as a high-dimensional treatment in DML is this book's extension of the standard formulation, which handles high-dimensional *confounders* rather than a high-dimensional *treatment*. The synthetic-recovery test is what validates this extension. `[verify — author's extension; confirm recovery on synthetic data before claiming the method applies]`

---

**Five-Part AI Exercise Block**

**When to use AI here.** Scaffold `grf` and `econml` code, translate an R causal-forest workflow into Python, explain the residual-on-residual mechanics of DML and the honest-splitting logic of the forest in plain language. Draft the stakeholder explanation of why a tight CI around a biased number is worse than an honest wide one.

**When NOT to use AI here.** Do not ask the model to "pick the best controls" or "select the most predictive features." It will hand you the colliders, because predictiveness is exactly the wrong criterion. The adjustment set is a causal-diagram decision. Do not let it attribute a pathway from the heterogeneity alone. Do not let it certify your conditioning set is confounder-only — you mark every variable pre/post-treatment by hand, against the SCM.

**LLM exercise (copy-paste prompt):**
```
I am estimating the causal effect of in-visit slide ORDERING on a
physician's downstream prescribing, using Double Machine Learning.
My dataset has these columns:
  - slide_sequence (the treatment; high-dimensional ordering)
  - downstream_nrx (the outcome)
  - baseline_rx, territory, rep_tenure, prior_message_history
  - dwell_seconds_per_slide, reaction_score (logged DURING the visit)

1. Which variables are admissible controls for estimating the TOTAL
   causal effect of slide ordering, and which are NOT? For each
   inadmissible one, state whether it is a mediator or a collider
   and what conditioning on it does to my estimate.
2. Give me econml code (LinearDML, cross-fitting) using ONLY the
   admissible controls.
3. Explain why DML's Neyman orthogonality and cross-fitting do NOT
   rescue me if I include the inadmissible variables.
4. Tell me what symptom I would wrongly take as reassurance if I
   made the mistake — and why that symptom is misleading.

Do not optimize controls for predictive accuracy. Choose by causal
admissibility only.
```

**CLI exercise.** In a repo with the synthetic clickstream data, have Claude Code: (a) generate the DML pipeline with an explicit commented SCM block listing every variable as pre- or post-treatment; (b) write a `pytest` that asserts the correctly-specified DML recovers the synthetic ground-truth sequence effect within tolerance *and* asserts the collider-contaminated specification fails that tolerance; (c) emit the side-by-side comparison plot. The test that proves "correct recovers truth, contaminated does not" is the validation artifact.

**AI validation.** For Project 3, validation is recovery on known ground truth — the correctly specified DML must recover the planted effect inside its CI, and the collider-contaminated run must visibly miss it. For Project 2, there is no ground truth, so validation is honesty discipline: confirm the covariate set is pre-treatment-only and that the readout labels the estimate associational and the pathway an assumption.

**AI Use Disclosure**

*Write two sentences naming what an AI tool did in your work for this chapter and the one judgment it could not make. For example: "I used an LLM to scaffold the causal forest and DML code and to draft the stakeholder explanation of the collider failure; I determined myself which variables were pre-treatment and which were post-treatment, because the model offered `reaction_score` as a useful control based on its predictiveness and could not apply the SCM to reject it."*

---

**What Would Change My Mind**

I would weaken the chapter's framing if, on real proprietary clickstream data, the slide-sequence treatment proved so collinear with pre-treatment confounders that DML cannot identify a sequence effect even with the correct adjustment set — making Project 3 a non-starter regardless of the collider discipline. I would also revisit the post-treatment classification of `Reaction_vod` if a credible argument showed it is in fact determined before the prescribing decision rather than after — but under the SCM constructed in Chapter 5, it is downstream of the treatment, and that classification is an assumption that must be argued, not a fact that resolves itself. If the causal-forest heterogeneity in Project 2 turned out to be entirely driven by residual confounding — which a Rosenbaum-bounds sensitivity analysis could partly probe — I would demote the per-physician CATEs from "heterogeneous effects" to "heterogeneous associations," which is a significant demotion.

**Still Puzzling**

- What is the right representation of a high-dimensional sequence treatment? "Which slides in what order with loops and skips" is not a scalar; embedding it for DML (n-grams? a learned sequence embedding?) is genuinely open, and the embedding choice could itself smuggle in post-treatment information. This is the book's flagged methodological extension and it is not fully solved.
- Can pathway separation — reciprocity versus education — ever be *identified* rather than *assumed*? Mediation analysis needs strong sequential-ignorability assumptions; rep elicitation supplies structural priors but not proof. The honest answer is "assumed, with the assumption made explicit and sourced to rep knowledge and the causal graph."
- How much residual confounding survives in the Open Payments causal forest? Without a quasi-experiment like the cohort-rollout instrument, the per-physician CATE is an adjusted association. How wrong could it be, and would Rosenbaum-bounds sensitivity analysis change which physicians land in the persuadable decile?

---

## Exercises

**Warm-up**

1. *(Factual recall — honest splitting)* Explain honest splitting in two sentences: what problem does it solve, and what would happen to leaf-level estimates if you used the same data for splitting and estimation?
   *What this tests: whether you understand the bias that honesty removes, not just that the method uses disjoint subsamples.*

2. *(Factual recall — the three-box DML flow)* Draw and label the three-box residual-on-residual flow for DML. For each box, state which job belongs to the diagram and which belongs to the ML algorithm.
   *What this tests: the central discipline — diagram chooses controls, ML adjusts for them — held as a structural fact, not a slogan.*

3. *(Factual recall — the failure mode)* State precisely what happens when you include a post-treatment mediator in the DML control set $Z$, versus what happens when you include a collider. For each: what quantity is the orthogonal moment now estimating, and why is a tighter CI a bad sign rather than a good one?
   *What this tests: the two structural failure modes distinguished, and the CI-as-false-confidence trap.*

**Application**

4. *(Apply — Project 2 setup)* Build the meal treatment and relative-prescribing outcome for one drug class from Open Payments and Part D. List your full covariate set and justify each variable's inclusion in one sentence, confirming it is pre-treatment. Identify the one variable you were most tempted to include but excluded, and explain why.
   *What this tests: executing the pre-treatment discipline on real public data, with an explicit collider-temptation check.*

5. *(Apply — causal forest output)* Extend the Project 2 setup: fit the causal forest, produce per-physician CATEs with pointwise confidence intervals, and report heterogeneity by specialty and baseline tier. Then write the two-sentence honest readout that (a) labels the base estimate as associational and (b) labels pathway attribution as an assumption requiring the Chapter 5 graph and rep structural knowledge.
   *What this tests: the full Project 2 deliverable including the honesty discipline on pathway attribution.*

6. *(Apply — collider demonstration)* On the synthetic clickstream data, run DML twice: once with the correct pre-treatment-only adjustment set, once with `dwell_seconds` and `reaction_score` added. Report both estimates and their CIs against the known ground truth. Write one paragraph explaining why cross-fitting did not rescue the second estimate.
   *What this tests: the Project 3 core demonstration — making the failure visible on known truth rather than accepting it as a theoretical claim.*

**Synthesis**

7. *(Synthesize — stakeholder memo)* Write a one-page memo to a stakeholder who proposes "adding reaction score to the model because it's our most predictive feature." Explain, using the SCM and the post-treatment-collider argument, why this would corrupt the estimate, what the tight CI would falsely signal, and what the correct substitute for predictiveness is as a control-selection criterion.
   *What this tests: translating the structural argument into a persuasive communication for a non-specialist who is asking a reasonable-sounding question.*

8. *(Synthesize — per-physician CATE to targeting)* Take the per-physician meal CATEs from Exercise 5 and rank physicians by causal responsiveness. Compare the top decile by CATE to the top decile by baseline prescribing volume. How different are the two lists? What does the gap between them tell you about the cost of targeting on volume rather than on effect?
   *What this tests: connecting the CATE output to the Chapter 12 persuadables-versus-sure-things distinction — the bridge from estimation to decision.*

**Challenge**

9. *(Open-ended — the sequence-as-treatment extension)* The chapter treats the slide sequence as a high-dimensional treatment in DML and flags this as the book's methodological extension. Propose a validation test — beyond the synthetic-recovery check already described — that would give you more confidence the extension works on real clickstream data. What would you need to observe to believe the sequence-treatment DML is identifying a real causal effect rather than a residual association?
   *What this tests: thinking critically about a claimed methodological extension and designing the evidence that would make it credible.*
