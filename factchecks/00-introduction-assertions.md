# Assertions Report: 00-introduction.md
**Date:** 2026-06-17
**Source file:** chapters/00-introduction.md
**Assertions flagged:** 2
**Breakdown:** STAT: 0 | GUIDELINE: 0 | APPROVAL: 0 | EVIDENCE: 1 | SPECIALIST: 0 | CURRENT: 1

---

## ⚠️ Critical — Requires Immediate Expert Review
None found.

---

## Full Findings

### CURRENT — CONFIRMED
**Assertion type:** POSITIVE
**Sentence:** **The contract that makes this safe is an IP firewall: Fellows work only on public data (CMS Open Payments, Medicare Part D, ICER value reports) and synthetic stand-ins.**
**Claim checked:** Open Payments and Medicare Part D are public sources suitable for a public-side research workflow.
**Site visited:** https://www.cms.gov/priorities/key-initiatives/open-payments ; https://data.cms.gov/provider-summary-by-type-of-service/medicare-part-d-prescribers
**Finding:** CMS confirms Open Payments is public and publishes annual downloadable/payment records. Medicare Part D Prescribers is hosted as a CMS public dataset. ICER reports are also publicly posted by ICER, but project-specific licensing should be checked before redistribution.
**Expert review needed:** No
**Suggested reference:** CMS Open Payments overview; CMS Medicare Part D Prescribers; ICER public reports.
**Notes:** Firewall policy itself is internal governance, not externally confirmable.

### EVIDENCE — UNVERIFIED
**Assertion type:** BASIC
**Sentence:** **In the worked run the plain model wins — a "no" that saves an engineering build.**
**Claim checked:** A specific benchmark result exists in the companion material.
**Site visited:** Local repository only
**Finding:** This is a result claim about a worked run in Book A, not verifiable from authoritative public sites during this pass. It should point to the exact notebook or output artifact if retained.
**Expert review needed:** Yes
**Suggested reference:** Could not identify a specific public source.
**Notes:** Add a local artifact citation or soften to a proposed benchmark.


---

## Unverified Assertions
| Sentence | Category | Assertion Type | Reason unverified |
| --- | --- | --- | --- |
| **In the worked run the plain model wins — a "no" that saves an engineering build.** | EVIDENCE | BASIC | This is a result claim about a worked run in Book A, not verifiable from authoritative public sites during this pass. It should point to the exact notebook or output artifact if retained. |

---

## AI-Pass Flags
No internal contradictions found in this pass beyond the verification caveats listed above.
