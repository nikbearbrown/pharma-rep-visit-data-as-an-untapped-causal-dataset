# `_lib_` excerpt — Difference-in-Differences (curated from library)

**Source:** `MD/ai-applied-causal-inference-powered-by-ml-and-ai.md`, Chapter 16 "Difference-in-Differences"
(Chernozhukov, Hansen, Kallus, Spindler, Syrgkanis — *Applied Causal Inference Powered by ML and AI*).
Raw text in the source is OCR-degraded; this is a **clean paraphrase of the load-bearing content** with
the chapter's own citations preserved. Use for Ch 10 notes. Anything paraphrased is from this chapter
unless marked `[verify]`.

---

## What the chapter establishes (canonical 2×2 DiD)

- DiD needs **two groups** (treated, control) over **two periods** (pre, post). The causal estimand is
  the **Average Treatment effect on the Treated (ATET / ATT)**.
- **Parallel trends assumption:** in expectation, the change in *untreated* potential outcomes would have
  been the same for the treated group as for the control group. Formally, the treated group's
  counterfactual change `E[Y₂(0) − Y₁(0) | D=1]` equals the observed control change
  `E[Y₂(0) − Y₁(0) | D=0]`. This lets you impute the unobserved treated-counterfactual change from the
  control group's observed change.
- **No-anticipation assumption:** period-2 treatment does not affect period-1 outcomes (often left
  implicit; the chapter states it for a clean estimand). Crucially, DiD *does* allow systematic level
  differences between groups pre-treatment — it differences them out.
- **Parallel trends is functional-form dependent:** if it holds for `Y` it need not hold for `log(Y)`.
  This is because differencing only eliminates confounding that is *additively separable* on that scale.
  One case where it holds on any transformation: when `D` is randomly assigned. (See also the
  *changes-in-changes* model of Athey & Imbens 2006 for a transformation-invariant alternative.)
- **ATET identification:** `ATET = [E(Y₂ − Y₁ | D=1)] − [E(Y₂ − Y₁ | D=0)]` — the "difference in
  differences."

## Conditional DiD + DML (the ML connection)

- Practitioners often want **conditional parallel trends**: parallel trends only after conditioning on
  covariates `X` (it's easier to believe two groups with identical observed characteristics share a
  trend). The chapter shows the **DML (double/debiased ML) machinery from Ch 9 applies directly**:
  flexibly model the outcome-change regression and/or the propensity, use doubly-robust/orthogonal
  moments, cross-fitting.
- Doubly-robust DiD estimand: **Sant'Anna & Zhao (2020)**, *Journal of Econometrics* 219(1):101–122.
- DML-for-DiD: **Chang (2020)**, *Econometrics Journal* 23(2):177–191; **Zimmert (2018)**, arXiv:1809.01643.
- Time-varying covariates in the parallel-trends assumption: **Caetano & Callaway (2023)**, arXiv:2202.02903.

## Staggered timing / modern DiD citations the chapter lists

(These are the references the book points to for going beyond canonical 2×2 — they are the spine of the
Ch 10 notes and are independently verified via web search; see notes Section D.)
- **Callaway & Sant'Anna (2021)**, "Difference-in-Differences with multiple time periods,"
  *Journal of Econometrics* 225(2):200–230. (arXiv:1803.09015; `did` R package.)
- **Callaway (2023)**, "Difference-in-Differences for Policy Evaluation," in *Handbook of Labor, Human
  Resources and Population Economics*, Springer.
- **Roth (2022)**, "Pre-test with Caution: Event-study Estimates After Testing for Parallel Trends,"
  *AER: Insights* 4(3):305–322.
- **Rambachan & Roth (2023)**, "A More Credible Approach to Parallel Trends" (Honest DiD),
  *Review of Economic Studies*.

## Why this matters for the pharma book

The cohort-rollout natural experiment (TIKTOC Ch 4 / Project 1) is **staggered**: training cohorts adopt
the new message deck at different calendar months. That is exactly the setting where the canonical 2×2 and
naive TWFE break (see notes Ch 10). The chapter gives the identification logic (parallel trends, ATET,
DML-conditioned version); the modern staggered-DiD papers give the estimator.
