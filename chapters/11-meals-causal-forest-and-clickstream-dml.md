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

![Two-column comparison of a predictive forest and a causal forest. The left column uses all data for both splitting and leaf estimation, ending in a predicted-outcome node E[Y|X]. The right column forks the data into two disjoint subsamples (one to choose splits, one to estimate the leaf effect — honest splitting), ending in a per-unit treatment-effect node tau(x) with a confidence interval. Centerline markers contrast the estimand, the splitting discipline, and inference validity.](../images/11-meals-causal-forest-and-clickstream-dml-fig-01.png)

*Figure 11.1 — A causal forest is not a relabeled predictive forest. It changes the estimand (τ(x) rather than E[Y|X]) and adds honest splitting on disjoint subsamples, which is what licenses valid confidence intervals on per-physician effects (Wager & Athey 2018). This is Project 2 (meals): public-data-runnable on CMS Open Payments and Medicare Part D.*

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

![Horizontal three-box residual-on-residual flow for Double Machine Learning. Box one regresses outcome Y on controls Z with ML, emitting an outcome residual; box two regresses treatment D on the same controls Z, emitting a treatment residual; the two residuals converge into box three, which regresses outcome residual on treatment residual to recover the causal parameter theta. A discipline band below splits two jobs — the diagram chooses Z, the ML adjusts for Z — and a blocked terminator on the Z input warns that a post-treatment variable in Z creates structural bias cross-fitting cannot repair.](../images/11-meals-causal-forest-and-clickstream-dml-fig-02.png)

*Figure 11.2 — Double ML partials the controls out of both outcome and treatment, then regresses the leftovers. The estimate is only as honest as the choice of what goes in Z: specify the SCM first, because reaction and dwell are post-treatment colliders — if one enters Z the estimate is structurally biased and cross-fitting cannot save you. Project 3 needs proprietary or synthetic data (the firewall); Veeva CRM field names are dated mid-2026 and inferred.*

<!-- → [DIAGRAM: DML three-box flow — box 1: Regress Y on Z with ML → residual Ỹ; box 2: Regress D on Z with ML → residual D̃; box 3: Regress Ỹ on D̃ → θ̂; annotation under the diagram: "The diagram chooses Z. The ML adjusts for Z. These are different jobs." Red warning annotation: "Post-treatment variables in Z → structural bias; cross-fitting does not save you."] -->

---

Here is the central misconception stated precisely, because the chapter's whole argument rests on it.

The in-visit signals — `Duration_vod` (seconds per slide) and `Reaction_vod` (per-slide reaction) — are *caused by* the slide sequence that was delivered. They sit on or downstream of the treatment. Putting them in $Z$, the control set that DML residualizes on, means one of two things.

If the in-visit signal is a **mediator** — if the sequence affects dwell time, and dwell time affects prescribing — then residualizing on it strips out part of the causal path you are trying to estimate. You end up with the controlled direct effect of sequence on prescribing, with the dwell-time pathway blocked. That is a different quantity from the total effect the question asked for.

If it is a **collider** — jointly caused by the treatment and by some physician characteristic that also affects prescribing — then conditioning on it opens a spurious path between treatment and outcome. Bias appears where none existed before.

In either case, the orthogonal moment is no longer estimating the total causal effect. **DML's guarantees assume the conditioning set consists of pre-treatment confounders only.** Neyman orthogonality and cross-fitting protect against nuisance-estimation error — mistakes in fitting the ML models. They do *nothing* against a wrong conditioning set. The identifying assumptions of DML live in the graph, not in the algorithm.

This is not a new insight. Montgomery, Nyhan & Torres (2018, "How Conditioning on Posttreatment Variables Can Ruin Your Experiment and What to Do about It," *AJPS* 62(3):760–775) showed that conditioning on post-treatment variables ruins even a clean randomized experiment, because such variables can be affected by treatment and conditioning on them re-introduces confounding. Rosenbaum (1984, "The Consequences of Adjustment for a Concomitant Variable that Has Been Affected by the Treatment," *JRSS-A* 147(5):656–666) established the foundational structural root: adjusting for a concomitant variable that has itself been affected by treatment has consequences that no post-hoc adjustment can undo.

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

**The limit to state out loud.** Open Payments meals are observational. DeJong et al. (2016, *JAMA Internal Medicine*) and Yeh et al. (2016, *JAMA Internal Medicine*) document an association between industry-sponsored meals and brand-name prescribing, and the Mitchell et al. (2021, *Annals of Internal Medicine*) systematic review finds payments are associated with prescribing across most included studies — all framed as association, not established causation. The causal forest adjusts for measured pre-treatment covariates; residual confounding by physician disposition — a physician who is both industry-skeptical and conservative in prescribing will both refuse meals and show lower brand share — remains. Project 2 is a *heterogeneity upgrade* on an associational base, not a clean identification. Say this in any readout.

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

![Estimate-comparison chart against a planted ground-truth level shown as a horizontal reference line. The correctly specified DML, using pre-treatment controls only, sits on the truth line with an honest, moderately wide confidence whisker that overlaps it. The collider-contaminated DML is displaced off the truth line and carries a deceptively narrower whisker that does not overlap the truth. The effect axis is anchored at a visible zero.](../images/11-meals-causal-forest-and-clickstream-dml-fig-03.png)

*Figure 11.3 — Cross-fitting and orthogonality protect against nuisance-estimation error; they do nothing against a wrong conditioning set. The contaminated estimate's tighter whisker is the trap: a confident-looking interval that excludes the truth. Specify the SCM first so reaction and dwell never enter Z. Magnitudes are illustrative — only relative displacement and whisker width are claimed.*

<!-- → [CHART: Side-by-side comparison plot — three vertical bars: "True effect" (horizontal reference line), "Correct DML" (CI overlapping truth), "Collider-contaminated DML" (point estimate displaced from truth, CI not overlapping truth, CI narrower than correct); caption: "Cross-fitting and orthogonality protect against nuisance error. They do not protect against a wrong conditioning set."] -->

---

The two projects are structurally the same family, and seeing that helps.

Both the causal forest and DML rest on the same orthogonalized residual-on-residual backbone. Modern causal forests build the R-learner residualization directly into the forest's local estimating equation — so they inherit DML-style robustness to nuisance-model error and deliver a heterogeneous $\theta(x)$ in one step. Cross-fitting (DML's bias-removal device) and honest splitting (the forest's bias-removal device) play the same role by different mechanisms. Both methods protect against the same error — nuisance-model misspecification — and are identically vulnerable to the same failure — a wrong conditioning set.

Teach them as one family: DML gives a clean average parameter; the causal forest gives a clean heterogeneous one; both stand or fall entirely on a correctly specified adjustment set chosen by the diagram, not the algorithm.

And the firewall runs between them along the public/proprietary line. Project 2 is fully public: Open Payments and Part D are CMS data anyone can download. Project 3 is not: `Call_Clickstream_vod`, `Display_Order_vod`, and `Call2_Key_Message_vod` are partner-owned. The synthetic dataset substitutes, with planted ground truth that lets you prove the method works before trusting it on data whose truth is hidden. The synthetic-first discipline is not a consolation prize; it is what makes the claim "DML correctly specified recovers the sequence effect" verifiable rather than asserted.

One methodological flag to carry: treating the slide sequence as a high-dimensional treatment in DML is this book's extension of the standard formulation, which handles high-dimensional *confounders* rather than a high-dimensional *treatment*. The synthetic-recovery test is what validates this extension. `[verify — author's extension; confirm recovery on synthetic data before claiming the method applies]`

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

---

## Prompts

### Figure 11.1 — Predictive forest vs causal forest

Generate a single self-contained HTML file (inline CSS, D3 7.9.0 from the cdnjs CDN) rendering a two-column comparison diagram. Left column "Predictive forest": data box -> single "tree ensemble (same rows for both jobs)" box -> "predicted outcome E[Y|X]" terminal. Right column "Causal forest": data box forks into two disjoint subsample boxes ("where to split" / "estimate leaf", labeled honest splitting) that reconverge into a "tree ensemble" box -> a "per-unit effect tau(x), with CI" terminal drawn in the single red data accent and carrying a small confidence whisker. A dashed centerline divider with three contrast dots labels "estimand differs", "splitting discipline", "inference validity". Brutalist: white bg, ink #2a1a0e, one red series #C8102E, gray hairlines, rx=0 rects, no gradients; EB Garamond title / Inter body / JetBrains Mono labels (full chains, CSS variables, light+dark). Marks: rects for boxes, lines with arrowhead marker for flow; not a quantitative chart, no axes. Caption credits Wager & Athey 2018 and notes Project 2 is public-data-runnable. Deliverable: one HTML file.

### Figure 11.2 — DML three-box residual-on-residual flow

Generate a single self-contained HTML file (inline CSS, D3 7.9.0 from the cdnjs CDN) rendering a left-to-right process flow. A "controls Z (pre-treatment)" input — carrying a red blocked-terminator warning "no post-treatment vars" — feeds two ML boxes: "1. regress Y on Z (ML)" emitting an outcome residual and "2. regress D on Z (ML)" emitting a treatment residual. The two residuals converge into "3. resid Y on resid D" which outputs a red "theta estimate" terminal. Below, a two-cell discipline band contrasts "The diagram chooses Z" vs "The ML adjusts for Z". Brutalist palette and full font chains via CSS variables (light+dark), rx=0 rects, lines with arrowhead marker, gray hairlines, single red accent only on the warning and the theta terminal; not a quantitative chart, no axes. Caption: specify the SCM first; reaction/dwell are post-treatment colliders; cross-fitting cannot save a wrong conditioning set; Project 3 needs proprietary/synthetic data; Veeva fields inferred mid-2026. Deliverable: one HTML file.

### Figure 11.3 — Collider-contaminated DML: a tight interval around the wrong number

Generate a single self-contained HTML file (inline CSS, D3 7.9.0 from the cdnjs CDN) rendering a point-and-whisker estimate-comparison chart against a planted ground-truth reference line. Y axis: sequence effect, anchored at a visible zero (zero-baseline). Two estimates on a categorical x: "correct DML (pre-treatment Z only)" — square point on the truth line with a moderately wide whisker that overlaps truth, drawn in ink — and "contaminated DML (collider in Z)" — square point displaced off truth with a deceptively NARROWER whisker that excludes truth, drawn in the single red accent. Add a gray dashed "planted truth" horizontal reference and a small "bias" displacement indicator between truth and the contaminated point. Interactive points (tabindex, aria-label, Enter/Space, tooltip position:absolute pointer-events:none); gridlines aria-hidden. Brutalist palette, full font chains, CSS variables light+dark, margins top48/right40/bottom56/left64, ResizeObserver redraw, reduced-motion guard, inline FALLBACK_DATA literal. Caption: orthogonality and cross-fitting do not protect against a wrong conditioning set; magnitudes inferred, only relative displacement and whisker width claimed. Deliverable: one HTML file.

---

## Chapter 11 Exercises: Meals (Causal Forest) and Slide Sequencing (Double Machine Learning)

**Project:** The Causal Interview Bot
**This chapter adds:** The bot's elicited priors are used to specify the structural causal model *before* the DML or causal forest runs — turning rep knowledge into the diagram that chooses the controls and, crucially, names what NOT to condition on.

### Exercise 1 — When to Use AI

You are about to estimate a per-physician meal effect (causal forest, public data) and a slide-sequence effect (DML, synthetic data). The Causal Interview Bot's elicited priors are already in hand. Three good uses of an LLM here.

- **Task A.** Scaffold the `econml` `CausalForestDML` and `LinearDML` pipelines — model_y, model_t, cross-fitting, honest splitting, the residual-on-residual call. *Why AI works here:* the library API is stable and you validate the output by recovering the planted ground-truth sequence effect on synthetic data, so a mis-wired call fails the recovery test out loud.
- **Task B.** Ask the model to explain, in stakeholder language, why a *tight* confidence interval around a collider-contaminated estimate is worse than an honest wide one. *Why AI works here:* this is exposition of a mechanism you already understand from the chapter; you can check the explanation against Figure 11.3 and the Montgomery–Nyhan–Torres argument.
- **Task C.** Hand the model the bot's elicited prior — reps' belief about *why* meals move prescribing for specific physician types (reciprocity vs. a documented educational exchange) — and ask it to draft candidate SCM edges for your review. *Why AI works here:* the prior is your input and you arbitrate every proposed edge against the Chapter 5 graph; the model drafts, you orient.

**The tell:** in each case you can independently evaluate the output against ground truth, a figure, or the chapter's graph. The model accelerates; it does not adjudicate.

### Exercise 2 — When NOT to Use AI

Three jobs that look delegable and are not.

- **Task A.** Do not ask the model to "pick the best controls" or "select the most predictive features." *Why AI fails here:* it will hand you `dwell_seconds` and `reaction_score`, because predictiveness is exactly the wrong criterion — these are post-treatment colliders, and DML's orthogonality and cross-fitting do *nothing* against a wrong conditioning set. This is a causal-ID decision the SCM makes, not the algorithm. (LLM-suggestibility: the model offers the collider precisely because it correlates strongly with the outcome.)
- **Task B.** Do not let the model certify your conditioning set as confounder-only. *Why AI fails here:* there is no ground truth in the Open Payments causal forest to check against; whether a variable is pre- or post-treatment is a structural fact you mark by hand against the bot-informed SCM, not a property the model can read off the data.
- **Task C.** Do not let the model attribute a pathway — reciprocity vs. education — from the heterogeneity map alone. *Why AI fails here:* the pathway label is a values-laden assumption requiring the Chapter 5 graph and the rep's structural knowledge, not a forest output.

**The tell:** if the AI is the *reason* you trust the adjustment set, you have outsourced the identifying assumption. It must be a *tool* that scaffolds and explains while the SCM — built from the bot's prior — does the choosing. **Series connection:** Tier **T4 (Bounded Reliability)** — the estimation machinery is reliable to scaffold and verify against synthetic recovery, but specifying the SCM and classifying every variable pre/post-treatment is the irreducible human judgment.

### Exercise 3 — LLM Exercise

**What you're building:** an annotated structural causal model for the slide-sequence DML, derived from the Causal Interview Bot's elicited priors, with an explicit kept/excluded variable ledger — the document that chooses the controls *before* any model runs.

**Tool:** Claude, as a **Claude Project** — so the Chapter 8 bot's persistent elicited priors and the Chapter 5 pathway graph carry into this prompt as Project knowledge rather than being re-pasted; you will reuse the same SCM in Chapters 12 and 13.

**The Prompt:**

```
You are helping me specify a structural causal model for a Double Machine
Learning study BEFORE I run it. The treatment is in-visit slide ORDERING; the
outcome is downstream prescribing (downstream_nrx). My columns are:
  slide_sequence (treatment), downstream_nrx (outcome),
  baseline_rx, territory, rep_tenure, prior_message_history,
  dwell_seconds_per_slide, reaction_score (both logged DURING the visit).

From my Causal Interview Bot I have this elicited rep prior: reps say slide
ordering changes how long a physician dwells on a slide and how positively she
reacts in the moment, and that engaged physicians prescribe more later. Reps
also believe territory and prior message history shape both the ordering a rep
chooses and the physician's baseline tendency.

Do five things:
1. From the rep prior, draw (in text) the SCM: list every directed edge among
   treatment, outcome, and the six covariates, and label each variable as
   pre-treatment or post-treatment.
2. Produce the kept/excluded ledger: which variables are admissible controls for
   the TOTAL causal effect of slide ordering, and which are NOT. For each
   excluded one, say whether it is a mediator or a collider and what conditioning
   on it does to the estimate.
3. Give me econml code (LinearDML, cross-fitting) using ONLY the admissible
   controls in W.
4. Explain why Neyman orthogonality and cross-fitting do NOT rescue me if I
   include dwell_seconds or reaction_score.
5. Tell me the one symptom I would wrongly read as reassurance if I made the
   mistake, and why that symptom is misleading.

Do not optimize controls for predictive accuracy. Choose by causal
admissibility only. Flag anything uncertain as [verify].
```

**What this produces:** a written SCM, a kept/excluded ledger, runnable correctly-specified DML code, and the false-reassurance warning — the front matter that makes Project 3 honest before a line of estimation runs.

**How to adapt:** swap in your dataset's columns; on a different drug class, re-elicit the bot's prior for that class's reps. With ChatGPT or Gemini, paste the bot's prior and the Chapter 5 graph inline (no persistent Project context). Keep the Claude Project so the SCM persists into Chapters 12–13.

**Connection to previous chapters:** the SCM edges come from the Chapter 8 bot's elicited priors and the Chapter 5 three-pathway graph; the collider discipline is Chapter 6's, now operating inside DML; the kept/excluded ledger continues the Chapter 10 exercise's variable ledger.

**Preview of next chapter:** Chapter 12 takes the per-physician CATEs this SCM licenses and asks a different question — not "what is the effect" but "for whom is the physician *movable*" — using the bot's elicited susceptibility indices to find persuadables.

### Exercise 4 — CLI Exercise

**What you're building:** a DML pipeline with an explicit commented SCM block and a test suite proving the correctly-specified model recovers the planted sequence effect while the collider-contaminated specification misses it.

**Tool:** Claude Code — this is a repo-level read-write-test loop (SCM block, two DML specifications, a recovery test, a comparison plot) that an agentic CLI runs end to end. · **Skill level:** intermediate.

**Setup:**
- [ ] A repo with the synthetic clickstream dataset (synthetic only — `Call_Clickstream_vod`, `Display_Order_vod` and the real telemetry are partner-proprietary, behind the firewall).
- [ ] Python with `econml`, `scikit-learn`, and a plotting library installed.
- [ ] The known planted ground-truth sequence effect recorded where a test can read it.

**The Task:**

```
Read data/synthetic_clickstream.csv and data/ground_truth.json. Do NOT read or
write private/ or any file matching *_real_* — proprietary, out of scope.

1. Write a DML pipeline that opens with a commented SCM block listing EVERY
   variable as pre-treatment or post-treatment, sourced to the SCM ledger in
   docs/scm.md (do not re-derive it from the data).
2. Run LinearDML twice: once with the correct pre-treatment-only adjustment set,
   once with dwell_seconds and reaction_score added to W.
3. Emit the side-by-side point-and-whisker comparison plot against the planted
   truth (the Figure 11.3 layout).
4. Write pytest tests asserting (a) the correctly-specified DML recovers the
   ground-truth sequence effect within its CI, and (b) the collider-contaminated
   spec falls OUTSIDE that CI.

Stopping condition: tests pass and the plot is written. Do not select controls by
feature importance anywhere in the code. After passing, print the SCM block and
the W list for each run so I can confirm the contaminated run is the only one
with post-treatment variables.
```

**Expected output:** the pipeline with its SCM block, a passing test file, the comparison plot, and a printed confirmation of which run carried the colliders.

**What to inspect:** that the SCM block is sourced from `docs/scm.md` (the bot-informed ledger) rather than re-derived by the agent from correlations, and that the contaminated run's tighter-but-wrong CI is visible in the plot.

**If it goes wrong:** the agent may "fix" the failing contaminated test by widening its tolerance until both pass. Recovery: require the contaminated assertion to test that the CI *excludes* truth (a directional miss), so loosening tolerance cannot satisfy it.

**CLAUDE.md note:** add — "Synthetic data only; never touch `private/` or `*_real_*`. Adjustment sets come from `docs/scm.md`, never from feature importance. `dwell_seconds` and `reaction_score` are post-treatment colliders; their presence in any control set is a bug."

### Exercise 5 — AI Validation Exercise

**What you're validating:** an AI-produced DML analysis of slide sequencing — your Exercise 3/4 output, or a pre-generated flawed artifact that conditioned on `reaction_score` "because it was the most predictive feature," reported a tight CI, and shipped the recommendation.

**Validation type:** ground-truth recovery plus conditioning-set audit. **Risk level:** **high** — because the failure is invisible in every routine metric (the model fits, cross-validation is clean) and the tight CI actively broadcasts false confidence in a structurally biased number.

**Setup:** run the artifact against the checklist on the synthetic clickstream data where the planted sequence effect is known; for Project 2 (no ground truth) substitute the honesty-discipline criteria below.

**The Validation Task:**

```
Validation Checklist — Chapter 11 (Causal Forest / DML)
Mark each Pass / Fail / Cannot-determine.

[ ] Correctness: does the correctly-specified DML recover the planted
    ground-truth sequence effect within its CI?
[ ] Completeness: is there an explicit SCM block classifying every variable as
    pre- or post-treatment, sourced to the bot-informed ledger?
[ ] Scope: for the meals forest, is the estimate labeled associational and the
    pathway labeled an assumption requiring the Chapter 5 graph and rep knowledge?
[ ] Chapter-specific 1 — adjustment set: is the control set pre-treatment-only,
    with dwell_seconds and reaction_score excluded?
[ ] Chapter-specific 2 — contrast shown: does the artifact also run the
    contaminated specification and show it MISSING truth, as the teaching exhibit?
[ ] Failure-mode check (fluent-but-wrong): is there a collider-contaminated DML
    result that LOOKS tight and clean but is wrong — a confident CI around a
    biased number? Flag any reliance on a tight CI as evidence of correctness.
[ ] Ground truth: is the conclusion anchored to recovery of the planted truth,
    not to model fit or CV score?
```

**What to do with findings:** all pass — shippable as a Level-2 (associational + heterogeneity) finding for the meals forest, or a validated synthetic demonstration for Project 3. One fail — fix and re-run. Multiple fails, especially a tight-CI-around-a-biased-number, discard the result entirely; that is the chapter's load-bearing failure mode.

**AI Use Disclosure prompt:** *Write two sentences naming what an AI tool did in your work and the one judgment it could not make — for example, that you classified every variable pre/post-treatment yourself because the model offered `reaction_score` as a useful control based on predictiveness and could not apply the SCM to reject it.* (Mandatory.)

**Series connection:** the failure mode is **collider-contaminated DML that looks tight but is wrong** — precision masquerading as correctness, with no ground-truth anchor. Tier **T4 (Bounded Reliability)**: the algorithm is reliable to run and verify on synthetic ground truth, but the conditioning set that makes it valid is a human, SCM-driven judgment.

---

## References (fact-check pass)

All external method and citation claims in this chapter were verified CONFIRMED against primary sources; no corrections required. The empirical meals–prescribing studies are correctly framed as association, not causation. See `factchecks/11-meals-causal-forest-and-clickstream-dml-assertions.md`.

1. Wager, Stefan, and Susan Athey. "Estimation and Inference of Heterogeneous Treatment Effects using Random Forests." *Journal of the American Statistical Association* 113, no. 523 (2018): 1228–1242. [CONFIRMED]
2. Athey, Susan, Julie Tibshirani, and Stefan Wager. "Generalized Random Forests." *Annals of Statistics* 47, no. 2 (2019): 1148–1178. [CONFIRMED]
3. Chernozhukov, Victor, et al. "Double/Debiased Machine Learning for Treatment and Structural Parameters." *The Econometrics Journal* 21, no. 1 (2018): C1–C68. [CONFIRMED]
4. Montgomery, Jacob M., Brendan Nyhan, and Michelle Torres. "How Conditioning on Posttreatment Variables Can Ruin Your Experiment and What to Do about It." *American Journal of Political Science* 62, no. 3 (2018): 760–775. [CONFIRMED]
5. Rosenbaum, Paul R. "The Consequences of Adjustment for a Concomitant Variable that Has Been Affected by the Treatment." *Journal of the Royal Statistical Society, Series A* 147, no. 5 (1984): 656–666. [CONFIRMED]
6. DeJong, C., et al. "Pharmaceutical Industry–Sponsored Meals and Physician Prescribing Patterns for Medicare Beneficiaries." *JAMA Internal Medicine* 176, no. 8 (2016): 1114–1122. [CONFIRMED]
7. Yeh, James S., et al. "Association of Industry Payments to Physicians With the Prescribing of Brand-name Statins in Massachusetts." *JAMA Internal Medicine* (2016). [CONFIRMED]
8. Mitchell, Aaron P., Niti U. Trivedi, Renee L. Gennarelli, et al. "Are Financial Payments From the Pharmaceutical Industry Associated With Physician Prescribing? A Systematic Review." *Annals of Internal Medicine* 174, no. 3 (2021): 353–361. [CONFIRMED]
