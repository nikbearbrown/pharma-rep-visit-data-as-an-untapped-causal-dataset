# Chapter 10 — Project 1: Staggered DiD on Message-Variant Transitions (research notes)

*The most feasible, highest-value study: staggered difference-in-differences / event study on
message-variant transitions, using the training-cohort rollout. The TWFE bias problem under staggered
adoption; Callaway–Sant'Anna (2021) and the related modern estimators (Sun–Abraham, Goodman-Bacon,
de Chaisemartin–D'Haultfœuille); treatment-on-the-treated; required data; falsification-first discipline.
Gather-only.*

Sources: `pantry/rep-visit-as-causal-dataset_source.md` (§"The natural experiment in every training
rollout," §"Research agenda" Project 1); TIKTOC WEEK 4 + WEEK 10; `_lib_did-applied-causal-inference_excerpt.md`
(library DiD chapter); `living-models.md` (ladder, identification framing); web search (all modern DiD
papers verified, cited inline). `[verify]` where unpinned.

---

## A. Foundations

**The natural experiment.** A new clinical message (e.g., efficacy-first → safety-first deck) rolls out
through **training cohorts** sequenced by region / geography / hiring calendar — timing set by admin
logistics, not by physician characteristics. Group A (trained month 1) delivers the new deck; comparable
Group B (trained month 2) is still delivering the old deck in that window. Physicians didn't choose their
message; they got it because of *when their rep trained*. This is **staggered adoption**: different cohorts
"turn on" the treatment at different times. (Source essay §"The natural experiment"; TIKTOC Ch 4.)

**The estimand: treatment-on-the-treated (ATT/ATET).** DiD identifies the average effect of the message
variant *on the physicians whose reps actually delivered it* — not a population ATE. From
`_lib_did-applied-causal-inference_excerpt.md`: under parallel trends + no-anticipation,
`ATET = [E(ΔY | treated)] − [E(ΔY | control)]`. Here ΔY = change in NPI-level NRx (30/60/90-day windows
per the source); treated = physicians of cohort-A reps; control = physicians of not-yet-trained reps.

**Parallel trends (the load-bearing assumption).** In expectation, treated physicians' NRx *would have*
evolved the same as control physicians' NRx absent the new message. Two riders from the library chapter:
(a) DiD *allows* arbitrary pre-existing level differences between groups — it differences them out; it does
*not* allow differing trends. (b) Parallel trends is **functional-form dependent** — holding for NRx need
not imply holding for log(NRx); choose the scale deliberately and defend it. (Caetano-Callaway 2023 extend
to time-varying covariates in the parallel-trends assumption.)

**The TWFE bias problem (the chapter's pivot and misconception).** The intuitive move is a **two-way
fixed-effects** regression: NRx on a treatment dummy + physician/territory FE + time FE. *This is biased
under staggered adoption with heterogeneous treatment effects.* The mechanism, now well-established:
- **Goodman-Bacon (2021)** decomposes the TWFE DiD estimator into a *weighted average of all possible 2×2
  DiD comparisons* between groups and periods. The poison: some of those comparisons use **already-treated
  units as controls for later-treated units** ("forbidden comparisons"). When treatment effects change over
  time, those comparisons get the sign of the dynamic effect *backwards*, and the weights can be
  **negative** — so TWFE can return a negative coefficient even when *every* unit-level effect is positive.
  ([Goodman-Bacon 2021, *J. Econometrics* / NBER w25018](https://www.nber.org/papers/w25018).)
- **de Chaisemartin & D'Haultfœuille (2020)** prove the same pathology generally: TWFE estimates a weighted
  sum of ATEs with weights that may be negative, so the coefficient can be negative while all ATEs are
  positive. ([AER 2020](https://www.aeaweb.org/articles?id=10.1257/aer.20181169);
  [arXiv:1803.08807](https://arxiv.org/abs/1803.08807).)
- **Sun & Abraham (2021)** show the *event-study* version of the bug: in a TWFE regression with leads/lags,
  the coefficient on a given relative-time lag is **contaminated by effects from other periods**, and
  apparent **pre-trends can be a pure artifact of treatment-effect heterogeneity** — not evidence of a real
  parallel-trends violation. ([*J. Econometrics* 2021](https://arxiv.org/abs/1804.05785).)

This is the chapter's central teaching inversion (and TIKTOC's "TWFE-bias trap").

### The modern estimators (what to use instead)
- **Callaway & Sant'Anna (2021)** — the workhorse. Define **group-time average treatment effects**
  `ATT(g,t)` = the effect for cohort first-treated at time *g*, evaluated at time *t*. Estimate each by a
  clean 2×2 DiD against a valid control (never-treated, or not-yet-treated), using **outcome regression,
  inverse-probability weighting, or doubly-robust** estimands (and conditional parallel trends on
  covariates). Then **aggregate** the `ATT(g,t)` into the summary you want — overall ATT, dynamic/event-study
  by exposure length, by calendar time, or by cohort. No forbidden comparisons; no negative weights.
  ([*J. Econometrics* 225(2):200–230](https://www.sciencedirect.com/science/article/abs/pii/S0304407620303948);
  [arXiv:1803.09015](https://arxiv.org/abs/1803.09015); `did` R package,
  [bcallaway11.github.io/did](https://bcallaway11.github.io/did/).)
- **Sun & Abraham (2021)** — the **interaction-weighted (IW)** estimator: compute cohort-specific
  relative-time effects (CATT), aggregate with sample-share weights → uncontaminated event-study
  coefficients. Good when you want the dynamic path and have a clean never-treated (or last-treated) control.
- **de Chaisemartin & D'Haultfœuille** — `did_multiplegt` family; robust to heterogeneity, handles
  treatments that turn on *and off* (relevant if a message deck is rolled back) — the source's variant
  transitions can be non-absorbing.
- **Goodman-Bacon (2021)** — primarily a **diagnostic**: the Bacon decomposition tells you *how much* of a
  naive TWFE estimate comes from forbidden (already-treated-as-control) comparisons. Run it to show the bug
  before switching estimators.

### The falsification-first discipline (the chapter's spine)
TIKTOC Ch 10 Opening: "the falsification test that must pass before any effect is believed." Concretely
(source essay §"natural experiment," three IV/DiD conditions):
1. **Independence / no-selection-on-trends:** regress cohort assignment on *pre-rollout* prescribing.
   A significant coefficient → cohort timing is correlated with physician characteristics → the experiment
   is dead. (The source frames this as the IV-independence falsification; in DiD it's the parallel-trends /
   pre-trends test.)
2. **Pre-trends / event-study leads:** estimate `ATT(g,t)` for *pre-treatment* periods (relative time < 0).
   They should be ≈ 0. **But** heed Sun-Abraham (apparent pre-trends can be heterogeneity artifacts under
   TWFE — use the clean estimator) **and** Roth (2022): pre-testing for parallel trends and then conditioning
   on passing it distorts inference; the test is underpowered and selection-biased. Modern best practice:
   report the event-study leads *and* a sensitivity analysis.
3. **Honest sensitivity:** **Rambachan & Roth (2023) "Honest DiD"** — instead of assuming exact parallel
   trends, bound how large a post-period violation could be (relative to observed pre-period deviations) and
   report which conclusions survive. ([*Review of Economic Studies* 2023](https://www.econometricsociety.org).)
4. **Exclusion (from the IV framing):** cohort affects NRx *only* through the message — threatened if early
   training also improves general selling skill; defend by controlling rep tenure + baseline rep performance.

### Required data (the panel)
From source essay Project 1: `Call2_Key_Message_vod__c` (the delivered message variant), **training-cohort
timing** (from HR / rollout calendar — the staggered "treatment time" *g*), **NPI-level NRx panel**
(IQVIA Xponent / Symphony), **territory fixed effects**, rep tenure + baseline performance (exclusion
controls). Outcome windows: 30/60/90-day NRx post-transition. Synthetic-first: TIKTOC firewall — Project 1
needs proprietary or synthetic data (vs Project 2/meals which is fully public via Open Payments).

### Misconception (to invert)
"Staggered rollout? Just throw in unit + time fixed effects and read the treatment coefficient (TWFE)."
**Wrong, and confidently wrong:** under staggered timing + heterogeneous/dynamic effects, TWFE mixes in
forbidden already-treated-as-control comparisons with negative weights and can flip the sign. Use
group-time ATT (Callaway–Sant'Anna) or interaction-weighted (Sun–Abraham); use Goodman-Bacon to *show* the
contamination. (TIKTOC Ch 10 Core; TIKTOC Part 2 misconception family.)

### Worked example (on this dataset)
Schematic: cohorts g = {Jan, Feb, Mar} adopt the safety-first deck; never-trained-in-window reps = control.
(1) Run Bacon decomposition on a naive TWFE → display the share of weight on forbidden comparisons.
(2) Estimate `ATT(g,t)` with Callaway–Sant'Anna, doubly-robust, conditional on tenure + baseline NRx +
territory; control group = not-yet-treated. (3) Falsify first: pre-treatment `ATT(g,t)` ≈ 0 + Honest-DiD
sensitivity band. (4) Aggregate to dynamic event-study (NRx lift at 30/60/90 days) and to overall ToT.
Report the ToT *with the parallel-trends assumption and the sensitivity bound stated.* (Numbers to be
generated on the synthetic dataset — `[verify]`.)

### Source anchor
TIKTOC WEEK 4 (IV setup + falsification) + WEEK 10 (Project 1 run); source essay §"natural experiment" +
Project 1; `_lib_did-applied-causal-inference_excerpt.md`.

---

## B. Examples + failure case

**Positive analogues.**
- The library DiD chapter's **minimum-wage / county-employment DML-DiD** worked example (conditional
  parallel trends estimated with double ML) — the template for the *conditional* version Fellows will need
  (parallel trends only after conditioning on tenure/baseline/territory).
- **Callaway–Sant'Anna `did` package vignettes** — direct, runnable templates for `ATT(g,t)` + aggregation
  ([multi-period-did vignette](https://bcallaway11.github.io/did/articles/multi-period-did.html)).

**Failure cases.**
1. **Sign flip from TWFE** (the canonical disaster): a brand concludes the new deck *hurt* prescribing
   because forbidden comparisons + negative weights flipped the estimate, kills a deck that actually worked.
2. **Believing a pre-trend artifact** (Sun-Abraham): an analyst sees nonzero TWFE event-study leads,
   declares parallel trends violated, abandons a valid design — when the "pre-trend" was pure heterogeneity
   contamination.
3. **Pre-test distortion** (Roth 2022): conditioning the analysis on having *passed* a low-power
   parallel-trends test biases the reported effect.
4. **Conditioning on a post-treatment collider** (cross-link to Ch 6): "controlling for engagement / dwell"
   inside the DiD reopens a closed path and corrupts the message→NRx estimate — the same collider discipline
   Ch 6 teaches, now inside Project 1.

---

## C. Connections

- **← Ch 4:** the IV framing of the same cohort rollout (relevance/independence/exclusion); Ch 10 runs the
  DiD/event-study version.
- **← Ch 6:** collider discipline — don't condition on dwell/reaction inside the DiD.
- **← Ch 9:** the population ToT from Project 1 parameterizes the SCM (the "identified effects" ingredient);
  Rung-2 → Rung-3 happens in Ch 9.
- **→ Ch 11:** Projects 2 (meals / causal forest, public-data-runnable) and 3 (slide-sequence DML); Ch 10 is
  the most feasible of the three.
- **→ Ch 12:** ToT and dynamic effects feed the persuadables ranking.
- **→ Ch 13:** consent/selection (opt-out) interacts with the panel's sampling frame; the falsification
  discipline + assumption-stating is the regulatory/credibility backbone.
- **Framework:** the falsification-first, assumptions-stated-every-chapter discipline is the Living Model
  book's house style (`living-models.md`).

---

## D. Current state / refs / last-3-yr (all web-verified)

**The modern staggered-DiD canon:**
- **Goodman-Bacon, A. (2021).** "Difference-in-Differences with Variation in Treatment Timing."
  *J. Econometrics* 225(2):254–277. [NBER w25018](https://www.nber.org/papers/w25018). — the decomposition;
  forbidden comparisons; negative weights.
- **Callaway, B. & Sant'Anna, P.H.C. (2021).** "Difference-in-Differences with Multiple Time Periods."
  *J. Econometrics* 225(2):200–230.
  [ScienceDirect](https://www.sciencedirect.com/science/article/abs/pii/S0304407620303948) /
  [arXiv:1803.09015](https://arxiv.org/abs/1803.09015). — group-time ATT; never/not-yet-treated controls;
  DR/IPW/OR; aggregation schemes; `did` package.
- **Sun, L. & Abraham, S. (2021).** "Estimating Dynamic Treatment Effects in Event Studies with
  Heterogeneous Treatment Effects." *J. Econometrics* 225(2):175–199.
  [arXiv:1804.05785](https://arxiv.org/abs/1804.05785). — contamination of event-study lags; IW estimator;
  pre-trend artifacts.
- **de Chaisemartin, C. & D'Haultfœuille, X. (2020).** "Two-Way Fixed Effects Estimators with Heterogeneous
  Treatment Effects." *AER* 110(9):2964–2996.
  [AER](https://www.aeaweb.org/articles?id=10.1257/aer.20181169) /
  [arXiv:1803.08807](https://arxiv.org/abs/1803.08807). — negative-weights result; `did_multiplegt`.
- **Roth, J. (2022).** "Pre-test with Caution: Event-Study Estimates After Testing for Parallel Trends."
  *AER: Insights* 4(3):305–322. — pre-testing distortion.
- **Rambachan, A. & Roth, J. (2023).** "A More Credible Approach to Parallel Trends" (Honest DiD).
  *Review of Economic Studies*. — sensitivity bounds instead of exact parallel trends.
- **Roth, Sant'Anna, Bilinski & Poet (2023).** "What's Trending in Difference-in-Differences? A Synthesis
  of the Recent Econometrics Literature." *J. Econometrics* 235(2):2218–2244.
  [arXiv:2201.01194](https://arxiv.org/abs/2201.01194). — **the practitioner survey + recommendation set;
  the single best "current state" cite.**
- **Roth & Sant'Anna (2023).** "When Is Parallel Trends Sensitive to Functional Form?" *Econometrica*
  91(2):737–747. — the functional-form rider made rigorous.

**Doubly-robust / DML-DiD (from the library chapter):** Sant'Anna & Zhao (2020) *J. Econometrics*
219(1):101–122; Chang (2020) *Econometrics Journal* 23(2):177–191; Zimmert (2018) arXiv:1809.01643;
Caetano & Callaway (2023) arXiv:2202.02903; Wooldridge — "Two-Way Mundlak" regression (TWFE-as-Mundlak)
`[verify]` exact title/year.

**Software:** R `did` (Callaway–Sant'Anna), `fixest::sunab` (Sun–Abraham IW), `did_multiplegt` /
`DIDmultiplegtDYN` (de Chaisemartin–D'Haultfœuille), `bacondecomp` (Goodman-Bacon diagnostic),
`HonestDiD` (Rambachan–Roth).

---

## E. Teaching

- **Assessed (Week 10/11, Project Run, 200 pts):** implement the staggered-DiD/event-study design on a
  synthetic or partner panel; **run the falsification test first**; report the effect *with assumptions
  stated*. (TIKTOC Part 7; Ch 10 LOs.)
- **Bloom:** Create (implement staggered DiD/IV on the panel) → Analyze (run falsification first) →
  Evaluate (report ToT + assumptions + sensitivity).
- **Exercise seeds:** (1) run a naive TWFE and a Bacon decomposition on the synthetic data; quantify the
  forbidden-comparison weight and explain the resulting bias; (2) re-estimate with Callaway–Sant'Anna and
  compare; (3) build the event-study with Sun–Abraham, then add an Honest-DiD sensitivity band; (4) write the
  one-paragraph "why the falsification had to pass first" memo.
- **Five-part AI block:** *Use AI* to scaffold `did`/`fixest`/`bacondecomp` code and explain estimator
  choice; *Don't* let it pick the estimator or declare parallel trends satisfied; *Validation* = recover the
  known synthetic ground-truth ToT, and confirm TWFE *fails* on the same data as a teaching artifact.

---

## F. Library files

- `_lib_did-applied-causal-inference_excerpt.md` — **primary** library support: canonical 2×2 DiD, parallel
  trends, ATET identification, functional-form dependence, conditional-DiD-via-DML, and the chapter's own
  pointers to Callaway, Roth, Sant'Anna-Zhao.
- `MD/ai-applied-causal-inference-powered-by-ml-and-ai.md` — Ch 16 (DiD) full text (OCR-degraded; the
  `_lib_` file is the clean version) + Ch 8–9 (DML, used for conditional parallel trends).
- `MD/ai-ml-assisted-adjustment.md` — adjacent ML-for-causal-adjustment material `[verify]` relevance.
- In-repo: `living-models.md` Ch 5 (ladder — DiD is a Rung-2 device), Ch 8 (effect estimation).
- *No dedicated econometrics/DiD monograph in `MD/`* — the modern staggered-DiD canon (Section D) is
  web-sourced and verified.

---

## G. Gaps / flags

- `[verify]` Wooldridge "Two-Way Mundlak" exact citation (referenced in the library chapter's footnotes as
  ref [4] but title garbled by OCR).
- `[verify]` worked-example magnitudes — generate on the synthetic rep-visit dataset (known ground truth);
  also demonstrate TWFE failure on that same data.
- **Identification flag (pervasive):** cohort timing is a *valid* instrument/clean DiD design **only if
  independence/parallel-trends holds** — present as *testable, not assumed* (TIKTOC Part 11: "Conditional").
  Falsify first; state the assumption and the Honest-DiD sensitivity bound every time.
- **Non-absorbing treatment flag:** message decks can be rolled back / re-version; if treatment turns off,
  prefer de Chaisemartin–D'Haultfœuille (handles on/off) over Callaway–Sant'Anna (designed for absorbing
  treatment). Flag which the synthetic data assumes.
- **Collider flag (← Ch 6):** do not condition on dwell/reaction/in-visit engagement inside the DiD.
- **Firewall flag:** Project 1 needs proprietary or synthetic data (cohort timing + `Call2_Key_Message_vod__c`
  are partner-owned); only Project 2 (meals/Open Payments) is fully public-data-runnable.
- **Consent/selection flag (→ Ch 13):** the NRx panel's sampling frame is shaped by opt-out; selection
  interacts with parallel trends if opt-out correlates with prescribing trends — note as a threat, route the
  full treatment to Ch 13 (FCI) and Ch 9 (opt-out drift monitor).
