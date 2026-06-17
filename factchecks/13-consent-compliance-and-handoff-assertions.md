# Assertions Report: 13-consent-compliance-and-handoff.md
**Date:** 2026-06-17
**Source file:** chapters/13-consent-compliance-and-handoff.md
**Assertions flagged:** 3
**Breakdown:** STAT: 0 | GUIDELINE: 1 | APPROVAL: 0 | EVIDENCE: 0 | SPECIALIST: 0 | CURRENT: 2

---

## ⚠️ Critical — Requires Immediate Expert Review
None found.

---

## Full Findings

### CURRENT — UNVERIFIED
**Assertion type:** BASIC
**Sentence:** In Veeva's CRM, `CLM_EXPLICIT_OPT_IN_vod` governs logging. Opt-out either suppresses the clickstream or anonymizes it; `CLM_OPT_OUT_BEHAVIOR_vod` strips the NPI from the activity record.
**Claim checked:** Exact current Veeva consent-field behavior.
**Site visited:** https://crmhelp.veeva.com/ ; https://vaultcrmhelp.veeva.com/
**Finding:** This is plausible and consistent with prior chapter caveats, but exact field behavior must be verified against current Veeva/Vault and partner schema before implementation.
**Expert review needed:** Yes
**Suggested reference:** Veeva CRM Help; Vault CRM Help.
**Notes:** The chapter already states field names are dated and should be verified.

### GUIDELINE — CONFIRMED
**Assertion type:** POSITIVE
**Sentence:** Promotion that is truthful, non-misleading, fairly balanced, and consistent with FDA-approved labeling sits inside what the FDA Office of Prescription Drug Promotion (OPDP) regulates and, on current interpretation, permits.
**Claim checked:** OPDP regulates prescription-drug promotion for truthful, balanced, non-misleading communication.
**Site visited:** https://www.fda.gov/about-fda/cder-offices-and-divisions/office-prescription-drug-promotion-opdp
**Finding:** FDA's OPDP mission page says OPDP helps ensure prescription-drug promotion is truthful, balanced, and accurately communicated, and reviews materials for false or misleading content.
**Expert review needed:** No
**Suggested reference:** FDA. Office of Prescription Drug Promotion page, content current 2025-09-09.
**Notes:** Legal interpretation should remain routed to counsel.

### CURRENT — CONFIRMED
**Assertion type:** POSITIVE
**Sentence:** Physician Payments Sunshine Act → CMS Open Payments. Mandatory disclosure of manufacturer payments and transfers of value.
**Claim checked:** Open Payments is the federal public disclosure program for payments from drug/device companies to covered recipients.
**Site visited:** https://www.cms.gov/priorities/key-initiatives/open-payments
**Finding:** CMS describes Open Payments as a national disclosure program with a publicly accessible database of payments made by reporting entities, including drug and device companies, to covered recipients such as physicians.
**Expert review needed:** No
**Suggested reference:** CMS. What is Open Payments?, page modified 2026-06-05.
**Notes:** The 2025 dollar thresholds should still be verified against current CMS guidance before use.


---

## Unverified Assertions
| Sentence | Category | Assertion Type | Reason unverified |
| --- | --- | --- | --- |
| In Veeva's CRM, `CLM_EXPLICIT_OPT_IN_vod` governs logging. Opt-out either suppresses the clickstream or anonymizes it; `CLM_OPT_OUT_BEHAVIOR_vod` strips the NPI from the activity record. | CURRENT | BASIC | This is plausible and consistent with prior chapter caveats, but exact field behavior must be verified against current Veeva/Vault and partner schema before implementation. |

---

## AI-Pass Flags
No internal contradictions found in this pass beyond the verification caveats listed above.
