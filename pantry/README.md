# Pantry Index

Last updated: 2026-06-16 · Chapter list source: `../TIKTOC.md` (13 chapters)

Research material for drafting — no chapter prose. Every TIKTOC chapter has an `NN-slug_notes.md`.
Claims are sourced or `[verify]`-flagged.

## Research notes (generated, one per chapter)

| File | Ch | Description |
|------|----|-------------|
| 01-the-experiment-nobody-knows_notes.md | 1 | Rep-visit telemetry as a behavioral+causal dataset; the 15-year unrun experiment; signal vs causal claim |
| 02-what-the-data-actually-is_notes.md | 2 | Veeva CLM schema (Duration/Reaction/Clickstream), rep-entered + warehouse layers; the physician-by-time panel |
| 03-stuck-on-rung-1_notes.md | 3 | Pearl's ladder + do-operator; why content-opt/rep-perf/NBA are all Rung-1; the engagement-proxy error |
| 04-the-natural-experiment-in-training-rollouts_notes.md | 4 | Training cohort as instrument; IV (relevance/independence/exclusion) + falsification; weak-IV (F>10 vs modern tF) |
| 05-the-causal-graph-and-three-pathways_notes.md | 5 | Educational/relationship/reciprocity mediation; NDE/NIE/CDE; pathway split is interventional, not naive |
| 06-the-collider-trap_notes.md | 6 | Post-treatment colliders (dwell/reaction); Berkson/selection; consent opt-out collider; FCI vs PC/GES |
| 07-markov-equivalence_notes.md | 7 | Equivalence classes; Verma–Pearl; peer-influence three stories (Shalizi–Thomas); why rep knowledge orients edges |
| 08-the-causal-interview-bot_notes.md | 8 | KEBN elicitation; Feigenbaum knowledge bottleneck; Rung-1/2/3 prompts; annotated prior DAG; LLM-elicitation risk |
| 09-from-priors-to-a-living-model_notes.md | 9 | Parameterized SCM; abduction→action→prediction; PN/PS; Living-Model architecture; opt-out drift monitoring |
| 10-staggered-did-message-variants_notes.md | 10 | Staggered DiD/event study; TWFE bias; Callaway–Sant'Anna / Sun–Abraham / Goodman-Bacon; ToT; pre-trends |
| 11-meals-causal-forest-and-clickstream-dml_notes.md | 11 | Causal forests (Wager–Athey/GRF); DML (Chernozhukov); meals = public-data project; clickstream collider discipline |
| 12-from-propensity-to-persuadables_notes.md | 12 | Persuadables vs sure-things (Ascarza); Qini; treatment-oriented allocation; NBA-list divergence as testable claim |
| 13-consent-compliance-and-handoff_notes.md | 13 | Consent/selection ethics + FCI; PhRMA Code/FDA OPDP/Sunshine Act (routed to counsel); IP firewall; handoff |

*(Two superseded duplicate stubs exist from a parallel run — `01-…-theyre-running_notes.md` and
`10-project-1-…_notes.md`; they point to the canonical files and can be deleted.)*

## Library files (`_lib_`, copied from the shared MD library)

| File | Relevant to | Contributes |
|------|------------|-------------|
| _lib_ai-applied-causal-inference-powered-by-ml-and-ai.md | Ch 4–7, 10, 11 | The methods workhorse: IV, DiD, RDD, DML, causal forests, do/backdoor/Markov |
| _lib_science-the-book-of-why-...md (+ excerpt) | Ch 3, 7, 9 | Pearl's ladder, do-operator, counterfactuals, SCM |
| _lib_math-causal-inference-mit-press-...md | Ch 4, 6, 7 | Causal-inference fundamentals |
| _lib_did-applied-causal-inference_excerpt.md | Ch 4, 10 | Curated DiD excerpt (raw OCR was degraded) |
| _lib_counterfactuals-book-of-why_excerpt.md | Ch 9 | SCM / abduction / PN-PS excerpt |
| _lib_ai-ml-assisted-adjustment.md | Ch 6, 7 | ML-assisted adjustment + expert-interview/graph-audit (mirrors this book's architecture) |
| _lib_math-weapons-of-math-destruction-...md | Ch 12, 13 | Persuadables + fairness/accountability |
| _lib_science-randomistas / calling-bullshit / naked-statistics / art-of-statistics | Ch 3–7 | RCT logic, evidence skepticism, stats literacy |

*(Most `_lib_` causal-inference titles were already present from the sibling HCP-book run and are
cross-referenced in each notes' Section F. No MD-library title covers Veeva, Open Payments, KEBN, or
staggered-DiD — those are web-sourced.)*

## Other pantry contents (pre-existing)
- `rep-visit-as-causal-dataset_source.md` — the foundational source essay (the book's spine)
- `living-models.md` — the full Living Model textbook (the framework parent book, 9,280 lines)

## Cross-cutting flags for the author / accuracy pass

- **Citation integrity — RESOLVED (2026-06-16).** The flagged arXiv IDs were verified one by one:
  `2511.00574` (LLM Bayesian-structure discovery), `2504.10397` (Shaposhnyk — LLM expert elicitation),
  `1911.02175` (Ness — Markov + SCM counterfactuals), and `1209.2355` (Bottou — counterfactual
  reasoning) are **real and correctly cited**; `2503.13323` was real but had wrong authors — **corrected**
  to Baker/Callaway/Cunningham/Goodman-Bacon/Sant'Anna (DiD practitioner's guide); `2602.01483` could
  **not** be confirmed and was **removed** (marked `[verify — citation removed]`, no fabricated
  replacement). One fabrication caught and excised — net clean.
- **"$40B promotional spend"** (source essay, Ch 5) — could not be sourced; primary figures cluster
  ~$30B US (Schwartz–Woloshin 2019) + ~$14B DTC. Re-source or footnote as approximate.
- **Veeva field names** (`Duration_vod`, `Reaction_vod`, `CLM_EXPLICIT_OPT_IN_vod`, etc.) — verify
  against current Veeva CLM docs; Veeva is migrating CRM→Vault (schema velocity = a standing risk);
  treat names as dated mid-2026 examples.
- **Identification-honesty caveats (load-bearing):** (a) the three-pathway split is generally *not*
  point-identified with correlated mediators — use interventional effects (Vansteelandt–Daniel 2017);
  (b) weak-IV: F>10 is legacy — modern work needs much larger first-stage F (Lee–McCrary–Moreira–Porter
  2022); (c) the cohort instrument is *testable, not assumed* — falsify first; (d) rep knowledge is
  necessary *in principle* (Verma–Pearl) — elicitation *quality* is the open empirical question.
- **BLOCKER (TIKTOC Risk 1 / Open Q1):** proprietary-data access + partner (Doceree) framing + consent
  ethics unresolved — gates Act III and the synthetic-dataset spec (Ch 13 §G).
- **Thin notes — RESOLVED.** Ch 2 (85→314 lines, full Veeva CLM schema + field-role table) and Ch 3
  (87→253 lines, Pearl's ladder + verified pharma-NBA practice) were enriched to match the others.
