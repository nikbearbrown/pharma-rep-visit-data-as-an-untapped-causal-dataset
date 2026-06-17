# Chapter Research Gatherer Report

**Book directory:** `/Users/bear/Documents/CoWork/bear-textbooks/books/pharma-rep-visit-data-as-an-untapped-causal-dataset`  
**Source TOC:** `TIKTOC.md`  
**Generated:** 2026-06-17

---

## Chapter List Extracted from TIKTOC.md

```text
01 | the-experiment-nobody-knows-theyre-running | The Experiment Nobody Knows They're Running
02 | what-the-data-actually-is                   | What the Data Actually Is
03 | stuck-on-rung-1                            | Stuck on Rung 1
04 | the-natural-experiment-in-every-training-rollout | The Natural Experiment in Every Training Rollout
05 | the-causal-graph-and-the-three-pathways     | The Causal Graph and the Three Pathways
06 | the-collider-trap-and-the-consent-collider  | The Collider Trap (and the Consent Collider)
07 | markov-equivalence-why-the-data-cant-decide | Markov Equivalence: Why the Data Can't Decide
08 | the-causal-interview-bot                    | The Causal Interview Bot
09 | from-priors-to-a-living-model               | From Priors to a Living Model
10 | project-1-staggered-did-on-message-variant-transitions | Project 1: Staggered DiD on Message-Variant Transitions
11 | projects-2-3-meals-causal-forest-and-slide-sequencing-dml | Projects 2 & 3: Meals (Causal Forest) and Slide Sequencing (DML)
12 | from-propensity-to-persuadables             | From Propensity to Persuadables
13 | consent-compliance-and-the-handoff          | Consent, Compliance, and the Handoff
```

Total: 13 chapters.

---

## Library Scan Summary

Shared library path: `/Users/bear/Documents/CoWork/bear-textbooks/MD`

Files copied to `pantry/`:

- `_lib_ai-applied-causal-inference-powered-by-ml-and-ai.md` — relevant to Chapters 8-12
- `_lib_business-flawless-consulting-4th-edition.md` — relevant to Chapter 13 handoff
- `_lib_economics-the-lean-startup-how-today-s-entrepreneurs-use-continuous-innovation-to-create.md` — relevant to Chapter 13 product decision framing
- `_lib_math-causal-inference.md` — relevant to Chapters 3-7, 10-12
- `_lib_math-causal-inference-mit-press-essential-knowledge-series.md` — relevant to Chapters 3-7, 10-12
- `_lib_math-naked-statistics-stripping-the-dread-from-the-data.md` — relevant to evidence skepticism throughout
- `_lib_math-the-art-of-statistics-how-to-learn-from-data.md` — relevant to statistical framing
- `_lib_math-weapons-of-math-destruction-how-big-data-increases-inequality-and-threatens-dem.md` — relevant to Chapters 12-13 accountability
- `_lib_science-calling-bullshit-the-art-of-skepticism-in-a-data-driven-world.md` — relevant to evidence critique
- `_lib_science-the-book-of-why-the-new-science-of-cause-and-effect.md` — relevant to Chapters 3, 7, 9

Files skipped: broad unrelated matches in literature, music, cancer biology, finance, and general history. The library has useful causal/statistical scaffolding but no obvious pharma rep-visit-specific source file beyond this repo's own pantry.

---

## Web Research Source Anchors Used

Core causal / model sources:

- Pearl / causal model overview: https://en.wikipedia.org/wiki/Causal_model
- Bottou et al., "Counterfactual Reasoning and Learning Systems": https://arxiv.org/abs/1209.2355
- Verma & Pearl, "On the Equivalence of Causal Models": https://arxiv.org/abs/1304.1108
- Spirtes / Meek / Richardson, latent variables and selection bias: https://arxiv.org/abs/1302.4983
- Wager & Athey, causal forests: https://arxiv.org/abs/1510.04342
- Chernozhukov et al., automatic debiased ML: https://arxiv.org/abs/1809.05224
- Bach et al., DML tuning: https://arxiv.org/abs/2402.04674
- Callaway & Sant'Anna, staggered DiD: https://arxiv.org/abs/1803.09015
- Sun & Abraham, dynamic treatment effects: https://arxiv.org/abs/1804.05785
- Baker et al., DiD practitioner's guide: https://arxiv.org/abs/2503.13323
- de Chaisemartin & D'Haultfoeuille, TWFE issues: https://arxiv.org/abs/2012.10077

Healthcare / pharma public-data sources:

- CMS Open Payments: https://openpaymentsdata.cms.gov/
- CMS Medicare Part D Prescribers by Provider and Drug: https://data.cms.gov/provider-summary-by-type-of-service/medicare-part-d-prescribers/medicare-part-d-prescribers-by-provider-and-drug
- Newham & Valente, "The Cost of Influence": https://arxiv.org/abs/2203.01778

Elicitation / knowledge engineering:

- BARD Bayesian network elicitation: https://arxiv.org/abs/2003.01207
- LLM-assisted causal elicitation: https://arxiv.org/abs/2504.10397
- Causal preference elicitation: https://arxiv.org/abs/2602.01483
- Knowledge engineering overview: https://en.wikipedia.org/wiki/Knowledge_engineering

Bias / selection:

- Collider overview: https://en.wikipedia.org/wiki/Collider_(statistics)
- Berkson's paradox: https://en.wikipedia.org/wiki/Berkson%27s_paradox
- Selection bias: https://en.wikipedia.org/wiki/Selection_bias
- Missing data adjustment criteria: https://arxiv.org/abs/1907.01654
- Collider bias magnitude/direction: https://arxiv.org/abs/1609.00606

---

## Verification Flags Remaining

- Veeva field names and CLM object details need partner documentation or official Veeva documentation before chapter drafting. Public search did not produce stable official pages for all field names.
- Any claim about "15-year natural experiment" should be framed as a book thesis/hypothesis unless supported by partner timeline evidence.
- Any dollar estimate for pharma promotion or rep spending should be re-sourced from a primary or high-quality industry report during chapter accuracy review.
- Act III depends on the unresolved partner/IP/consent framing described in `TIKTOC.md` Risk 1.

