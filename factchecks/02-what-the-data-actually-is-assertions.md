# Assertions Report: 02-what-the-data-actually-is.md
**Date:** 2026-06-17
**Source file:** chapters/02-what-the-data-actually-is.md
**Assertions flagged:** 3
**Breakdown:** STAT: 0 | GUIDELINE: 0 | APPROVAL: 0 | EVIDENCE: 0 | SPECIALIST: 0 | CURRENT: 3

---

## ⚠️ Critical — Requires Immediate Expert Review
- CURRENT / UNVERIFIED: A standing caveat, stated once and meant throughout the book. Every `*_vod` field name below is a dated mid-2026 example. Veeva is migrating from its Salesforce-based CRM to a proprietary Vault CRM — end-of-support for legacy Veeva CRM is December 2029; Vault CRM is generally available and had 125+ live customers in early 2026.

---

## Full Findings

### CURRENT — UNVERIFIED
**Assertion type:** COMBINATION
**Sentence:** A standing caveat, stated once and meant throughout the book. Every `*_vod` field name below is a dated mid-2026 example. Veeva is migrating from its Salesforce-based CRM to a proprietary Vault CRM — end-of-support for legacy Veeva CRM is December 2029; Vault CRM is generally available and had 125+ live customers in early 2026.
**Claim checked:** Veeva migration timing, end-of-support, and live-customer count.
**Site visited:** https://www.veeva.com/products/crm-suite/ ; https://crmhelp.veeva.com/ ; https://vaultcrmhelp.veeva.com/
**Finding:** Veeva public pages confirm Vault CRM/CRM Suite exists and that current product documentation should be checked. The exact December 2029 end-of-support and 125+ live-customer figures were not confirmed from accessible public pages in this pass.
**Expert review needed:** Yes
**Suggested reference:** Veeva CRM Suite; Veeva CRM Help; Vault CRM Help.
**Notes:** Keep dated caveat; add a Veeva release/investor citation if those numbers remain.

### CURRENT — UNVERIFIED
**Assertion type:** BASIC
**Sentence:** `Display_Order_vod` and `View_Order_vod` capture the sequence of key messages: loops, skips, backtracks through the content.
**Claim checked:** Exact public Veeva field split for display order versus actual view order.
**Site visited:** https://crmhelp.veeva.com/
**Finding:** The chapter already flags `View_Order_vod` as unresolved. Public documentation could not conclusively confirm the distinct field split during this pass.
**Expert review needed:** Yes
**Suggested reference:** Could not identify a specific public source.
**Notes:** The embedded `[verify]` flag is appropriate.

### CURRENT — CONFIRMED
**Assertion type:** POSITIVE
**Sentence:** **NRx** — new prescriptions, excluding refills. New scripts and renewals of expired scripts, but not routine refills.
**Claim checked:** NRx/TRx/NBRx are commercial prescribing measures; NRx excludes routine refills.
**Site visited:** IQVIA/Symphony public terminology and industry data dictionaries
**Finding:** The terminology is standard in life-sciences commercial analytics, but public primary documentation is thin because these are proprietary data products. Treat as industry-standard terminology and cite the data provider's glossary where available.
**Expert review needed:** No
**Suggested reference:** IQVIA Xponent and Symphony Health product documentation.
**Notes:** No contradiction found.


---

## Unverified Assertions
| Sentence | Category | Assertion Type | Reason unverified |
| --- | --- | --- | --- |
| A standing caveat, stated once and meant throughout the book. Every `*_vod` field name below is a dated mid-2026 example. Veeva is migrating from its Salesforce-based CRM to a proprietary Vault CRM — end-of-support for legacy Veeva CRM is December 2029; Vault CRM is generally available and had 125+ live customers in early 2026. | CURRENT | COMBINATION | Veeva public pages confirm Vault CRM/CRM Suite exists and that current product documentation should be checked. The exact December 2029 end-of-support and 125+ live-customer figures were not confirmed from accessible public pages in this pass. |
| `Display_Order_vod` and `View_Order_vod` capture the sequence of key messages: loops, skips, backtracks through the content. | CURRENT | BASIC | The chapter already flags `View_Order_vod` as unresolved. Public documentation could not conclusively confirm the distinct field split during this pass. |

---

## AI-Pass Flags
No internal contradictions found in this pass beyond the verification caveats listed above.
