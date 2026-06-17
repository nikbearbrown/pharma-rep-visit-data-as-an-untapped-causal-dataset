# Assertions Report: 10-staggered-did-message-variants.md
**Date:** 2026-06-17
**Source file:** chapters/10-staggered-did-message-variants.md
**Assertions flagged:** 3
**Breakdown:** STAT: 0 | GUIDELINE: 1 | APPROVAL: 0 | EVIDENCE: 2 | SPECIALIST: 0 | CURRENT: 0

---

## ⚠️ Critical — Requires Immediate Expert Review
- EVIDENCE / CONFIRMED: You can have every single cohort's effect positive and a negative TWFE coefficient.

---

## Full Findings

### EVIDENCE — CONFIRMED
**Assertion type:** COMBINATION
**Sentence:** You can have every single cohort's effect positive and a negative TWFE coefficient.
**Claim checked:** Negative TWFE coefficients can arise from positive heterogeneous effects due to negative weights.
**Site visited:** https://doi.org/10.1016/j.jeconom.2021.03.014
**Finding:** This is a known implication of the staggered-adoption TWFE critique. The claim is strongly stated but consistent with Goodman-Bacon and de Chaisemartin & D'Haultfoeuille.
**Expert review needed:** No
**Suggested reference:** Goodman-Bacon, 2021; de Chaisemartin & D'Haultfoeuille, 2020.
**Notes:** Combination risk because it is emphatic and positive, but confirmed.

### GUIDELINE — CONFIRMED
**Assertion type:** POSITIVE
**Sentence:** The fix is to build comparisons by hand from clean 2×2 building blocks that never use already-treated units as controls.
**Claim checked:** Modern DiD practice should avoid already-treated controls under heterogeneous/dynamic effects.
**Site visited:** https://arxiv.org/abs/1803.09015
**Finding:** Callaway-Sant'Anna and Sun-Abraham provide heterogeneity-robust alternatives; Rambachan-Roth supports sensitivity analysis around parallel trends. The recommendation matches current econometric guidance.
**Expert review needed:** No
**Suggested reference:** Callaway & Sant'Anna, 2021; Sun & Abraham, 2021; Rambachan & Roth, 2023.
**Notes:** No contradiction found.

### EVIDENCE — UNVERIFIED
**Assertion type:** BASIC
**Sentence:** The event study now shows flat pre-treatment leads and positive, growing lags — recovering the planted ground truth that TWFE destroyed. [verify magnitudes against synthetic data before publication]
**Claim checked:** Synthetic run recovers planted ground truth.
**Site visited:** Local synthetic notebook/output
**Finding:** This is a local empirical result claim; it must be checked against the actual synthetic pipeline output. The source sentence already flags the magnitudes for verification.
**Expert review needed:** Yes
**Suggested reference:** Could not identify a specific source.
**Notes:** Attach notebook path/output before publication.


---

## Unverified Assertions
| Sentence | Category | Assertion Type | Reason unverified |
| --- | --- | --- | --- |
| The event study now shows flat pre-treatment leads and positive, growing lags — recovering the planted ground truth that TWFE destroyed. [verify magnitudes against synthetic data before publication] | EVIDENCE | BASIC | This is a local empirical result claim; it must be checked against the actual synthetic pipeline output. The source sentence already flags the magnitudes for verification. |

---

## AI-Pass Flags
No internal contradictions found in this pass beyond the verification caveats listed above.
