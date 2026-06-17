# Assertions Report: 09-from-priors-to-a-living-model.md
**Date:** 2026-06-17
**Source file:** chapters/09-from-priors-to-a-living-model.md
**Assertions flagged:** 2
**Breakdown:** STAT: 0 | GUIDELINE: 0 | APPROVAL: 0 | EVIDENCE: 1 | SPECIALIST: 1 | CURRENT: 0

---

## ⚠️ Critical — Requires Immediate Expert Review
None found.

---

## Full Findings

### EVIDENCE — CONFIRMED
**Assertion type:** POSITIVE
**Sentence:** Pearl's three-step counterfactual procedure is the heart of the chapter and the thing no next-best-action engine can do.
**Claim checked:** Abduction-action-prediction is Pearl's counterfactual procedure.
**Site visited:** https://www.basicbooks.com/titles/judea-pearl/the-book-of-why/9780465097616/
**Finding:** Pearl's counterfactual procedure is commonly described as abduction, action, and prediction. The second half of the sentence is a domain inference about current NBA engines and should remain framed as a public-material inference.
**Expert review needed:** No
**Suggested reference:** Pearl. Causality, 2nd ed., 2009; Pearl & Mackenzie, 2018.
**Notes:** No contradiction found for the procedure.

### SPECIALIST — UNVERIFIED
**Assertion type:** POSITIVE
**Sentence:** A propensity engine or a next-best-action ranker has no $U_i$ to recover — it has no structural model, so it cannot read a specific physician's state.
**Claim checked:** Current NBA engines do not implement SCM abduction.
**Site visited:** Vendor public materials
**Finding:** This is a claim about commercial implementations and cannot be fully verified from public materials. It is plausible as a critique of public propensity/engagement tools, but proprietary systems could differ.
**Expert review needed:** Yes
**Suggested reference:** Could not identify a specific public source.
**Notes:** Frame as 'publicly described NBA engines' unless partner/vendor internals are audited.


---

## Unverified Assertions
| Sentence | Category | Assertion Type | Reason unverified |
| --- | --- | --- | --- |
| A propensity engine or a next-best-action ranker has no $U_i$ to recover — it has no structural model, so it cannot read a specific physician's state. | SPECIALIST | POSITIVE | This is a claim about commercial implementations and cannot be fully verified from public materials. It is plausible as a critique of public propensity/engagement tools, but proprietary systems could differ. |

---

## AI-Pass Flags
No internal contradictions found in this pass beyond the verification caveats listed above.
