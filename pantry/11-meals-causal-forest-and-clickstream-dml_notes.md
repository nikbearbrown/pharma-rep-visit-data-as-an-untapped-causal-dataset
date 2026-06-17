# Chapter 11 Notes — Projects 2 & 3: Meals (Causal Forest) and Slide Sequencing (DML)

*Research-gathering notes for Week 11. Grounded in `rep-visit-as-causal-dataset_source.md`
(the "Research agenda — 3 projects" section + the collider warning), the Living Model framework,
and the library/web research below. NOT chapter prose. Sourced or `[verify]`-flagged.*

**Chapter scope (from TIKTOC Week 11):** Two of the three research projects.
- **Project 2 — Meals (causal forest):** heterogeneous treatment effects of Open-Payments meals on
  prescribing, sliced by specialty / baseline / competition, to separate the **reciprocity** pathway
  from the **education** pathway. This is the *fully public-data-runnable* project (Open Payments + Part D).
- **Project 3 — Slide sequencing (DML):** the `Call_Clickstream_vod` slide-sequence treated as a
  high-dimensional treatment; Double Machine Learning for the causal effect of ordering on NRx —
  with the hard lesson that **the SCM must be specified BEFORE residualization**, because
  reaction/dwell are post-treatment colliders that corrupt the estimate.

---

## SECTION A — FOUNDATIONS, MISCONCEPTION, WORKED EXAMPLE, SOURCE

### A.1 Causal forests (Project 2)

**The core idea.** A causal forest estimates the **Conditional Average Treatment Effect (CATE)** —
the treatment effect for a unit with covariates *x*, written τ(x) = E[Y(1) − Y(0) | X = x] — rather
than a single average. It adapts the random forest from prediction to causal estimation. Two
load-bearing innovations:
- **Honest splitting:** one subsample decides where the tree splits; a *disjoint* subsample estimates
  the effect inside each leaf. This removes the bias that adaptive partitioning would otherwise inject
  and is what licenses valid inference.
- **Asymptotic normality + pointwise confidence intervals** for τ(x), via an infinitesimal-jackknife
  variance estimator. (Wager & Athey 2018 — first RF-based HTE method with formal inference guarantees.)

The estimand we want for Project 2 is the **CATE of receiving a promotional meal on prescribing the
promoted brand**, as a function of physician covariates: specialty, baseline prescribing volume,
local competitive detailing intensity, patient mix. The forest returns, for each physician, a
personalized "how much does a meal move *this* physician" number — which is exactly the persuadables
input Ch 12 consumes.

**Generalized Random Forests (GRF).** Athey, Tibshirani & Wager (2019) generalize causal forests:
the forest becomes an **adaptive local-weighting (kernel) scheme** for solving any *local moment
condition* E[ψ_{θ(x)}(O) | X = x] = 0. Causal-effect estimation, quantile regression, and IV
estimation all become special cases of the moment ψ. The `grf` R package is the canonical
implementation (`causal_forest`, `instrumental_forest`, etc.).

**Pathway separation (the meals-specific move).** The DAG (from the source essay) puts meals on the
**M3 reciprocity** pathway, distinct from **M1 educational**. A meal can move prescribing through
reciprocity (the $40B pathway, regulatorily fraught) OR through education (the legally drivable
pathway — the meal accompanied a substantive clinical presentation). The causal forest does not
*label* the pathway by itself; the separation is achieved by **how you specify covariates and
interpret heterogeneity**: e.g., heterogeneity by meal value and frequency (pure reciprocity signal)
vs. heterogeneity tied to a documented educational presentation / on-label message. The forest gives
you the heterogeneity map; the *pathway interpretation* is a modeling + elicitation decision (ties to
Ch 5 three-pathway graph and Ch 8 rep elicitation). **[flag]** the forest alone cannot prove which
pathway carried the effect — be explicit that pathway attribution is an identifying *assumption*
layered on the heterogeneity estimate, not an output of the forest.

### A.2 Double Machine Learning (Project 3)

**The core idea.** DML (Chernozhukov et al. 2018) estimates a low-dimensional causal parameter θ
(here: the effect of slide ordering on NRx) when the confounders are high-dimensional and you want to
adjust for them with flexible ML. Two essential ingredients:
- **Neyman orthogonality:** use a moment/score that is first-order insensitive to errors in the
  estimated nuisance functions, so regularization bias in the ML models does not contaminate θ.
- **Cross-fitting:** estimate nuisances on one fold, evaluate the target moment on the held-out fold,
  average. This breaks the dependence between nuisance-estimation error and the final estimate and
  avoids restrictive empirical-process (Donsker) conditions.

In the partially linear model the mechanics are **partialling-out / residual-on-residual regression**:
regress outcome Y on controls Z (ML) → residual Ỹ; regress treatment D on controls Z (ML) → residual
D̃; regress Ỹ on D̃ → θ̂. (Living-models lib gives the same three-box flow:
"Regress X on Z → X̃; Regress Y on Z → Ỹ; Regress Ỹ on X̃ → τ̂"; with the standing note that **Z is
chosen by the causal diagram (backdoor criterion), not by the ML step.**)

**The high-dimensional treatment twist (Project 3).** Here the *treatment itself* is high-dimensional:
the `Call_Clickstream_vod` slide sequence (which slides, in what order, with loops/skips encoded by
`Display_Order_vod`). DML's appeal is precisely that ML can carry the high-dimensional structure while
orthogonality keeps the causal target clean.

### A.3 THE CENTRAL MISCONCEPTION (the chapter's hinge)

> **"Slide dwell time and reaction score are useful features — control for them to clean up the
> estimate."** They are **post-treatment colliders.** Conditioning on them to estimate the
> message/sequence → NRx effect is a structural error that no amount of ML flexibility or
> cross-fitting can fix.

Why: `Duration_vod` (dwell), `Reaction_vod`, and other in-visit signals are *caused by* the treatment
(the slide sequence delivered) and also relate to the outcome. They sit on, or downstream of, the
causal path. Including them among the controls Z that DML residualizes on means:
- if the in-visit signal is a **mediator**, residualizing on it strips out part of the very effect you
  want (you estimate a controlled-direct effect, not the total effect);
- if it is a **collider** (jointly caused by treatment and an outcome-related factor), conditioning
  *opens* a non-causal path and induces bias.

Either way the orthogonal moment is no longer estimating the total causal effect. **DML's guarantees
assume the conditioning set is pre-treatment confounders only.** This is the formal reason the SCM
must be drawn *first* (Ch 5–6): the diagram decides which variables are admissible controls; the ML
step only adjusts for them. "Garbage graph in → precise estimate of the wrong quantity out" — DML
produces a *tight confidence interval around a biased number*, which is worse than an honest wide one.

**Primary source for the caveat:** Montgomery, Nyhan & Torres (2018), *AJPS* — conditioning on
post-treatment variables ruins even a clean randomized experiment, because such variables can be
affected by treatment (mediator/collider). Deeper structural root: Rosenbaum (1984), *JRSS-A* — the
consequences of adjusting for a concomitant variable affected by the treatment.

### A.4 Worked example sketch (on this dataset; process incl. dead ends → lesson → limit)

**Project 2 (meals, public data).**
1. Pull CMS Open Payments general payments, filter to **meals** by NPI/time; link to **Medicare Part D**
   prescribing by NPI for the promoted drug class (this is exactly the DeJong 2016 linkage — fully
   public). Treatment D = received ≥1 sponsored meal for drug X in window; outcome Y = relative
   prescribing of X vs. class alternatives.
2. Build covariate set X (pre-treatment only): specialty, baseline prescribing, region, patient
   volume, local competitive detailing proxy.
3. Fit a **causal forest** (`grf::causal_forest` or EconML `CausalForestDML`) → CATE per physician.
4. Read heterogeneity: which specialties / baseline tiers / competitive contexts show the largest
   meal effect. Map high-frequency / high-value-meal heterogeneity toward the **reciprocity** reading;
   education-linked heterogeneity toward **M1**.
5. **Dead end to teach:** the naive instinct is to *control for in-visit engagement or post-meal
   reaction* to "isolate" the effect — that is the collider error from A.3; do not. Another dead end:
   reading CATE heterogeneity as proof of pathway — it is not (A.1 flag).
6. **Lesson:** the forest gives heterogeneity + valid pointwise CIs; pathway attribution is an
   assumption requiring the Ch 5 graph + Ch 8 rep knowledge.
7. **Limit:** Open Payments meals are *observational* — confounding by physician characteristics
   remains; the forest adjusts for *measured* covariates only (DeJong is explicitly associational).

**Project 3 (clickstream, synthetic/proprietary).**
1. **Specify the SCM first** (Ch 5–6): mark `Duration_vod`/`Reaction_vod`/in-visit signals as
   post-treatment; admissible controls Z = pre-treatment confounders only (physician baseline,
   territory, rep tenure, prior message history).
2. Treatment D = slide-sequence representation from `Call_Clickstream_vod`/`Display_Order_vod`.
3. Run **DML** (cross-fitting; ML nuisances) on Ỹ ~ D̃.
4. **Dead end to teach:** add dwell/reaction to Z "because they're predictive" → biased θ̂ with a
   deceptively tight CI. Show the recovery failure on the synthetic dataset (known ground truth).
5. **Lesson:** orthogonality + cross-fitting protect against *nuisance-estimation* error, not against
   a *wrong conditioning set*. The diagram is the load-bearing decision.
6. **Limit:** high-dim treatment + sequence structure is the highest-ceiling, highest-difficulty
   project; needs proprietary or synthetic data (the firewall).

**Primary source anchor:** `rep-visit-as-causal-dataset_source.md`, "Research agenda (3 projects)"
items 2 & 3, and the "Collider warning" in "The data as a Living Model problem."

---

## SECTION B — EXAMPLES + FAILURE CASE

**Worked positive examples:**
- **Meals → prescribing association (DeJong 2016, JAMA Intern Med):** receiving even a single
  sponsored meal promoting a target brand was associated with higher prescribing of that brand vs.
  class alternatives. Odds ratios: rosuvastatin OR 1.18 (95% CI 1.17–1.18); nebivolol OR 1.70
  (1.69–1.72); olmesartan OR 1.52 (1.51–1.53); desvenlafaxine OR 2.18 (2.13–2.23). 279,669 physicians;
  95% of payments were meals, mean < $20. **The authors' own caveat: association, not causation,
  cross-sectional.** This is the public-data backbone of Project 2 — and the textbook upgrade is to go
  from this association to a *heterogeneous causal estimate* via causal forest.
- **HTE motivating analogy (living-models lib):** the SaaS price-increase / credit-limit examples —
  ATE = −10.5% churn hides enterprise (0%) vs small-business (−15%); the *average hides the
  segmentation that makes the decision tractable.* Same logic: an average meal effect hides which
  physicians the meal actually moves.

**The canonical FAILURE CASE (the chapter opens on it):**
A slide-sequencing "optimization" that **conditions on engagement** (dwell/reaction) to clean its
estimate — and thereby **blocks its own causal path / opens a collider path.** It reports a precise,
confident, wrong effect of ordering on NRx. This is the source essay's opening image for Week 11 and
the live demonstration of the Ch 6 collider discipline. The failure is *invisible in the metrics* —
the CI is tight — which is exactly why it's dangerous.

Secondary failure: treating CATE heterogeneity as *proof* of the reciprocity-vs-education split
(A.1 flag) — confusing a heterogeneity map with a mediation decomposition.

---

## SECTION C — CONNECTIONS

- **← Ch 5 (three-pathway graph):** meals = M3 reciprocity; the pathway separation Project 2 attempts
  is the Ch 5 graph made quantitative.
- **← Ch 6 (collider trap):** Project 3 *is* Ch 6 in action — the dwell/reaction collider discipline
  is the whole lesson. The chapter cannot be understood without Ch 6.
- **← Ch 9 (parameterized SCM / Living Model):** both projects feed the SCM; the CATE per physician is
  the counterfactual-engine input.
- **→ Ch 12 (persuadables):** the causal forest's per-physician CATE *is* the responsiveness ranking
  Ch 12 turns into targeting. Project 2 → persuadables directly.
- **→ Ch 13 (firewall + consent):** Project 2 is public-data-runnable (Open Payments); Projects 1 & 3
  need proprietary or synthetic data — this chapter is where the IP firewall becomes concrete. Also:
  the consent collider (Ch 13) is a *selection* analog of the post-treatment collider taught here.
- **DML ↔ causal forest unity:** both share the **orthogonalization/residualization** backbone.
  Modern causal forests (GRF; EconML `CausalForestDML`) build the residual-on-residual ("R-learner")
  moment into the forest's local estimating equation, so they inherit DML-style robustness to nuisance
  models *and* deliver heterogeneous θ(x). Cross-fitting (DML) and honesty/out-of-bag subsampling
  (forests) play the same bias-removal role. Teach them as one family, not two methods.
- **Companion books:** general HTE/DML theory lives in the *Causal Reasoning* companion + the
  `_lib_ai-applied-causal-inference` file; this chapter teaches only what these two projects need.

---

## SECTION D — CURRENT STATE / REFS / LAST-3-YR

**Methods (stable, primary):**
- Wager, S. & Athey, S. (2018). "Estimation and Inference of Heterogeneous Treatment Effects Using
  Random Forests." *JASA* 113(523): 1228–1242. DOI 10.1080/01621459.2017.1319839. — causal forests,
  honest splitting, valid pointwise CIs.
- Athey, S., Tibshirani, J. & Wager, S. (2019). "Generalized Random Forests." *Annals of Statistics*
  47(2): 1148–1178. DOI 10.1214/18-AOS1709. — GRF; forest as adaptive local-weighting for local moments.
- Chernozhukov, V., Chetverikov, D., Demirer, M., Duflo, E., Hansen, C., Newey, W. & Robins, J.
  (2018). "Double/Debiased Machine Learning for Treatment and Structural Parameters."
  *The Econometrics Journal* 21(1): C1–C68. DOI 10.1111/ectj.12097 (NBER w23564). — orthogonality +
  cross-fitting + partialling-out.
- Montgomery, J., Nyhan, B. & Torres, M. (2018). "How Conditioning on Posttreatment Variables Can
  Ruin Your Experiment and What to Do about It." *AJPS* 62(3): 760–775 `[verify exact pages]`.
  DOI 10.1111/ajps.12357. — the collider/post-treatment caveat.
- Rosenbaum, P. (1984). "The Consequences of Adjustment for a Concomitant Variable That Has Been
  Affected by the Treatment." *JRSS-A* 147(5): 656–666 `[verify pages]`. — foundational structural root.

**Software (last-3-yr, actively maintained):**
- **`grf` (R)** — grf-labs; canonical GRF implementation (`causal_forest`, `instrumental_forest`,
  multi-arm/multi-outcome). Maintainer Erik Sverdrup et al. `[verify current version/date]`.
- **EconML (Python)** — Microsoft Research ALICE team, now under py-why. DML/orthogonal-ML family:
  `LinearDML`, `SparseLinearDML`, `CausalForestDML`, `DRLearner`, OrthoForest, S/T/X-learners, IV.
  `CausalForestDML` = DML residualization + causal-forest final stage, CIs via Bootstrap-of-Little-Bags.
- **causalml (Python)** — Uber; CATE + uplift; meta-learners (S/T/X/R), DR/TMLE learners, uplift
  trees/forests (KL, Euclidean, Chi-square split criteria). Ref: Chen et al. (2020) arXiv:2002.11631.

**Meals / Open Payments empirical (Project 2 substrate):**
- DeJong et al. (2016), *JAMA Intern Med* 176(8): 1114–1122. DOI 10.1001/jamainternmed.2016.2765.
- Yeh et al. (2016), *JAMA Intern Med* 176(6): 763–768. DOI 10.1001/jamainternmed.2016.1709 —
  MA statins; each +$1,000 in payments ≈ +0.1 pp brand-name prescribing; educational-training payments
  ≈ +4.8 pp. Associational.
- Mitchell et al. (2021), *Ann Intern Med* 174(3): 362–367 `[verify vol/pages]`. DOI 10.7326/M20-5665 —
  systematic review: payments consistently associated with increased/costlier prescribing; literature
  "consistent with but [does] not definitively prove" causation.
- (Larkin 2017 DiD belongs to Project 1 / Ch 10 but is the causal-design counterpoint — see Ch 13/10.)

**Aging risk:** methods + the collider argument are stable; Open Payments coverage/thresholds and Veeva
field names drift (verify each cycle — see Ch 13 notes for current thresholds).

---

## SECTION E — TEACHING

- **Lead with the failure, not the method.** Open on the engagement-conditioned slide-sequencing
  optimization that confidently reports the wrong number. Let the Fellow feel the seduction of "control
  for engagement" before naming it a collider error.
- **The one-sentence rule to drill:** *"The diagram chooses the controls; the ML only adjusts for
  them."* DML/forests protect against nuisance-estimation error, never against a wrong conditioning set.
- **Synthetic-first verification:** because the synthetic dataset has known ground-truth structure,
  have Fellows run DML *with* and *without* the dwell/reaction collider in Z and watch the estimate
  move away from truth — the bias is then concrete, not abstract.
- **Causal forest as the persuadables bridge:** frame the per-physician CATE explicitly as "the number
  Ch 12 ranks on." Heterogeneity is not a curiosity; it is the product.
- **Project 2 is the safe sandbox:** fully public (Open Payments + Part D), so every Fellow can run it
  end-to-end regardless of partner-data access. Make it the graded project for those without proprietary
  access.
- **Honesty about pathway attribution:** repeatedly distinguish "the forest found heterogeneity" from
  "the meal worked through reciprocity." The second is an assumption + rep-knowledge claim.
- **Bloom's:** (Create) causal-forest CATE on Open-Payments meals; (Apply) DML on clickstream with SCM
  specified first; (Evaluate) explain why naive conditioning on reaction/dwell corrupts the estimate.

---

## SECTION F — LIBRARY FILES

Relevant `_lib_` files already present in `pantry/` (copied in prior library scans):
- **`_lib_ai-applied-causal-inference-powered-by-ml-and-ai.md`** — primary library coverage of causal
  forests, DML, and heterogeneous treatment effects (57 hits on those terms). The single most relevant
  library file for this chapter.
- **`_lib_math-causal-inference.md`** and
  **`_lib_math-causal-inference-mit-press-essential-knowledge-series.md`** — foundational causal-inference
  framing (backdoor, adjustment, identification).
- **`_lib_science-the-book-of-why-the-new-science-of-cause-and-effect.md`** — Pearl on colliders,
  do-operator, why adjustment sets matter (supports the SCM-before-residualization argument).
- **`_lib_math-weapons-of-math-destruction-...md`** — for the "precise but wrong / confidently
  biased model" failure framing (more central to Ch 13, but the WMD lens on opaque confident models
  fits the collider failure case here).

**Library-scan finding (`MD/` grep):** the only library book with substantive causal-forest / DML /
HTE content is `ai-applied-causal-inference-powered-by-ml-and-ai.md` (already copied). No library book
covers Open Payments or the Sunshine Act — those are web-sourced (Section D). Portfolio-optimization
material in the library (`finance-advances-in-financial-machine-learning.md`) is about asset allocation
(HRP/Markowitz), tangential — routed to Ch 12, not here.

The living-models framework file itself (`living-models.md`) contains a clean DML three-box flow
diagram and the SaaS/credit-limit HTE worked examples — reuse-ready and cited above.

---

## SECTION G — GAPS / FLAGS

- **[flag — pathway attribution is an assumption, not a forest output.]** The causal forest gives
  heterogeneous τ(x); it does NOT prove the effect ran through reciprocity vs education. Separating M3
  from M1 requires the Ch 5 graph + Ch 8 rep elicitation layered on top. Do not let the chapter imply
  the forest "discovers" the pathway. (Mirrors TIKTOC Part 11: "meals cause prescribing — association-
  strong, causal-contested; separate reciprocity vs education; cite primary causal work, `[verify]`.")
- **[flag — public-data causality ceiling.]** Project 2 on Open Payments is observational; DeJong/Yeh
  are explicitly associational. The causal forest adjusts for *measured* pre-treatment covariates only;
  residual confounding by physician disposition remains. Be honest that Project 2 is a *heterogeneity*
  upgrade on an associational base, not a clean identification (unlike the cohort-rollout IV of Ch 4/10).
- **[flag — firewall boundary made concrete here.]** Project 3 (clickstream) needs proprietary or
  synthetic data; only Project 2 is public-runnable. This is the first chapter where a Fellow without
  partner-data access hits the wall — gates assessment design (Ch 13 firewall). Connect to Risk 1.
- **[verify]** current Open Payments meal-reporting thresholds and field availability (Section D /
  Ch 13). **[verify]** `grf` current version; Montgomery-Nyhan-Torres and Rosenbaum exact page ranges.
- **Gap:** no library source for the *high-dimensional-treatment* flavor of DML (treatment = a sequence).
  Project 3's framing of `Call_Clickstream_vod` as a high-dim treatment is novel to this book — the
  standard DML literature treats high-dim *confounders*, not high-dim *treatments*. Flag as an open
  methodological stretch; consider citing the sequence-as-treatment as `[verify — author's extension]`
  and validating recovery on the synthetic dataset.
