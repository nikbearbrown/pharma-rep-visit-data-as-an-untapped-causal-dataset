# Assertions Report: 00-frontmatter.md
**Date:** 2026-06-17
**Source file:** chapters/00-frontmatter.md
**Assertions flagged:** 1
**Breakdown:** STAT: 0 | GUIDELINE: 0 | APPROVAL: 0 | EVIDENCE: 0 | SPECIALIST: 0 | CURRENT: 1

---

## ⚠️ Critical — Requires Immediate Expert Review
None found.

---

## Full Findings

### CURRENT — CONFIRMED
**Assertion type:** BASIC
**Sentence:** It does not use proprietary data, and it never will: everything runs on public Open Payments and Medicare Part D data, or on a synthetic dataset whose ground truth you can check your methods against.
**Claim checked:** Open Payments and Medicare Part D Prescribers are public data sources.
**Site visited:** https://www.cms.gov/priorities/key-initiatives/open-payments ; https://data.cms.gov/provider-summary-by-type-of-service/medicare-part-d-prescribers
**Finding:** CMS describes Open Payments as a publicly accessible database. CMS Data hosts Medicare Part D Prescribers datasets, though the dynamic CMS Data page itself requires JavaScript. The claim about this book's own synthetic dataset is internal rather than web-verifiable.
**Expert review needed:** No
**Suggested reference:** CMS. Open Payments overview, 2026. CMS Medicare Part D Prescribers dataset. URLs above.
**Notes:** Public-data portion confirmed; synthetic-dataset availability should be checked locally when packaging.


---

## Unverified Assertions
| Sentence | Category | Assertion Type | Reason unverified |
| --- | --- | --- | --- |
| None | — | — | — |

---

## AI-Pass Flags
No internal contradictions found in this pass beyond the verification caveats listed above.
