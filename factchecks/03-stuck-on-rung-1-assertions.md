# Assertions Report: 03-stuck-on-rung-1.md
**Date:** 2026-06-17
**Source file:** chapters/03-stuck-on-rung-1.md
**Assertions flagged:** 3
**Breakdown:** STAT: 0 | GUIDELINE: 0 | APPROVAL: 0 | EVIDENCE: 2 | SPECIALIST: 0 | CURRENT: 1

---

## ⚠️ Critical — Requires Immediate Expert Review
None found.

---

## Full Findings

### EVIDENCE — CONFIRMED
**Assertion type:** POSITIVE
**Sentence:** Judea Pearl introduced the ladder as a classification of kinds of questions — a taxonomy not of models or data but of the queries a reasoning system is capable of answering.
**Claim checked:** Pearl's ladder has association, intervention, and counterfactual levels.
**Site visited:** https://www.basicbooks.com/titles/judea-pearl/the-book-of-why/9780465097616/
**Finding:** Pearl and Mackenzie's Book of Why presents the Ladder of Causation as association, intervention, and counterfactual reasoning. The chapter's description matches that framework.
**Expert review needed:** No
**Suggested reference:** Pearl, Judea and Dana Mackenzie. The Book of Why. Basic Books, 2018.
**Notes:** Direct quotation in source chapter should be checked against the book text before publication.

### EVIDENCE — CONFIRMED
**Assertion type:** POSITIVE
**Sentence:** Bottou and colleagues showed this formally in 2013, studying Bing ad placement.
**Claim checked:** Counterfactual reasoning is needed for learning-system interventions in advertising.
**Site visited:** https://arxiv.org/abs/1209.2355
**Finding:** Bottou et al. discuss counterfactual reasoning and learning systems in computational advertising, including why observational optimization is insufficient for system changes.
**Expert review needed:** No
**Suggested reference:** Bottou et al. Counterfactual Reasoning and Learning Systems, JMLR/arXiv, 2013.
**Notes:** No contradiction found.

### CURRENT — UNVERIFIED
**Assertion type:** BASIC
**Sentence:** No major vendor publicly claims to estimate $P(\text{NRx} \mid do(\text{message}))$ with a stated identification strategy.
**Claim checked:** Vendor public materials do not claim do-operator identification.
**Site visited:** Vendor public pages for Aktana, IQVIA, Indegene
**Finding:** This is a sourced inference from absence in public materials, not a proof. The chapter says that explicitly, which is the right caveat.
**Expert review needed:** Yes
**Suggested reference:** Vendor materials should be archived with access dates.
**Notes:** Keep as an inference, not a universal fact about proprietary internals.


---

## Unverified Assertions
| Sentence | Category | Assertion Type | Reason unverified |
| --- | --- | --- | --- |
| No major vendor publicly claims to estimate $P(\text{NRx} \mid do(\text{message}))$ with a stated identification strategy. | CURRENT | BASIC | This is a sourced inference from absence in public materials, not a proof. The chapter says that explicitly, which is the right caveat. |

---

## AI-Pass Flags
No internal contradictions found in this pass beyond the verification caveats listed above.
