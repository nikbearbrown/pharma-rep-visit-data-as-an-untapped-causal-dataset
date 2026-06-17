# Chapter 11: Projects 2 & 3 — Meals (Causal Forest) and Slide Sequencing (Double Machine Learning)

## 1. Overview

Chapter 10 gave you one number — the average effect of a message variant on the physicians who got it. This chapter answers the two questions an average cannot: *for whom* does promotion work, and *can we even estimate the effect of the thing we most want to optimize* without poisoning it?

Project 2 takes the first question. Using a **causal forest** on fully public CMS Open Payments data, you will estimate a *per-physician* meal effect — the conditional average treatment effect, τ(x) — and read its heterogeneity to begin separating the reciprocity pathway from the educational one. This is the safe sandbox: every Fellow can run it end to end on public data, no partner access required.

Project 3 takes the second, and harder, question. The slide-sequence clickstream is the single richest, most optimizable object in the dataset — and it is a trap. Using **Double Machine Learning (DML)**, you will estimate the causal effect of slide ordering on prescribing, and you will learn the central, load-bearing lesson of this book: **the structural causal model must be specified *before* you residualize.** The in-visit signals that look like the most useful controls — dwell time, reaction scores — are *post-treatment colliders.* Conditioning on them does not clean the estimate; it corrupts it, and DML's machinery will hand you a *tight confidence interval around a biased number* — which is worse than an honest wide one.

By the end you will be able to fit a causal forest for heterogeneous meal effects, run DML on a high-dimensional treatment with a correctly specified adjustment set, and explain — with a worked demonstration on synthetic data where the truth is known — exactly why naive conditioning on engagement destroys the clickstream estimate.

---

## 2. Learning objectives

By the end of this chapter you will be able to:

- **(Create)** Fit a causal forest to estimate heterogeneous treatment effects of promotional meals on prescribing, sliced by specialty, baseline volume, and competitive context, on public Open Payments + Part D data.
- **(Apply)** Run Double Machine Learning on the clickstream slide-sequence treatment with the SCM specified *first* — admissible controls being pre-treatment confounders only.
- **(Evaluate)** Explain why conditioning on reaction/dwell (post-treatment colliders) corrupts the DML estimate, and demonstrate the corruption on synthetic data with known ground truth.
- **(Evaluate)** Distinguish "the forest found heterogeneity" from "the meal worked through reciprocity" — i.e., recognize pathway attribution as an *assumption* layered on the estimate, not an output of it.
- **(Understand)** Locate the public-data/IP firewall concretely: why Project 2 is public-runnable and Project 3 is not.

---

## 3. Opening case: the optimization that blocked its own causal path

A martech team wants to optimize slide sequencing. The pitch is clean: the clickstream logs which slides a rep showed, in what order, with what loops and skips; downstream we have prescribing; so let's learn the *ordering* that causes the most prescribing and push it to every rep's iPad.

They build it carefully. They know slide ordering is correlated with all kinds of confounders, so they reach for Double Machine Learning — the right modern tool for adjusting flexibly for high-dimensional controls. And then, reasonably, they decide to "control for how engaged the physician was during the visit," because surely a physician who was engaged is different from one who wasn't. So into the control set go `Duration_vod` (seconds per slide) and `Reaction_vod` (the rep's per-slide positive/neutral/negative button).

The model runs. It returns a precise, confidently small confidence interval around an effect of slide ordering on NRx. The deck recommendation ships.

It is wrong. Not noisily wrong — *structurally* wrong, and invisibly so. Dwell time and reaction score are *caused by* the slide sequence that was delivered. They are post-treatment variables sitting on (or downstream of) the very causal path the team is trying to measure. Conditioning on them either strips out part of the effect (if they are mediators) or opens a non-causal path (if they are colliders). Either way the orthogonal moment DML so carefully constructs is now estimating the wrong quantity — and because DML is *good*, it estimates that wrong quantity *precisely.* The tight CI is the most dangerous part: it broadcasts confidence in a number that the structure guarantees is biased.

The failure is invisible in every metric the team looks at. The model fits. The CI is tight. Cross-validation is fine. Nothing on the dashboard says "you conditioned on a collider." That is exactly why this chapter leads with the failure before it teaches the method.

---

## 4. Core sections

### 4.1 Causal forests — the CATE machine (Project 2)

A causal forest estimates not a single average effect but a **conditional average treatment effect** (CATE): the effect for a unit with covariates *x*,

```
τ(x) = E[ Y(1) − Y(0) | X = x ].
```

It adapts the random forest from prediction to causal estimation. Two innovations make it more than a relabeled regression forest (Wager & Athey 2018, *JASA* 113(523):1228–1242):

- **Honest splitting.** One subsample decides *where* the tree splits; a *disjoint* subsample estimates the *effect* inside each leaf. Reusing the same data for both would bias the leaf estimates toward whatever split was chosen; splitting the labor removes that bias and is what licenses valid inference.
- **Asymptotic normality + pointwise confidence intervals** for τ(x), via an infinitesimal-jackknife variance estimator. This was the first random-forest-based heterogeneous-effects method with *formal* inference guarantees — you get not just a personalized effect but an honest error bar on it.

**Generalized Random Forests (GRF)** (Athey, Tibshirani & Wager 2019, *Annals of Statistics* 47(2):1148–1178) reframe the forest as an **adaptive local-weighting (kernel) scheme** for solving *any* local moment condition E[ψ_{θ(x)}(O) | X = x] = 0. Causal-effect estimation, quantile regression, and IV estimation all fall out as special cases of the moment ψ. The `grf` R package (`causal_forest`, `instrumental_forest`) is canonical; Python's `econml` provides `CausalForestDML`.

For Project 2, the estimand is the **CATE of receiving a promotional meal on prescribing the promoted brand**, as a function of physician covariates: specialty, baseline prescribing volume, local competitive detailing intensity, patient mix. The forest returns, for each physician, a personalized "how much does a meal move *this* physician" number — which is exactly the persuadables input Chapter 12 consumes. **Heterogeneity is not a curiosity here; it is the product.**

### 4.2 Pathway separation — what the forest can and cannot do

The Chapter 5 graph puts meals on the **M3 reciprocity** pathway, distinct from **M1 educational.** A meal can move prescribing through reciprocity (the $40B pathway, regulatorily fraught) *or* through education (the legally drivable pathway — the meal accompanied a substantive on-label clinical presentation). These have opposite ethical and regulatory status, so separating them is the whole point.

Here is the discipline you must hold: **the causal forest does not label the pathway.** It gives you a heterogeneity map — which physicians show large meal effects and how that varies with covariates. The pathway *interpretation* is a modeling-plus-elicitation decision layered on top. You can lean heterogeneity *toward* a reading — heterogeneity that tracks meal *value and frequency* with no clinical content smells like reciprocity; heterogeneity tied to a documented educational presentation smells like M1 — but "smells like" is an assumption, not a forest output.

> **Load-bearing flag.** The forest cannot *prove* which pathway carried the effect. Pathway attribution is an identifying *assumption* (informed by the Chapter 5 graph and Chapter 8 rep elicitation), not a result. Do not let yourself, or a stakeholder, slide from "the forest found heterogeneity" to "the meal worked through reciprocity." The second sentence is a claim that requires the graph and the rep's structural knowledge — the data alone is Markov-equivalent across the stories (Chapter 7).

### 4.3 Double Machine Learning — clean adjustment for high-dimensional controls (Project 3)

DML (Chernozhukov et al. 2018, *Econometrics Journal* 21(1):C1–C68) estimates a low-dimensional causal parameter θ — here, the effect of slide ordering on NRx — when the confounders are high-dimensional and you want to adjust for them with flexible ML. Two ingredients make it work:

- **Neyman orthogonality.** Use a moment/score that is *first-order insensitive* to errors in the estimated nuisance functions, so the regularization bias inherent in any ML model does not leak into θ. The estimate of θ is, to first order, immune to small mistakes in the nuisance models.
- **Cross-fitting.** Estimate the nuisance functions on one fold; evaluate the target moment on the held-out fold; average. This breaks the dependence between nuisance-estimation error and the final estimate, and it lets you use arbitrarily flexible ML without the restrictive empirical-process (Donsker) conditions classical theory would demand.

In the partially linear model the mechanics are **partialling-out**, a residual-on-residual regression:

```
Regress Y on controls Z (with ML)  →  residual Ỹ
Regress D on controls Z (with ML)  →  residual D̃
Regress Ỹ on D̃                     →  θ̂
```

The same three-box flow appears in the living-models framework, with one permanent annotation that is the heart of this chapter: **Z is chosen by the causal diagram (the backdoor criterion), not by the ML step.** The ML adjusts for the controls. The diagram *chooses* the controls. These are different jobs and they belong to different people — the diagram to the analyst, the adjustment to the algorithm.

**The high-dimensional-treatment twist.** In Project 3 the *treatment itself* is high-dimensional: the `Call_Clickstream_vod` slide sequence — which slides, in what order, with loops and skips encoded by `Display_Order_vod`. DML's appeal is precisely that ML can carry that high-dimensional structure in the nuisance models while orthogonality keeps the causal target clean. (Note: standard DML literature treats high-dimensional *confounders*; framing the *treatment* as high-dimensional sequence is this book's extension, and is flagged `[verify — author's extension]` — validate recovery on the synthetic data.)

### 4.4 The central misconception — the chapter's hinge

> **"Slide dwell time and reaction score are useful, predictive features — control for them to clean up the estimate."** They are **post-treatment colliders.** Conditioning on them to estimate the slide-sequence → NRx effect is a structural error that *no amount of ML flexibility or cross-fitting can fix.*

Why, mechanically. `Duration_vod` (dwell), `Reaction_vod`, and the other in-visit signals are *caused by* the treatment — the slide sequence that was delivered — and also relate to the outcome. They sit on, or downstream of, the causal path. Putting them into the control set Z that DML residualizes on means one of two bad things:

- If the in-visit signal is a **mediator** (sequence → dwell → prescribing), residualizing on it strips out part of the very effect you want. You end up estimating a *controlled direct effect*, not the *total effect* — a different quantity than the one the question asked for.
- If it is a **collider** (jointly caused by the treatment *and* by some outcome-related factor), conditioning on it *opens* a non-causal path between treatment and outcome and *induces* bias where none existed.

Either way the orthogonal moment is no longer estimating the total causal effect. **DML's guarantees assume the conditioning set is pre-treatment confounders only.** This is the formal reason the SCM must be drawn *first* (Chapters 5–6): the diagram decides which variables are admissible controls; the ML step only adjusts for them.

The one-sentence rule to drill: ***the diagram chooses the controls; the ML only adjusts for them.*** Orthogonality and cross-fitting protect you against *nuisance-estimation error.* They do nothing — *nothing* — against a *wrong conditioning set.* Garbage graph in, precise estimate of the wrong quantity out. A tight confidence interval around a biased number is the signature failure mode, and it is worse than an honest wide one because it manufactures false confidence.

The primary sources for the caveat are not new: **Montgomery, Nyhan & Torres (2018)**, *AJPS* 62(3):760–775 `[verify exact pages]`, show that conditioning on post-treatment variables ruins even a clean *randomized experiment*, because such variables can be affected by treatment. The deeper structural root is **Rosenbaum (1984)**, *JRSS-A* 147(5):656–666 `[verify pages]` — the consequences of adjusting for a concomitant variable that has itself been affected by the treatment.

> **Term box.**
> *CATE, τ(x):* the treatment effect for units with covariates x.
> *Honest splitting:* using disjoint subsamples to choose splits vs. estimate leaf effects.
> *Neyman orthogonality:* a score first-order insensitive to nuisance-estimation error.
> *Cross-fitting:* estimating nuisances and the target moment on disjoint folds.
> *Post-treatment variable:* a variable caused by the treatment — never an admissible control for the total effect.
> *Mediator:* a post-treatment variable on the causal path (sequence → dwell → Rx).
> *Collider:* a variable jointly caused by two others; conditioning on it opens a spurious path.

### 4.5 DML and causal forests are one family

It helps to see these two projects as two faces of one idea rather than two unrelated methods. Both rest on the same **orthogonalization / residualization backbone.** Modern causal forests (GRF; `econml`'s `CausalForestDML`) build the residual-on-residual ("R-learner") moment directly into the forest's local estimating equation — so they inherit DML-style robustness to nuisance-model error *and* deliver a heterogeneous θ(x). Cross-fitting (DML's bias-removal device) and honesty / out-of-bag subsampling (the forest's bias-removal device) play the same role. Teach them as one family: DML gives you a clean *average* parameter; the causal forest gives you a clean *heterogeneous* one; both depend, identically and absolutely, on a correctly specified adjustment set.

---

## 5. Worked example on this dataset (full run, including dead ends → lesson → limit)

### Project 2 — Meals (fully public data)

**Step 1 — Build the public dataset.** Pull CMS Open Payments general payments, filter to **meals** by NPI and time; link to **Medicare Part D** prescribing by NPI for the promoted drug class. This is exactly the DeJong (2016) linkage — fully public, no partner access.

```python
import pandas as pd

payments = pd.read_csv("openpayments_general.csv")
meals = payments[payments["nature_of_payment"] == "Food and Beverage"]
meals_by_npi = (meals.groupby(["covered_recipient_npi", "drug"])
                     .agg(meal_count=("record_id", "size"),
                          meal_value=("total_amount_usd", "sum"))
                     .reset_index())

partd = pd.read_csv("partd_prescriber.csv")   # NPI-level Rx by drug
df = partd.merge(meals_by_npi, how="left",
                 left_on=["npi", "drug"],
                 right_on=["covered_recipient_npi", "drug"])
df["treated"] = (df["meal_count"].fillna(0) >= 1).astype(int)   # D
df["y_rel"]   = df["brand_rx"] / (df["brand_rx"] + df["class_alt_rx"])  # relative prescribing of X
```

**Step 2 — Build pre-treatment covariates only.** Specialty, baseline prescribing, region, patient volume, a local competitive-detailing proxy. *Pre-treatment only* — the same discipline as Project 3.

```python
X = df[["specialty", "baseline_rx", "region", "patient_volume", "competitor_detailing"]]
X = pd.get_dummies(X, columns=["specialty", "region"], drop_first=True)
D = df["treated"]
Y = df["y_rel"]
```

**Step 3 — Fit the causal forest.**

```python
from econml.dml import CausalForestDML
from sklearn.ensemble import RandomForestRegressor, RandomForestClassifier

cf = CausalForestDML(
    model_y=RandomForestRegressor(n_estimators=500),
    model_t=RandomForestClassifier(n_estimators=500),
    discrete_treatment=True,
    cv=5,                 # cross-fitting
    n_estimators=2000,
    honest=True,
)
cf.fit(Y, D, X=X)
tau_hat = cf.effect(X)                 # per-physician CATE
lb, ub  = cf.effect_interval(X)        # pointwise CIs
```

In R, `grf::causal_forest(X, Y, W = D)` plus `predict(..., estimate.variance = TRUE)` is the canonical equivalent.

**Step 4 — Read heterogeneity.** Which specialties, baseline tiers, and competitive contexts show the largest meal effect? This is where the average meal effect (think DeJong's odds ratios) becomes a *segmentation* — the way an average churn effect of −10.5% can hide enterprise (0%) and small-business (−15%) segments, the average meal effect hides the physicians the meal actually moves.

```python
import numpy as np
df["tau"] = tau_hat
print(df.groupby("specialty_bucket")["tau"].mean().sort_values())
# feature importance for WHERE the effect varies:
print(cf.feature_importances_)
```

**Dead end #1 (teach it).** The naive instinct is to "isolate" the meal effect by *controlling for in-visit engagement or post-meal reaction.* That is the collider error of §4.4 — do not. Those are post-treatment.

**Dead end #2 (teach it).** Reading the CATE heterogeneity as *proof* of pathway — "the high-frequency-meal physicians respond more, therefore it's reciprocity." It is not proof (§4.2). The forest found a heterogeneity map; the pathway label is an assumption.

**Lesson.** The forest gives you heterogeneity *and* valid pointwise confidence intervals on a per-physician effect — the persuadables input. Pathway attribution is a separate, assumption-laden step requiring the Chapter 5 graph and Chapter 8 rep knowledge.

**Limit (be honest about the ceiling).** Open Payments meals are *observational.* DeJong, Yeh, and the Mitchell systematic review are *explicitly associational* — they document association and decline to claim causation. The causal forest adjusts for *measured* pre-treatment covariates only; residual confounding by physician disposition (a privacy-skeptical, industry-wary physician both refuses meals and prescribes conservatively) remains. **Project 2 is a *heterogeneity upgrade* on an associational base, not a clean identification** — unlike the cohort-rollout quasi-experiment of Chapters 4 and 10. Say this out loud in any readout.

### Project 3 — Slide sequencing (synthetic / proprietary)

The synthetic dataset plants a known ground-truth effect of slide ordering on NRx, and plants the collider structure too (dwell and reaction are generated *downstream* of the sequence). So we can watch the correct and incorrect specifications, and see exactly how far the wrong one drifts from truth.

**Step 1 — Specify the SCM first.** This is the whole assignment. Before any code, mark each variable:

- Treatment D = slide-sequence representation from `Call_Clickstream_vod` / `Display_Order_vod`.
- **Admissible controls Z = pre-treatment confounders ONLY:** physician baseline prescribing, territory, rep tenure, prior message history.
- **Inadmissible (post-treatment colliders/mediators):** `Duration_vod` (dwell), `Reaction_vod`, any in-visit signal.

```python
Z = ["baseline_rx", "territory", "rep_tenure", "prior_message_history"]   # pre-treatment ONLY
forbidden = ["dwell_seconds", "reaction_score"]                            # post-treatment — DO NOT CONDITION
```

**Step 2 — Run DML, correctly specified.**

```python
from econml.dml import LinearDML
from sklearn.ensemble import GradientBoostingRegressor

dml = LinearDML(
    model_y=GradientBoostingRegressor(),
    model_t=GradientBoostingRegressor(),
    cv=5,                       # cross-fitting
)
dml.fit(Y, D_seq, X=None, W=df[Z])    # W = controls chosen by the DIAGRAM
theta_correct = dml.coef_
ci_correct    = dml.coef__interval()
```

**Step 3 — The dead end, run on purpose.** Add the colliders to the control set "because they're predictive":

```python
dml_bad = LinearDML(model_y=GradientBoostingRegressor(),
                    model_t=GradientBoostingRegressor(), cv=5)
dml_bad.fit(Y, D_seq, X=None, W=df[Z + forbidden])   # WRONG: post-treatment in W
theta_biased = dml_bad.coef_
ci_biased    = dml_bad.coef__interval()
```

**Step 4 — Compare to ground truth.** Because the synthetic truth is known, you can plot both estimates against it:

```python
print("truth:   ", TRUE_SEQUENCE_EFFECT)
print("correct: ", theta_correct, ci_correct)   # ≈ truth, honest CI
print("biased:  ", theta_biased,  ci_biased)    # drifts from truth, deceptively TIGHT CI
```

The biased estimate moves *away* from the planted truth — and its confidence interval is often *tighter* than the correct one. That is the §3 disaster made concrete and measurable: not a noisier estimate, a *confidently wrong* one.

**Lesson.** Orthogonality + cross-fitting protect against *nuisance-estimation* error, not against a *wrong conditioning set.* The diagram is the load-bearing decision. The synthetic-first demonstration turns "colliders are bad" from a slogan into a number you watched drift.

**Limit.** High-dimensional treatment with sequence structure is the highest-ceiling, highest-difficulty project, and it needs proprietary or synthetic data — the firewall. The sequence-as-treatment framing is the book's methodological stretch (`[verify — author's extension]`); the synthetic-recovery test is what justifies it.

---

## 6. Common misconceptions

- **"Control for engagement to clean up the estimate."** The single most dangerous instinct in this chapter. Dwell and reaction are post-treatment colliders/mediators; conditioning on them biases the effect and DML reports that bias *precisely.*
- **"DML / cross-fitting fixes confounding — so I can throw in all my features."** No. DML fixes *nuisance-estimation* error given a *correct* adjustment set. It cannot fix a wrong adjustment set; the diagram chooses the set.
- **"The causal forest discovered that meals work through reciprocity."** No. The forest found heterogeneity. Pathway attribution is an assumption requiring the Chapter 5 graph and rep elicitation.
- **"Open Payments meals → prescribing is a causal estimate."** It is associational. The forest upgrades it to *heterogeneous* association after adjusting for measured covariates; residual confounding remains.
- **"A tight confidence interval means a trustworthy estimate."** A tight CI around a structurally biased number is worse than an honest wide one — it is false confidence.

---

## 7. Evidence check + consent/compliance & patient-welfare check

**Evidence check.** Methods are *independent, peer-reviewed, and method-stable*: Wager–Athey (2018), Athey–Tibshirani–Wager (2019) for causal forests/GRF; Chernozhukov et al. (2018) for DML; Montgomery–Nyhan–Torres (2018) and Rosenbaum (1984) for the post-treatment-conditioning caveat. The meals *empirics* are explicitly **associational, not causal** — DeJong et al. (2016, *JAMA Intern Med* 176(8):1114–1122), Yeh et al. (2016, *JAMA Intern Med* 176(6):763–768), and the Mitchell et al. (2021, *Ann Intern Med* 174(3):362–367 `[verify vol/pages]`) systematic review all report associations and decline causation. DeJong's odds ratios — rosuvastatin OR 1.18 (95% CI 1.17–1.18), nebivolol OR 1.70 (1.69–1.72), olmesartan OR 1.52 (1.51–1.53), desvenlafaxine OR 2.18 (2.13–2.23), across 279,669 physicians, 95% of payments being meals under ~$20 — are the public backbone of Project 2, and the textbook's contribution is the *heterogeneity upgrade*, not a claim of clean identification. Software (`grf`, `econml`, `causalml`) is actively maintained; versions carry `[verify]`.

**Consent / compliance & patient-welfare check.** Three live issues. (1) **Pathway and regulation:** the reciprocity pathway is the regulatorily fraught one; the educational pathway is the legally drivable one. Because the forest *cannot* separate them on its own, any meals-based targeting that optimizes a total meal effect risks optimizing reciprocity — which counsel would not bless. The pathway decomposition (Chapters 5, 8, 13) is a compliance prerequisite, not a nicety. (2) **The collider has a consent cousin:** the post-treatment collider taught here is structurally the same hazard as the *consent opt-out* selection collider of Chapter 13 — conditioning on a variable shaped by treatment or by selection both open spurious paths. (3) **Patient welfare stays primary:** the only defensible thing to *cause* is better-informed, on-label prescribing. A heterogeneity map that identifies "persuadable" physicians is ethically neutral only if the persuasion is educational; using it to amplify reciprocity is exactly what the regulatory line forbids.

---

## 8. Named Fellow artifact

**Artifact A (Project 2 — public-data, gradeable for everyone): the causal-forest heterogeneous meal-effects notebook.** On Open Payments + Part D: build the meal treatment and relative-prescribing outcome; assemble a *pre-treatment-only* covariate set; fit a causal forest; produce per-physician CATEs with pointwise CIs; report the heterogeneity map (by specialty, baseline tier, competitive context); and write the honest readout — including the explicit statement that pathway attribution is an assumption and that the base is associational.

**Artifact B (Project 3 — synthetic, demonstrates the collider discipline): the DML-with-SCM-first notebook.** Specify the SCM *first* (mark every variable pre/post-treatment in writing). Run DML with the correct pre-treatment-only adjustment set; recover the synthetic ground-truth sequence effect. Then run DML *with* dwell/reaction added to the controls and show the estimate drift away from truth with a deceptively tight CI. The deliverable is the side-by-side `truth / correct / biased` comparison plus the one-paragraph explanation of *why* — orthogonality protects against nuisance error, not against a wrong conditioning set.

Together these are the Week-11 portfolio: one project every Fellow can run on public data, one that *demonstrates the firewall and the collider discipline* on synthetic ground truth.

---

## 9. Research finding for Fellows

The firewall is real and it falls exactly along the public/proprietary line. **Project 2 is fully public-data-runnable** — Open Payments meals linked to Part D prescribing reproduce DeJong's linkage and upgrade it to a heterogeneous causal-forest estimate that no published meals study has produced. **Projects 1 and 3 are not** — cohort timing, `Call2_Key_Message_vod__c`, and the `Call_Clickstream_vod` sequence are partner-owned and run only on proprietary or synthetic data. This is the first chapter where a Fellow *without* partner access hits the wall, and it is by design: the synthetic dataset, with its known ground truth, is what lets a Fellow *prove their method recovers truth* before trusting it on data whose truth is hidden. The deeper finding is methodological and transferable: the per-physician CATE *is* the product — heterogeneity, not the average, is what makes targeting tractable — and the collider discipline is what stands between a precise estimate and a precisely wrong one.

---

## 10. Exercises

1. **(Apply)** On Open Payments + Part D, build the meal treatment and relative-prescribing outcome for one drug class. Fit a causal forest and report the overall (averaged) effect alongside the heterogeneity by specialty. Confirm your covariate set contains *only* pre-treatment variables and justify each inclusion in one line.

2. **(Apply+ — produces artifact)** Extend Exercise 1 into Named Artifact A: per-physician CATEs with pointwise CIs, a heterogeneity map across specialty × baseline tier × competitive context, and an honest readout that explicitly labels pathway attribution as an assumption and the base as associational.

3. **(Evaluate — the collider demonstration, produces artifact)** On the synthetic clickstream data, run DML twice — once with the correct pre-treatment-only adjustment set, once with dwell + reaction added. Plot both against the known ground truth. Report how far the biased estimate drifts and whether its CI is *tighter* than the correct one. Write the paragraph explaining why cross-fitting did not save it.

4. **(Apply+)** Take the per-physician meal CATEs from Exercise 2 and rank physicians by responsiveness. Compare the top decile to a baseline-volume ranking. How different are the two lists? (This is the bridge to Chapter 12's persuadables-vs-sure-things distinction.)

5. **(Evaluate)** Write a one-page memo to a stakeholder who proposes "adding reaction score to the model because it's our most predictive feature." Explain — using the SCM and the post-treatment-collider argument — why this would corrupt the estimate, and what the tight CI would falsely signal.

---

## 11. When to use AI (five-part block)

**1. When to use AI.** Use an LLM to scaffold `grf` / `econml` / `causalml` code, translate an R causal-forest workflow into Python, and explain the residual-on-residual mechanics of DML and the honest-splitting logic of forests. Use it to draft the plain-language explanation of *why* a tight CI around a biased number is worse than an honest wide one — a useful framing for stakeholders.

**2. When NOT to use AI.** Do **not** ask the model to "pick the best controls" or "select the most predictive features" — it will happily hand you the colliders, because predictiveness is exactly the wrong criterion. The adjustment set is a *causal-diagram* decision, not a feature-selection one. Do not let it attribute a pathway ("the meal worked through reciprocity") from heterogeneity alone. Do not let it certify that your conditioning set is confounder-only — *you* mark every variable pre/post-treatment, by hand, against the SCM.

**3. LLM exercise (ready to paste).**

```
I am estimating the causal effect of in-visit slide ORDERING on a
physician's downstream prescribing, using Double Machine Learning.
My dataset has these columns:
  - slide_sequence (the treatment; high-dimensional ordering)
  - downstream_nrx (the outcome)
  - baseline_rx, territory, rep_tenure, prior_message_history
  - dwell_seconds_per_slide, reaction_score (logged DURING the visit)

1. Which of these variables are admissible as controls for estimating
   the TOTAL causal effect of slide ordering, and which are NOT? For
   each inadmissible one, state whether it is a mediator or a collider
   and what conditioning on it would do to my estimate.
2. Give me econml code (LinearDML, cross-fitting) using ONLY the
   admissible controls.
3. Explain why DML's Neyman-orthogonality and cross-fitting do NOT
   rescue me if I include the inadmissible variables.
4. Tell me what symptom I would (wrongly) take as reassurance if I
   made the mistake — and why that symptom is misleading.

Do not optimize my controls for predictive accuracy. Choose them by
causal admissibility only.
```

**4. CLI exercise (Claude Code).** In a repo with the synthetic clickstream data, have Claude Code: (a) generate the DML pipeline with an explicit, commented SCM block that lists every variable as pre- or post-treatment; (b) write a `pytest` that *asserts the correctly-specified DML recovers the synthetic ground-truth sequence effect within tolerance* and *asserts the collider-contaminated specification fails that tolerance*; (c) emit a comparison plot. The test that proves "correct recovers truth, collider-contaminated does not" is the validation artifact.

**5. AI validation.** Validation is *recovery on known ground truth*, not plausibility. For Project 3, the correctly specified DML must recover the planted synthetic effect inside its CI, and the collider-contaminated run must *visibly miss* it — that contrast is the proof the method (and the LLM's code) is right. For Project 2, there is no ground truth (real public data), so validation is *honesty discipline*: confirm the covariate set is pre-treatment-only and that the readout labels the estimate associational and the pathway an assumption.

---

## 12. AI Use Disclosure

This chapter was drafted with AI assistance for prose structuring and code scaffolding. Method citations (Wager–Athey, Athey–Tibshirani–Wager, Chernozhukov et al., Montgomery–Nyhan–Torres, Rosenbaum) and the meals empirics (DeJong, Yeh, Mitchell) were verified against the Chapter 11 research notes and not generated by the model; `[verify]` tags are carried forward where the notes flagged them (Open Payments thresholds, exact page ranges, `grf` version, the sequence-as-treatment extension). The bonus-earning disclosure for a Fellow: name the one edge orientation or pathway assumption — e.g., "high-frequency meals act through reciprocity rather than education" — that the causal forest *could not* supply and that required the Chapter 5 graph or rep structural knowledge.

---

## 13. What would change my mind

I would weaken the chapter's framing if: (a) on real proprietary clickstream data, the slide-sequence treatment proves so collinear with pre-treatment confounders that DML cannot identify a sequence effect at all even with the correct adjustment set — making Project 3 a non-starter regardless of the collider discipline; (b) a credible argument or experiment showed that in-visit reaction is in fact *pre-treatment* relative to the prescribing decision (it is not, under the SCM here, but the SCM is an assumption); or (c) the causal-forest heterogeneity on Open Payments turned out to be entirely driven by residual confounding rather than real effect modification — which a sensitivity analysis (e.g., Rosenbaum bounds) could partly probe but not settle. None of these would overturn the methods; they would relocate where the honest line falls.

---

## 14. Still puzzling

- What is the right *representation* of a high-dimensional sequence treatment? "Which slides in what order with loops/skips" is not a scalar; embedding it for DML (n-grams? a learned sequence embedding?) is genuinely open, and the embedding choice could itself smuggle in post-treatment information. This is the book's flagged extension and it is not fully solved.
- Can pathway separation (reciprocity vs education) ever be *identified* rather than *assumed*? Mediation analysis needs strong sequential-ignorability assumptions; the rep-elicitation route (Chapter 8) supplies structural priors but not proof. The honest answer is "assumed, with the assumption made explicit and sourced to rep knowledge."
- How much residual confounding survives in the Open Payments forest? Without a quasi-experiment, the per-physician CATE is an adjusted association. How wrong could it be, and would a Rosenbaum-bounds sensitivity analysis change which physicians land in the persuadable decile?

---

## 15. Summary

Projects 2 and 3 answer the two questions the average effect of Chapter 10 cannot. The causal forest (Project 2) estimates a *per-physician* meal effect — the CATE, τ(x) — on fully public Open Payments + Part D data, with honest splitting and valid pointwise confidence intervals; its heterogeneity map is the persuadables input Chapter 12 consumes, but it *cannot* by itself attribute the effect to reciprocity vs education — that is an assumption requiring the graph and rep knowledge, and the base estimate is associational, not identified. Double Machine Learning (Project 3) estimates the causal effect of the high-dimensional slide-sequence treatment — but only if the structural causal model is specified *first.* The in-visit signals that look most useful as controls — dwell, reaction — are post-treatment colliders/mediators; conditioning on them corrupts the estimate, and DML's orthogonality and cross-fitting, which protect against nuisance-estimation error, do *nothing* against a wrong conditioning set. The signature failure is a tight confidence interval around a biased number, worse than an honest wide one. The two methods are one family — both rest on orthogonalized residual-on-residual estimation — and both stand or fall on a correct adjustment set chosen by the diagram, not the algorithm. The synthetic dataset, with known ground truth, is what lets a Fellow *prove* the correct specification recovers truth while the collider-contaminated one does not. And the firewall is concrete: Project 2 is public for everyone; Project 3 needs proprietary or synthetic data.

---

## 16. Key terms

- **Conditional average treatment effect (CATE), τ(x):** the treatment effect for units with covariates x — the per-physician number.
- **Causal forest:** a random-forest method estimating τ(x) with honest splitting and valid pointwise CIs (Wager–Athey).
- **Generalized Random Forests (GRF):** forests as adaptive local weighting for any local moment condition (Athey–Tibshirani–Wager).
- **Double Machine Learning (DML):** estimation of a low-dimensional causal parameter with ML nuisances via Neyman-orthogonal scores and cross-fitting (Chernozhukov et al.).
- **Neyman orthogonality:** a score first-order insensitive to nuisance-estimation error.
- **Cross-fitting:** estimating nuisances and the target moment on disjoint folds.
- **Post-treatment collider / mediator:** a variable caused by the treatment; never an admissible control for the total effect.
- **Pathway attribution:** assigning an effect to reciprocity vs education — an assumption, not a forest output.
- **Public-data/IP firewall:** Project 2 public-runnable; Projects 1 & 3 proprietary/synthetic.

---

## 17. Bridge

You now have, per physician, a *causal responsiveness* number — from the meal forest and (on partner data) the sequence DML — and you have the discipline to keep those numbers honest. But an estimate is not yet a decision. Chapter 12 turns the per-physician CATE into targeting: it distinguishes the **persuadables** (physicians the right message at the right time actually moves) from the **sure-things** (high-propensity prescribers who would prescribe anyway — pure budget waste), and reframes detailing allocation as constrained portfolio optimization on causal responsiveness rather than baseline volume. The forest's τ(x) is the input; the persuadables ranking is what the partner ships.

---

## 18. Further reading

- Wager, S. & Athey, S. (2018). "Estimation and Inference of Heterogeneous Treatment Effects Using Random Forests." *JASA* 113(523):1228–1242. DOI 10.1080/01621459.2017.1319839. — Causal forests; honest splitting; valid pointwise CIs.
- Athey, S., Tibshirani, J. & Wager, S. (2019). "Generalized Random Forests." *Annals of Statistics* 47(2):1148–1178. DOI 10.1214/18-AOS1709. — GRF; forest as adaptive local weighting.
- Chernozhukov, V., Chetverikov, D., Demirer, M., Duflo, E., Hansen, C., Newey, W. & Robins, J. (2018). "Double/Debiased Machine Learning for Treatment and Structural Parameters." *Econometrics Journal* 21(1):C1–C68. DOI 10.1111/ectj.12097 (NBER w23564). — Orthogonality + cross-fitting + partialling-out.
- Montgomery, J., Nyhan, B. & Torres, M. (2018). "How Conditioning on Posttreatment Variables Can Ruin Your Experiment and What to Do about It." *AJPS* 62(3):760–775 `[verify exact pages]`. DOI 10.1111/ajps.12357. — The post-treatment-conditioning caveat.
- Rosenbaum, P. (1984). "The Consequences of Adjustment for a Concomitant Variable That Has Been Affected by the Treatment." *JRSS-A* 147(5):656–666 `[verify pages]`. — The foundational structural root.
- DeJong, C. et al. (2016). "Pharmaceutical Industry-Sponsored Meals and Physician Prescribing Patterns for Medicare Beneficiaries." *JAMA Internal Medicine* 176(8):1114–1122. DOI 10.1001/jamainternmed.2016.2765. — The public-data meals backbone (associational).
- Yeh, J. S. et al. (2016). "Association of Industry Payments to Physicians With the Prescribing of Brand-name Statins in Massachusetts." *JAMA Internal Medicine* 176(6):763–768. DOI 10.1001/jamainternmed.2016.1709. — Associational dose-response.
- Mitchell, A. P. et al. (2021). "Are Financial Payments From the Pharmaceutical Industry Associated With Physician Prescribing? A Systematic Review." *Annals of Internal Medicine* 174(3):362–367 `[verify vol/pages]`. DOI 10.7326/M20-5665. — Consistent association; not definitive causation.
- Software: `grf` (R; grf-labs), `econml` (Python; py-why — `CausalForestDML`, `LinearDML`, `DRLearner`), `causalml` (Python; Uber — meta-learners, uplift). Chen et al. (2020) arXiv:2002.11631.
- In-library: `_lib_ai-applied-causal-inference-powered-by-ml-and-ai.md` — primary library coverage of causal forests, DML, and heterogeneous treatment effects.
