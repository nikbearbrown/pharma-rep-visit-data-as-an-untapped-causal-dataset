# Assertions Report: 11-meals-causal-forest-and-clickstream-dml.md
**Date:** 2026-06-17
**Source file:** chapters/11-meals-causal-forest-and-clickstream-dml.md
**Assertions flagged:** 4
**Breakdown:** STAT: 0 | GUIDELINE: 0 | APPROVAL: 0 | EVIDENCE: 2 | SPECIALIST: 1 | CURRENT: 1

---

## ⚠️ Critical — Requires Immediate Expert Review
None found.

---

## Full Findings

### EVIDENCE — CONFIRMED
**Assertion type:** POSITIVE
**Sentence:** Two innovations make a causal forest more than a relabeled regression forest. The first is honest splitting (Wager & Athey 2018, *JASA* 113(523):1228–1242).
**Claim checked:** Causal forests use honest splitting and support inference for heterogeneous effects.
**Site visited:** https://doi.org/10.1080/01621459.2017.1319839
**Finding:** Wager and Athey establish causal forests with honesty and asymptotic inference for heterogeneous treatment effects.
**Expert review needed:** No
**Suggested reference:** Wager & Athey. JASA, 2018.
**Notes:** No contradiction found.

### EVIDENCE — CONFIRMED
**Assertion type:** POSITIVE
**Sentence:** Double Machine Learning (DML — Chernozhukov et al. 2018, *Econometrics Journal* 21(1):C1–C68) estimates a low-dimensional causal parameter θ when the confounders are high-dimensional and you want to adjust for them with flexible ML rather than a parametric model.
**Claim checked:** DML uses orthogonalization and cross-fitting for low-dimensional causal parameters with high-dimensional nuisance functions.
**Site visited:** https://arxiv.org/abs/1608.00060
**Finding:** Chernozhukov et al. describe double/debiased ML using orthogonal scores and cross-fitting, allowing flexible ML for nuisance estimation.
**Expert review needed:** No
**Suggested reference:** Chernozhukov et al. Econometrics Journal, 2018.
**Notes:** No contradiction found.

### CURRENT — CONFIRMED
**Assertion type:** BASIC
**Sentence:** Build the dataset from CMS Open Payments (filter to meals by NPI and drug) and Medicare Part D prescribing by NPI. This is the DeJong (2016) linkage, fully public.
**Claim checked:** Open Payments and Medicare Part D Prescribers are public sources that can be linked for public analysis.
**Site visited:** https://www.cms.gov/priorities/key-initiatives/open-payments ; https://data.cms.gov/provider-summary-by-type-of-service/medicare-part-d-prescribers
**Finding:** CMS confirms Open Payments is a public database of payments made by reporting entities to covered recipients. CMS hosts Part D Prescribers public data. Linkage by NPI is analytically common but requires careful data cleaning.
**Expert review needed:** No
**Suggested reference:** CMS Open Payments; CMS Medicare Part D Prescribers; DeJong et al., 2016.
**Notes:** No contradiction found.

### SPECIALIST — UNVERIFIED
**Assertion type:** BASIC
**Sentence:** One methodological flag to carry: treating the slide sequence as a high-dimensional treatment in DML is this book's extension of the standard formulation, which handles high-dimensional confounders rather than a high-dimensional treatment.
**Claim checked:** DML extension to slide sequence as high-dimensional treatment is validated.
**Site visited:** Local synthetic validation only
**Finding:** The chapter correctly marks this as the author's extension and requires synthetic recovery before claiming applicability. No public validation source was identified.
**Expert review needed:** Yes
**Suggested reference:** Could not identify a specific source.
**Notes:** Keep `[verify]`; do not present as settled method.


---

## Unverified Assertions
| Sentence | Category | Assertion Type | Reason unverified |
| --- | --- | --- | --- |
| One methodological flag to carry: treating the slide sequence as a high-dimensional treatment in DML is this book's extension of the standard formulation, which handles high-dimensional confounders rather than a high-dimensional treatment. | SPECIALIST | BASIC | The chapter correctly marks this as the author's extension and requires synthetic recovery before claiming applicability. No public validation source was identified. |

---

## AI-Pass Flags
No internal contradictions found in this pass beyond the verification caveats listed above.
