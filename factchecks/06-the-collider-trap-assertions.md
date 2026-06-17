# Assertions Report: 06-the-collider-trap.md
**Date:** 2026-06-17
**Source file:** chapters/06-the-collider-trap.md
**Assertions flagged:** 3
**Breakdown:** STAT: 0 | GUIDELINE: 0 | APPROVAL: 0 | EVIDENCE: 2 | SPECIALIST: 0 | CURRENT: 1

---

## ⚠️ Critical — Requires Immediate Expert Review
None found.

---

## Full Findings

### EVIDENCE — CONFIRMED
**Assertion type:** POSITIVE
**Sentence:** Montgomery, Nyhan, and Torres (2018) put this plainly: conditioning on post-treatment variables can ruin your experiment.
**Claim checked:** Post-treatment conditioning can bias causal estimates even in experiments.
**Site visited:** https://doi.org/10.1111/ajps.12357
**Finding:** Montgomery, Nyhan, and Torres directly address bias from conditioning on post-treatment variables, including regression adjustment and sample restrictions.
**Expert review needed:** No
**Suggested reference:** Montgomery, Nyhan & Torres. AJPS, 2018.
**Notes:** No contradiction found.

### EVIDENCE — CONFIRMED
**Assertion type:** POSITIVE
**Sentence:** Hernán, Hernández-Díaz, and Robins (2004) unified this under one roof: selection bias is not a separate phenomenon from collider bias.
**Claim checked:** Selection bias can be represented as conditioning on a collider.
**Site visited:** https://pubmed.ncbi.nlm.nih.gov/15218803/
**Finding:** Hernan, Hernandez-Diaz, and Robins present selection bias as a form of collider/conditioning bias in causal diagrams.
**Expert review needed:** No
**Suggested reference:** Hernan, Hernandez-Diaz & Robins. Epidemiology, 2004.
**Notes:** No contradiction found.

### CURRENT — UNVERIFIED
**Assertion type:** BASIC
**Sentence:** Veeva's consent mechanism governs whether a physician's CLM activity is logged at all.
**Claim checked:** Exact Veeva consent field behavior in public docs.
**Site visited:** https://crmhelp.veeva.com/ ; https://vaultcrmhelp.veeva.com/
**Finding:** The conceptual consent-selection claim is plausible and consistent with the chapter's earlier Veeva-field discussion. The exact mid-2026 field names and opt-out behavior should be verified against current partner documentation before implementation.
**Expert review needed:** Yes
**Suggested reference:** Veeva CRM Help and Vault CRM Help.
**Notes:** Do not remove the schema-aging caveat.


---

## Unverified Assertions
| Sentence | Category | Assertion Type | Reason unverified |
| --- | --- | --- | --- |
| Veeva's consent mechanism governs whether a physician's CLM activity is logged at all. | CURRENT | BASIC | The conceptual consent-selection claim is plausible and consistent with the chapter's earlier Veeva-field discussion. The exact mid-2026 field names and opt-out behavior should be verified against current partner documentation before implementation. |

---

## AI-Pass Flags
No internal contradictions found in this pass beyond the verification caveats listed above.
