# Chapter 9 — From Priors to a Living Model (research notes)

*Combining the elicited prior + identified effects + data into a parameterized SCM; the counterfactual
procedure (abduction → action → prediction); the Living Model architecture (SCM + counterfactual engine +
structural-change/drift monitoring, esp. opt-out-rate drift); the Rung-3 individual recommendation —
"which message for THIS physician next week." Gather-only.*

Sources: `pantry/rep-visit-as-causal-dataset_source.md` (§"The data as a Living Model problem,"
§"Counterfactual layer (Rung 3)," §"Consent architecture caveat"); `pantry/living-models.md` (Ch 9 "The
Counterfactual" — abduction/action/prediction, worked example, three granularities, PN/PS; Ch 6 DAG→SCM;
the Living Model architecture); web search (Pearl SCM/counterfactual; cited inline). `[verify]` where unpinned.

> Format note: this file uses the mandated A–G structure. A concurrent agent briefly overwrote it with a
> shorter A–E version on 2026-06-17; useful pointers from that version are reconciled in Section G (some
> arXiv IDs there need verification before use).

---

## A. Foundations

**The transition the chapter makes.** Ch 4 (IV) and Ch 10 (staggered DiD) deliver a *population* / Rung-2
estimate: the average treatment-on-treated effect of a message variant on NRx. But the decision a brand
actually faces is Rung-3 and *individual*: **"for THIS physician, which message sequence next week?"** The
opening contrast (TIKTOC Ch 9 Opening): the population IV estimate vs the decision the brand needs.
A Living Model closes that gap by assembling three ingredients into a parameterized SCM and running
counterfactuals on it. (The common student trap is conflating the population ATE with the individual
recommendation — see Misconception below.)

**The three ingredients combined.**
1. **The elicited prior DAG** (Ch 8) — structure: nodes, oriented edges, evidence/confidence tags. Orients
   the edges the data cannot (Markov-equivalence; the KEBN handoff in `living-models.md` Ch 7).
2. **The identified effects** (Ch 4 IV / Ch 10 DiD / Ch 11 causal forest + DML) — the magnitudes the
   quasi-experiments license you to believe.
3. **The data** (the assembled physician-by-time panel from Ch 2) — fits the conditional probabilities /
   structural-equation parameters and supplies each physician's history for abduction.

**Parameterizing the SCM.** A DAG becomes a structural causal model when each node gets a structural
equation `Xᵢ = fᵢ(parents(Xᵢ), Uᵢ)` with an exogenous noise term `Uᵢ` (`living-models.md` Ch 6
"From DAG to Structural Causal Model"; Ch 9). In the KEBN/Bayesian-net rendering this is **edges + CPTs +
provenance/confidence** (TIKTOC Ch 9 LO: "parameterize the SCM (KEBN: edges, CPTs, provenance/confidence)").
Each edge should carry **provenance** — data-estimated, rep-elicited, literature-supported, or assumed —
plus an uncertainty. The provenance tag is what makes the model auditable: every edge orientation traces to
either data, an identified effect, or a named rep observation (the Living Model's auditability claim,
`living-models.md` Ch 7 Implication 2). Where edges remain unresolved, **sensitivity analysis over the
equivalence class** is the honest output, not a forced single graph (`living-models.md` Ch 7 Concept 3
worked example).

**The counterfactual procedure (the engine).** From `living-models.md` Ch 9 and Pearl's three steps
([abduction-action-prediction, verified](https://arxiv.org/pdf/2301.02499)):
- **Step 1 — Abduction.** From the observed evidence for a specific physician (her history, her rep's
  elicited read, prior NRx), infer the exogenous noise `P(U | evidence)`. This is where *individual*
  particularity enters — the physician's idiosyncratic responsiveness that distinguishes her from the panel
  average.
- **Step 2 — Action.** Apply `do(message = m')` — surgically set the message/sequence to the candidate,
  leaving all other structural equations intact (the do-operator replaces the structural equation, not the
  whole model).
- **Step 3 — Prediction.** Run the modified SCM forward with the recovered `U` to compute counterfactual
  NRx under each candidate message. Rank candidates → the recommendation.

This is a **pre-factual** individual counterfactual (evaluated *before* acting; conditions on the
physician's pre-action characteristics) — distinct from the *retrospective* "would that account have
converted without the MSL visit?" the rep is asked in Ch 8 (`living-models.md` Ch 9 pre-factual vs
retrospective table).

**The three granularities (why individual is the payoff).** Population ACE (no abduction, Rung-2 reachable)
→ subgroup CATE (causal forest, Ch 11) → **individual** (full abduction, the hardest and most valuable;
`living-models.md` Ch 9). The "research finding" for Fellows (TIKTOC): the Rung-3 individual recommendation
is the operational payoff *no NBA engine produces* — NBA engines stop at Rung-1 propensity.

**The Living Model architecture (three components).** From the framework (`living-models.md` — the SCM +
counterfactual + monitoring spine) and the source essay:
1. **Parameterized SCM** (the model of mechanism).
2. **Counterfactual engine** (abduction-action-prediction → individual recommendations).
3. **Structural-change / drift monitoring** — the model is "living" because it watches for the world
   shifting under it. The book's signature drift target: **opt-out-rate drift.** When physician
   opt-out rates change, the *selection structure* changes (consent is a Berkson/collider on the sampling
   frame — Ch 6/13), so the bias structure of every downstream estimate changes. The drift monitor watches
   opt-out-rate shifts specifically, not just covariate drift. (Source essay §"Consent architecture caveat":
   "the drift monitor watches opt-out-rate shifts that change the bias structure.") Other live drift
   triggers: loss-of-exclusivity (LOE), formulary changes — anything that makes pre-shift parameters
   non-transportable.

### Misconception (to invert)
"A good population effect estimate is the deliverable — once you have the IV/DiD number, you're done." Or
its twin: "a DAG alone is the model." **Wrong on both counts:** the population number is Rung-2 background,
the *decision* is a Rung-3 individual counterfactual, and reaching it strictly requires the SCM (DAG **plus**
structural equations **plus** noise) and abduction. A correlation/effect engine cannot do abduction — it has
no `U` to recover. (`living-models.md` Ch 9 "The Living Model Connection.")

### Worked example (on this dataset)
Schematic adaptation of `living-models.md` Ch 9's marketing example to rep-visit data (use as the chapter's
worked case; numbers illustrative `[verify]` — to be regenerated on the synthetic dataset):
- SCM (simplified): `Message = U_M`; `Knowledge = α·Message + U_K`; `NRx = β·Knowledge + U_Rx`, with
  α, β estimated from the identified effects.
- Observe Dr. Martinez: delivered safety-first, modest knowledge lift, NRx below what α,β predict.
- **Abduction** recovers her `U_K`, `U_Rx` (she over-responds on knowledge, under-converts to scripts —
  matches the rep's read: "she gets it but is formulary-blocked").
- **Action:** `do(message = efficacy-first sequence)`.
- **Prediction:** carry her recovered noise forward → counterfactual NRx under efficacy-first vs status quo.
  Rank → recommendation for *next week*. The case-specific noise is what makes this her recommendation, not
  the panel average's.

(A clean teaching analogy: a weather model ingests *current* conditions before simulating tomorrow —
abduction is the "read the current state" step that population averages skip.)

### Source anchor
TIKTOC WEEK 9; source essay §"The data as a Living Model problem" + §"Counterfactual layer (Rung 3)" +
§"Consent architecture caveat"; `living-models.md` Ch 9 (counterfactual procedure, granularities,
pre/retrospective, PN/PS) + Ch 6 (DAG→SCM) + Ch 7 (sensitivity over equivalence class).

---

## B. Examples + failure case

**Positive analogues.**
- `living-models.md` Ch 9 board-chair / marketing-campaign worked counterfactual — the structural template
  the pharma case instantiates.
- **PN/PS → persuadables.** Probability-of-necessity maps cleanly onto the Ch 12 reframe: a *sure thing*
  has low PN (would prescribe without the message); a *persuadable* has high PN for the message. The
  counterfactual engine *is* the persuadables ranker. (`living-models.md` Ch 9 necessity/sufficiency.)
- **Individual message choice** (chapter case): use prior visits + prescribing to infer the physician's
  state (abduction), then compare next-message counterfactuals (e.g., a mid-tier cardiologist with access
  concerns — predicted NRx under an access-education message vs an efficacy message).

**Failure cases.**
1. **Over-claiming point identification.** If the SCM is nonlinear or `U` is not uniquely recoverable, the
   individual counterfactual is only *partially identified* — bounded, not pinned (`living-models.md` Ch 9
   caveat). A Living Model that emits a confident single NRx number where only bounds are identified is
   lying. Honest output: a range + the assumption it depends on.
2. **Silent generalization off the consenting subsample.** TIKTOC Ch 13 opening, surfaced here: a model
   fit/abducted on consenting physicians and applied to all generalizes wrong, *and gets worse over time as
   opt-out rates drift* — which is exactly why drift monitoring on opt-out rate is architectural, not
   optional. (Source essay §"Consent architecture caveat.")
3. **Static causal model / stale priors.** Without drift checks, a model trained before an LOE or formulary
   change recommends obsolete messages; if the elicited prior (Ch 8) ages out (territory turnover, new
   competitive landscape), the SCM's orientations rot. The drift monitor must flag *structural* change, not
   just parameter drift.

---

## C. Connections

- **← Ch 8:** the annotated prior DAG is the structural input + confidence priors.
- **← Ch 4 / Ch 10 / Ch 11:** identified effects supply the parameters α, β, heterogeneity.
- **← Ch 6 / Ch 13:** the consent collider drives the drift target (opt-out-rate monitoring).
- **→ Ch 12:** the counterfactual engine produces the persuadables ranking (PN-based).
- **→ Ch 13:** the prototype + handoff brief; FCI for the consent/latent structure; regulatory line
  constrains which pathway's counterfactual you're allowed to act on (educational M1 only).
- **Framework:** this *is* the Living Model — the chapter where the book's title pays off
  (SCM + counterfactual engine + drift monitoring, the `living-models.md` Part Three architecture).

---

## D. Current state / refs / last-3-yr

- **Pearl, J.** — structural causal models, the do-operator, the three-step counterfactual
  (abduction/action/prediction). Primary: Pearl, *Causality* (2009, 2nd ed.); Pearl & Mackenzie,
  *The Book of Why* (2018). Three-step procedure cleanly restated and applied in recent work
  ([arXiv:2301.02499 — Pearl's counterfactual method](https://arxiv.org/pdf/2301.02499);
  [Pearl's Causal Hierarchy overview](https://www.emergentmind.com/topics/pearl-s-causal-hierarchy-pch)).
- **Bareinboim, Correa, Ibeling, Icard — "Pearl's Causal Hierarchy" (PCH) / Causal Hierarchy Theorem**
  formalizes why each rung is strictly more than the one below — undergirds "abduction strictly requires the
  SCM." `[verify]` exact citation (On Pearl's Hierarchy and the Foundations of Causal Inference, 2022).
- **Recent (parameterization + LLM-assisted):** LLMs being used to *parameterize/seed* causal models and
  improve Bayesian causal discovery — relevant to filling CPTs / priors from elicited text, with the same
  hallucination caveat as Ch 8 ([LLMs to improve Bayesian causal discovery, OpenReview](https://openreview.net/pdf?id=k3qX5G46zw)).
  Keep the firewall: data + identified effects parameterize; LLM only structures elicited language.
- **Drift / structural-change monitoring** is the Living Model framework's own contribution
  (`living-models.md` Ch 2 "The Map That Doesn't Move," Ch 3 latency); the *opt-out-rate* specialization is
  this book's original move.

---

## E. Teaching

- **Assessed (terminal, Week 13, 200 pts):** Living-Model targeting prototype + product-handoff brief —
  Ch 9 builds the engine that the final delivers.
- **Where students get stuck:** conflating the population ATE with the individual recommendation (the whole
  point of abduction is to leave the population average behind).
- **Bloom (TIKTOC LOs):** Create (parameterize the SCM) → Apply (run an individual counterfactual,
  abduction→action→prediction, on a synthetic physician) → Evaluate (specify drift monitoring, esp.
  opt-out-rate).
- **Exercise seeds:** (1) given a 3-equation rep-visit SCM and one physician's observed (message, knowledge,
  NRx), run all three steps by hand and compare to the no-abduction population prediction (mirrors
  `living-models.md` Problem 9.1); (2) classify "which message next week?" vs "would she have converted
  without the meal?" as pre-factual vs retrospective; (3) build a mini-SCM with three edges, two parameter
  sources (one data-estimated, one rep-elicited), and one drift trigger; (4) design the opt-out-rate drift
  alarm — what threshold, what does it trigger (refit? re-elicit? FCI re-spec?).
- **Five-part AI block:** *Use AI* to scaffold the SCM code and run sensitivity sweeps over unresolved
  edges; *Don't* let it invent structural equations or parameters; *Validation* = recover known
  ground-truth effects on the synthetic dataset before trusting any real run.

---

## F. Library files

- `_lib_counterfactuals-book-of-why_excerpt.md` — **primary** library support for this chapter (ladder,
  abduction/action/prediction, three granularities, PN/PS, identification caveat).
- `_lib_did-applied-causal-inference_excerpt.md` — the identified-effects ingredient (population estimate
  feeding the SCM parameters).
- `MD/math-bayesian-*` (reasoning / statistical-methods / inference) — CPT / prior-likelihood vocabulary for
  the KEBN parameterization.
- `MD/ai-applied-causal-inference-powered-by-ml-and-ai.md` — DML for the conditional-effect parameters.
- In-repo: `living-models.md` Ch 9 (counterfactual), Ch 6 (DAG→SCM), Ch 7 (sensitivity), Ch 2–3 (drift/
  latency for the monitoring component).

---

## G. Gaps / flags

- `[verify]` worked-example numbers — illustrative; regenerate on the synthetic rep-visit dataset with known
  ground truth (TIKTOC Feature: synthetic dataset; Open Question #2 on its structure).
- `[verify]` Bareinboim et al. PCH / Causal Hierarchy Theorem full citation if the chapter formalizes
  "abduction needs the SCM."
- **VERIFIED (was flagged):** Ness, Paneri & Vitek (2019), "Integrating Markov processes with structural
  causal modeling enables counterfactual inference in complex systems,"
  [arXiv:1911.02175](https://arxiv.org/abs/1911.02175) — confirmed on arXiv; casts an equilibrium Markov
  process model as an SCM and runs Pearl's abduction-action-prediction counterfactual procedure (NeurIPS
  2019 causal-ML workshop). Directly supports this chapter's SCM/counterfactual spine; safe to cite.
  Bottou et al., "Counterfactual Reasoning and Learning Systems" (arXiv:1209.2355) is also real and is
  in fact reference [7] of the Ness paper — confirmed, safe to cite.
- `[verify]` exact mechanism by which the framework's drift monitor detects *structural* (vs parametric)
  change — `living-models.md` Ch 2/3 cover drift conceptually; the opt-out-rate specialization is novel and
  has no external benchmark yet.
- **Identification honesty flag:** the individual counterfactual is point-identified only under full SCM
  specification + recoverable noise; otherwise bound it. Pervasive caveat per the book's "assumptions stated
  every chapter" discipline.
- **Regulatory flag (→ Ch 13):** the counterfactual engine could rank messages on the reciprocity pathway
  (M3) — legally non-drivable. Constrain the *action* step to the educational pathway (M1) for any
  deployable recommendation; flag M2/M3 counterfactuals as analysis-only.
- **No proprietary data** — prototype on synthetic/public; partner replicates internally (IP firewall).
