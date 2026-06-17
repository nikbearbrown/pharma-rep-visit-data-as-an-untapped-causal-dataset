# Chapter 12 Notes — From Propensity to Persuadables

*Research-gathering notes for Week 12. Grounded in `rep-visit-as-causal-dataset_source.md`
("The targeting reframe — persuadables, not sure-things"), the Living Model framework, and the
library/web research below. NOT chapter prose. Sourced or `[verify]`-flagged.*
*(Supersedes an earlier thin stub of this file; rewritten to A–G format with verified citations.)*

**Chapter scope (from TIKTOC Week 12):** Rank physicians by **causal responsiveness**, not baseline
volume. The "sure-things" problem; uplift/persuadables; treatment-oriented allocation as **constrained
portfolio optimization**; how the Living-Model target list **differs systematically** from the NBA
propensity list (a testable, productizable claim).

---

## SECTION A — FOUNDATIONS, MISCONCEPTION, WORKED EXAMPLE, SOURCE

### A.1 The reframe: propensity vs persuadability

**Propensity** answers a Rung-1 question: *who is most likely to prescribe?* (P(NRx | X)). It is what
every NBA engine ranks on. **Persuadability** answers a Rung-2/3 question: *for whom does the message
**cause** a change?* (the CATE, τ(x) = E[Y(1) − Y(0) | X = x] — the same per-physician number the Ch 11
causal forest produces). These are *different orderings of the same population.* The high-propensity
physician who would have prescribed anyway is a **sure thing**: targeting her burns budget without
causing anything.

**The four-quadrant uplift taxonomy** (cross treatment-response with no-treatment-response):
- **Persuadables** — prescribe *only if* treated. The entire ROI of detailing lives here.
- **Sure Things** — prescribe regardless. Targeting = pure waste (the propensity trap).
- **Lost Causes** — never prescribe, treated or not. Wasted contact.
- **Sleeping Dogs / Do-Not-Disturbs** — respond *negatively* to treatment (detailing annoys them /
  triggers reactance — e.g., the industry-skeptical physician, who also tends to opt out: links to the
  consent collider in Ch 13). Targeting actively backfires.

NBA ranks by propensity → preferentially selects Sure Things (high baseline volume). The Living Model
ranks by τ(x) → selects Persuadables. **The lists differ. That difference is the product.**

### A.2 THE CENTRAL MISCONCEPTION

> **"A model that predicts prescribers well tells you who to detail."** Propensity ≠ persuadability.
> The best-predicted prescribers are disproportionately Sure Things; ranking on them spends the most
> budget on the physicians who would prescribe with no contact at all.

The canonical academic statement of this error is **Ascarza (2018), "Retention Futility"** (*JMR*):
targeting customers by churn *risk* (propensity) is ineffective because **risk and responsiveness are
distinct and weakly correlated** — firms should target on *heterogeneity in treatment responsiveness*,
not on the risk score. Direct transfer: ranking physicians by prescribing *propensity* (the NBA list)
is the pharma analog of targeting high-churn-risk customers — the wrong target. This is the single
strongest citation for the chapter's thesis. (TIKTOC Part 2 misconception #1: "propensity ≠
persuadability; the sure-things are waste.")

### A.3 Treatment-oriented allocation as constrained portfolio optimization

Detailing is a **scarce resource** (rep hours, sample budget, meal budget, call-frequency caps). The
allocation problem is: choose which physicians to treat, and at what intensity, to **maximize total
caused prescribing subject to a budget/capacity constraint.** This is a constrained optimization /
knapsack-style problem where the "value" of treating physician *i* is her CATE τ(x_i) (the *caused*
increment), not her predicted volume.

- **Why "portfolio":** like asset allocation, you distribute a fixed budget across units with
  heterogeneous expected (causal) returns and constraints; you want the highest *incremental* return
  per unit of resource, not the highest *level*. (Library analog: `finance-advances-in-financial-
  machine-learning.md` on portfolio optimization / allocation under constraint — the framing analogy,
  not the math; HRP/Markowitz allocate by risk-adjusted return, here we allocate by causal return.)
- **Constraints are real, not decorative:** rep capacity, call-frequency caps, compliance rules,
  territory, channel preference, and — load-bearing — the **patient-welfare gate** and the
  **educational-pathway-only** regulatory line (Ch 13). A high-CATE physician can be *excluded* if the
  pathway driving the effect is reciprocity rather than education. So the objective is not "rank by CATE
  and detail the top": it is "maximize caused, *legally drivable* prescribing under capacity."
- **Qini / uplift-curve evaluation:** the standard way to score the allocation is the **Qini curve**
  (cumulative incremental gain as you treat the top-ranked fraction by predicted uplift, vs random) and
  the **Qini coefficient** — the uplift field's analog of AUC. This is how you *demonstrate* the
  Living-Model list beats the NBA list: a higher Qini = more caused prescribing per physician contacted.
- **Living-Model connection:** the source essay frames the Rung-3 payoff as "for THIS physician, which
  message next week" (individual counterfactual). The portfolio optimization is the *aggregation* of
  those individual counterfactuals under constraint — the operational targeting list.

### A.4 How the Living-Model list differs from the NBA propensity list (the testable claim)

The source essay's productizable claim: **the targeting list a Living Model produces differs
systematically from the NBA list.** Concretely, persuadables tend to be:
- **mid-tier prescribers** (not the top-volume Sure Things the NBA over-selects),
- with a **specific clinical concern** the right message addresses,
- in **markets peer influence hasn't yet reached** (pre-saturation — where a message can still move
  the needle vs. where the decision is already locked).

This is *falsifiable*: hold out a randomized slice, score both lists by realized incremental NRx (Qini),
show divergence + Living-Model superiority. Ch 12's named artifact is the causal-responsiveness ranking
+ allocation; the divergence-from-NBA comparison is the research finding.

### A.5 Worked example sketch (process incl. dead end → lesson → limit)

1. Take the per-physician CATE τ(x_i) from the Ch 11 causal forest (meals project, public data) or the
   Ch 9 SCM counterfactual engine.
2. Rank by τ(x_i) (persuadability), not by predicted volume (propensity). Concretely: a decile-10
   loyalist has high predicted NRx but ~zero incremental response (Sure Thing); a decile-5 physician
   with an access/safety concern has lower baseline but high τ for the matching message (Persuadable).
3. Solve the constrained allocation: select physicians by marginal caused return until rep-capacity /
   budget exhausted; intensity ∝ marginal caused return; *exclude* reciprocity-driven and negative-τ
   physicians.
4. **Dead end to teach:** rank by predicted NRx and "go where the volume is." On the synthetic dataset
   (known ground truth) this list concentrates on Sure Things and yields *less caused prescribing* than
   the persuadables list at the same budget. Second dead end: rank by CATE *alone*, ignoring the
   pathway/welfare/compliance gate — ships a high-CATE-but-reciprocity-driven target that Ch 13 kills.
5. Compare lists: quantify overlap (low) and Qini gap (Living-Model > NBA).
6. **Lesson:** budget should chase the *increment*, not the *level* — and only the *legally drivable*
   increment. Two different populations.
7. **Limit:** τ(x) is estimated with uncertainty; ranking on noisy CATEs can mis-rank near the
   persuadable/sure-thing boundary. Sleeping Dogs (negative τ) must be *excluded*, not merely
   deprioritized — which requires detecting negative effects (needs power).

**Primary source anchor:** `rep-visit-as-causal-dataset_source.md`, "The targeting reframe —
persuadables, not sure-things."

---

## SECTION B — EXAMPLES + FAILURE CASE

**Positive examples / evidence:**
- **Ascarza (2018), Retention Futility (*JMR* 55(1): 80–98):** two field experiments + ML show high-risk
  customers are *not* the best retention targets; target on responsiveness heterogeneity. The empirical
  proof that propensity is the wrong target. (2018 Paul E. Green Award.)
- **Uplift four-quadrant practice:** political-campaign microtargeting popularized "persuadables"
  (Obama 2012 / Analyst Institute lineage — practitioner provenance `[verify]`, not a citable academic
  origin). The negative-uplift "Sleeping Dogs" concept traces to Radcliffe's direct-marketing uplift
  work (cleaner academic lineage).
- **Message-specific targeting:** one physician is persuadable by access education, another by safety
  data — the action is *message-specific*, not generic visit/no-visit. Persuadability is a function of
  the (physician, message) pair, which is why the Ch 9 SCM (not a scalar score) is the right substrate.
- **HTE-decision analog (living-models lib):** the bank credit-limit example — ATE = +$240/mo hides
  Segment A (+$580), B ($0), C (−$150). Acting on the average rolls out to everyone including the
  negative segment; acting on the CATE targets only A. The persuadables-vs-sure-things logic in a
  non-pharma setting.

**The canonical FAILURE CASE (chapter opens on it):**
The **high-propensity prescriber who would have prescribed anyway** — selected by the NBA engine,
detailed repeatedly, "converts," and the engine claims credit. Budget burned; *nothing was caused.*
The metric (conversion among targeted) looks great precisely because the target was guaranteed to
convert. The better the NBA model predicts, the more efficiently it wastes money on people who needed
no contact.

Secondary failure: targeting Sleeping Dogs — detailing the industry-skeptical physician *reduces*
prescribing (reactance) and pushes her toward opt-out (feeding the Ch 13 consent collider). A
propensity model is blind to this; only a *signed* CATE catches it.

---

## SECTION C — CONNECTIONS

- **← Ch 11 (causal forest):** the per-physician CATE is the *input* to this chapter. Persuadability =
  the forest's τ(x). Ch 11 estimates it; Ch 12 ranks and allocates on it.
- **← Ch 9 (Living Model / counterfactual engine):** the Rung-3 "which message for THIS physician"
  counterfactual is the per-unit value; Ch 12 aggregates these under constraint.
- **← Ch 3 (Pearl's ladder):** propensity = Rung 1; persuadability = Rung 2/3. The chapter is the
  ladder cashed out as a targeting decision.
- **→ Ch 13 (consent + ethics):** Sleeping Dogs ↔ opt-out-prone physicians; the negative-uplift
  population overlaps the consent collider. The patient-welfare gate and the educational-pathway-only
  line are *constraints in the optimization*, not afterthoughts.
- **Portfolio analog:** `finance-advances-in-financial-machine-learning.md` portfolio-optimization
  framing (allocation under constraint by risk-adjusted return) — borrowed as analogy; here the
  "return" is caused prescribing and the constraint is rep capacity + compliance.
- **Contrast with NBA vendor tools (Part 4 positioning):** NBA optimizes engagement proxies on Rung 1;
  this chapter is where the book's central thesis becomes a concrete, different output (the list).

---

## SECTION D — CURRENT STATE / REFS / LAST-3-YR

**Uplift / persuadables (primary + canonical):**
- Ascarza, E. (2018). "Retention Futility: Targeting High-Risk Customers Might Be Ineffective."
  *Journal of Marketing Research* 55(1): 80–98. DOI 10.1509/jmr.16.0163. — **the** propensity-is-the-
  wrong-target result.
- Radcliffe, N. J. & Surry, P. D. (2011). "Real-World Uplift Modelling with Significance-Based Uplift
  Trees." Stochastic Solutions / Portrait Technical Report TR-2011-1. — significance-based uplift trees;
  negative-uplift ("Sleeping Dogs") handling.
- Radcliffe, N. J. (2007). "Using control groups to target on predicted lift: Building and assessing
  uplift models." *Direct Marketing Analytics Journal* (DMA). — origin of the **Qini** measure
  `[verify exact title/vol/pages]`.
- Gutierrez, P. & Gérardy, J.-Y. (2017). "Causal Inference and Uplift Modelling: A Review of the
  Literature." *PMLR* 67: 1–13. — frames uplift as CATE estimation; Two-Model / Class-Transformation /
  direct-uplift families.
- Four-quadrant taxonomy (persuadables / sure-things / lost-causes / sleeping-dogs): well-documented in
  uplift literature; clean citable secondary statement = scikit-uplift docs, "Types of customers."
  Political-campaign "persuadables" provenance `[verify]` (journalistic, not academic origin).
- **Qini / AUUC:** Qini coefficient = area between Qini curve and random diagonal; the uplift analog of
  AUC. Standard evaluation metric for ranking-by-uplift.

**CATE machinery (shared with Ch 11):** Wager & Athey (2018, *JASA*); Athey-Tibshirani-Wager (2019,
*Ann. Stat.*, GRF); Chernozhukov et al. (2018, DML); Nie & Wager R-learner ("Quasi-oracle estimation
of heterogeneous treatment effects") `[verify cite]`. Bottou et al. on counterfactual reasoning /
computational advertising `[verify cite]` — the ad-targeting precedent for treatment-effect-based
allocation.

**Software (last-3-yr):**
- **causalml (Uber, Python)** — uplift trees/forests, meta-learners (S/T/X/R), Qini/AUUC evaluation.
  Chen et al. (2020) arXiv:2002.11631.
- **EconML (py-why, Python)** — CATE estimation feeding the ranking (`CausalForestDML`, DRLearner,
  metalearners).
- **scikit-uplift** — uplift-specific modeling + Qini/uplift-curve utilities.

**Portfolio-optimization framing (analogy only):**
- `finance-advances-in-financial-machine-learning.md` — constrained allocation / HRP / Markowitz
  critical-line algorithm (library). Conceptual analogy for budget allocation by causal return.

**Settled vs contested:** *Settled* — treatment-effect heterogeneity matters for targeting/policy
assignment; propensity ≠ responsiveness (Ascarza). *Contested/emerging* — whether HCP targeting systems
can *operationalize* causal responsiveness in production depends on data access, design quality, and
compliance acceptance; commercial deployment still lags prediction-based NBA workflows.

**Aging risk:** the persuadables *concept* and the Ascarza result are stable; specific NBA-vendor
practices and the empirical list-divergence claim should be re-tested on current partner data.

---

## SECTION E — TEACHING

- **Open on the Sure Thing.** The high-propensity prescriber who converts and was never going to do
  otherwise — make the Fellow feel the wasted budget before naming the quadrant.
- **Analogy:** don't water the plants that are already watered; water the ones that respond.
- **Drill the two-orderings idea:** same physicians, two rankings (propensity vs CATE), low overlap.
  Have Fellows compute the overlap on the synthetic / Open-Payments data and *see* the divergence.
- **Make them compute a Qini, not just a list.** The deliverable is "here is the Qini curve showing
  this list beats the NBA list at the same budget." The metric *is* the argument.
- **Ascarza as the anchor reading** — assign it; it transfers cleanly from churn to detailing.
- **Negative uplift is not just low uplift.** Drill that Sleeping Dogs must be *excluded* (signed CATE),
  and tie to the opt-out / reactance physician → sets up Ch 13.
- **Portfolio framing keeps it honest about constraints.** Targeting is not "rank and detail everyone
  good"; it is allocate a *fixed* budget under compliance + welfare gates.
- **Bloom's:** (Analyze) distinguish sure-things from persuadables; (Evaluate) critique a propensity-
  ranked NBA target list; (Create) produce a causal-responsiveness ranking + constrained allocation.
- **Exercise:** given propensity and CATE columns, build two target lists, compute overlap + expected
  incremental value + Qini.

---

## SECTION F — LIBRARY FILES

Relevant `_lib_` files in `pantry/`:
- **`_lib_math-weapons-of-math-destruction-how-big-data-increases-inequality-and-threatens-dem.md`**
  — O'Neil; contains the "persuadables" discussion (22 hits on persuad/WMD/fairness/accountability)
  and the critique of optimizing-the-wrong-target / opaque scoring that the NBA-propensity critique
  leans on. Most relevant library file for this chapter.
- **`_lib_ai-applied-causal-inference-powered-by-ml-and-ai.md`** — HTE/CATE machinery that defines
  persuadability quantitatively (shared with Ch 11).
- **`finance-advances-in-financial-machine-learning.md`** (in `MD/`; copy into `pantry/` as `_lib_` if
  the portfolio framing earns chapter space) — portfolio optimization / constrained allocation analogy.

**Library-scan finding:** `persuadable` appears in only two library books —
`math-weapons-of-math-destruction...` (already copied as `_lib_`) and
`ai-co-intelligence-living-and-working-with-ai.md` (passing mention). `portfolio optimization` appears
substantively in `finance-advances-in-financial-machine-learning.md` (asset-allocation context — used
here as analogy only). The strongest persuadables/uplift sources are *web*-sourced (Ascarza, Radcliffe,
Gutierrez — Section D); the library supplies the *critical lens* (WMD), the web supplies the *method +
canonical result*.

---

## SECTION G — GAPS / FLAGS

- **[flag — list-divergence is a claim, not a proven fact.]** "The Living-Model list differs
  systematically from the NBA list" is the chapter's productizable hypothesis; *testable* (held-out Qini
  comparison) but unproven on this dataset. Present as a research finding to be demonstrated, not an
  established result. (TIKTOC Part 11: "a testable, productizable claim.")
- **[flag — propensity ≠ persuadability is the load-bearing misconception (TIKTOC Part 2 #1).]** The
  whole chapter rests on it; Ascarza is the academic warrant. Keep it prominent.
- **[verify]** Radcliffe (2007) exact bibliographic details (Qini origin); Nie & Wager and Bottou et al.
  exact cites. **[verify]** the Obama-2012 / Analyst Institute "persuadables" attribution — provenance is
  journalistic, not a citable academic origin; do not cite as primary.
- **Gap — CATE-ranking under uncertainty.** Ranking on *noisy* CATE estimates can mis-rank near the
  persuadable/sure-thing boundary; the chapter should flag that ranking ignores the CIs the causal
  forest provides. No clean source for ranking-on-noisy-CATE in this domain (`[verify]` / author
  judgment).
- **Gap — Sleeping Dogs detection needs power.** Detecting *negative* uplift requires adequate
  treated/control data; on public Open-Payments data this may be infeasible. Full four-quadrant
  separation may only be achievable on richer (proprietary/synthetic) data → ties to Ch 13 firewall and
  Risk 1.
- **Portfolio analogy caveat:** financial portfolio optimization assumes returns/covariances; the
  detailing analog has *causal* returns with their own estimation error and ethical constraints (Ch 13).
  Use the analogy for intuition, not as a literal Markowitz transfer.
