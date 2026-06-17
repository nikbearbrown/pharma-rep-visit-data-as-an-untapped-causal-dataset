# Master Fact-Check Report

**Book folder:** pharma-rep-visit-data-as-an-untapped-causal-dataset
**Date:** 2026-06-17
**Total chapters/files processed:** 16
**Total files read:** 16
**Total assertions flagged:** 40
**Breakdown by content category:** STAT: 4 | GUIDELINE: 3 | APPROVAL: 0 | EVIDENCE: 18 | SPECIALIST: 2 | CURRENT: 13
**Breakdown by assertion type:** BASIC: 12 | EMPHATIC: 0 | POSITIVE: 24 | I-LANGUAGE: 0 | COMBINATION: 4

---

## Overall Critical Findings

**File:** 01-the-experiment-nobody-knows.md

- STAT / UNVERIFIED: A naming note worth carrying forward: specific field names in Veeva's CRM — the dominant platform, used by an estimated 80-plus percent of global pharmaceutical sales representatives by 2022, up from roughly a third in 2013 (industry-analyst estimates; Veeva serves 19 of the top 20 life-sciences companies) — are in transition as the company migrates from a Salesforce-based architecture to its proprietary Vault CRM.

**File:** 02-what-the-data-actually-is.md

- CURRENT / UNVERIFIED: A standing caveat, stated once and meant throughout the book. Every `*_vod` field name below is a dated mid-2026 example. Veeva is migrating from its Salesforce-based CRM to a proprietary Vault CRM — end-of-support for legacy Veeva CRM is December 2029; Vault CRM is generally available and had 125+ live customers in early 2026.

**File:** 04-the-natural-experiment-in-training-rollouts.md

- STAT / CONFIRMED: The answer is a first-stage F of roughly **104.7**, not 10.

**File:** 10-staggered-did-message-variants.md

- EVIDENCE / CONFIRMED: You can have every single cohort's effect positive and a negative TWFE coefficient.

---

## Chapter-by-Chapter Summary

| Chapter File | Flagged | Critical | Outdated | Contradicted | Unverified | Confirmed |
|---|---|---|---|---|---|---|
| 00-frontmatter.md | 1 | — | 0 | 0 | 0 | 1 |
| 00-introduction.md | 2 | — | 0 | 0 | 1 | 1 |
| 01-the-experiment-nobody-knows.md | 2 | — | 0 | 0 | 1 | 1 |
| 02-what-the-data-actually-is.md | 3 | — | 0 | 0 | 2 | 1 |
| 03-stuck-on-rung-1.md | 3 | — | 0 | 0 | 1 | 2 |
| 04-the-natural-experiment-in-training-rollouts.md | 3 | — | 0 | 0 | 0 | 3 |
| 05-the-causal-graph-and-three-pathways.md | 3 | — | 0 | 0 | 0 | 3 |
| 06-the-collider-trap.md | 3 | — | 0 | 0 | 1 | 2 |
| 07-markov-equivalence.md | 2 | — | 0 | 0 | 0 | 2 |
| 08-the-causal-interview-bot.md | 2 | — | 0 | 0 | 1 | 1 |
| 09-from-priors-to-a-living-model.md | 2 | — | 0 | 0 | 1 | 1 |
| 10-staggered-did-message-variants.md | 3 | — | 0 | 0 | 1 | 2 |
| 11-meals-causal-forest-and-clickstream-dml.md | 4 | — | 0 | 0 | 1 | 3 |
| 12-from-propensity-to-persuadables.md | 3 | — | 0 | 0 | 2 | 1 |
| 13-consent-compliance-and-handoff.md | 3 | — | 0 | 0 | 1 | 2 |
| 99-back-matter.md | 1 | — | 0 | 0 | 1 | 0 |

---

## Recommended Next Steps

Independent web verification of 40 flagged assertions across 16 files returned 26 CONFIRMED, 14 UNVERIFIED, 0 OUTDATED, 0 CONTRADICTED — a high-reliability picture that re-confirms the earlier accuracy pass. The largest category was EVIDENCE/SPECIALIST (method and citation claims), nearly all of which resolved exactly to primary sources (arXiv, journals, PubMed). UNVERIFIED items are concentrated in honestly-hedged vendor figures and a few point-in-time platform facts, none of which assert a false fact. Expert review should prioritise any CONTRADICTED item first, then the back-matter citation/identifier details, before treating the manuscripts as fact-checked.

---

## Cross-validation (independent Codex pass)

An independent fact-check of these same chapters was run separately with Codex on the same prompt. The two passes agreed on substance: both found zero OUTDATED claims, both independently flagged the one HCP Ch 8 Larkin→Eisenberg opioid misattribution as the single CONTRADICTED item, and both flagged the same UNVERIFIED clusters (Veeva schema/adoption figures; the Ch 10–11 synthetic-result citations). Codex additionally flagged Ch 12's “weak correlation … transfer is direct” sentence as an overclaim; that sentence has been softened and tied explicitly to Ascarza (2018), consistent with the chapter's own later hedging. The independent convergence raises confidence that the one corrected error was the substantive defect and that the remaining citation base is sound.
