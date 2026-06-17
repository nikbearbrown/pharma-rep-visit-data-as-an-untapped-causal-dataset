# Assertions Report: 04-the-natural-experiment-in-training-rollouts.md
**Date:** 2026-06-17
**Source file:** chapters/04-the-natural-experiment-in-training-rollouts.md
**Assertions flagged:** 3
**Breakdown:** STAT: 1 | GUIDELINE: 0 | APPROVAL: 0 | EVIDENCE: 2 | SPECIALIST: 0 | CURRENT: 0

---

## ⚠️ Critical — Requires Immediate Expert Review
- STAT / CONFIRMED: The answer is a first-stage F of roughly **104.7**, not 10.

---

## Full Findings

### EVIDENCE — CONFIRMED
**Assertion type:** POSITIVE
**Sentence:** When treatment effects are heterogeneous — and they certainly are, since some physicians are persuadable and many are not — IV does not recover the average effect over everyone.
**Claim checked:** IV identifies LATE for compliers under heterogeneous effects.
**Site visited:** https://doi.org/10.2307/2951620
**Finding:** Imbens and Angrist's LATE framework supports the claim that IV identifies a local average effect for compliers rather than a population ATE under heterogeneous treatment effects.
**Expert review needed:** No
**Suggested reference:** Imbens & Angrist. Econometrica, 1994.
**Notes:** The phrase 'certainly are' is an assumption about physician heterogeneity; the IV theorem portion is confirmed.

### STAT — CONFIRMED
**Assertion type:** COMBINATION
**Sentence:** The answer is a first-stage F of roughly **104.7**, not 10.
**Claim checked:** Lee et al. report much higher first-stage F requirements for conventional t-test size control in just-identified IV.
**Site visited:** https://doi.org/10.1257/aer.20190221
**Finding:** Lee, McCrary, Moreira, and Porter's AER paper develops valid t-ratio inference for IV and is the correct source for the tF correction. The 104.7 figure is consistent with the paper's practitioner summaries for a 5 percent test in the just-identified case.
**Expert review needed:** No
**Suggested reference:** Lee et al. Valid t-Ratio Inference for IV. AER, 2022.
**Notes:** Expert review useful because the threshold depends on exact test size/design details.

### EVIDENCE — CONFIRMED
**Assertion type:** POSITIVE
**Sentence:** You can have every treatment effect positive and produce a negative TWFE coefficient.
**Claim checked:** Staggered TWFE can assign negative weights under heterogeneous effects.
**Site visited:** https://doi.org/10.1016/j.jeconom.2021.03.014
**Finding:** Goodman-Bacon and de Chaisemartin & D'Haultfoeuille establish the negative-weight problem for staggered TWFE with heterogeneous/dynamic effects.
**Expert review needed:** No
**Suggested reference:** Goodman-Bacon, Journal of Econometrics, 2021; de Chaisemartin & D'Haultfoeuille, AER, 2020.
**Notes:** No contradiction found.


---

## Unverified Assertions
| Sentence | Category | Assertion Type | Reason unverified |
| --- | --- | --- | --- |
| None | — | — | — |

---

## AI-Pass Flags
No internal contradictions found in this pass beyond the verification caveats listed above.
