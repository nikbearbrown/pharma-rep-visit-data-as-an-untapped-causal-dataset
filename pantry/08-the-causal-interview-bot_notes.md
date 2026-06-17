# Chapter 8 — The Causal Interview Bot (research notes)

*Eliciting a rep's tacit causal knowledge through a ladder-structured chatbot: the rep talks like a rep,
the system extracts a structured causal prior. KEBN elicitation, Feigenbaum's bottleneck, Rung-1/2/3
question design, output = annotated prior DAG. Gather-only — not chapter prose.*

Sources used throughout: `pantry/rep-visit-as-causal-dataset_source.md` (the source essay, §"The Causal
Interview Bot," §"The epistemic gap," §"Markov equivalence"); `pantry/living-models.md` (Ch 7 Concept 3
"Why Knowledge Is Mathematically Necessary"; the "Expert in the Room" principle; KEBN handoff to the
CPDAG); web search (cited inline). `[verify]` flags where a specific claim wasn't pinned to a primary text.

> Format note: this file uses the mandated A–G structure. A concurrent agent briefly overwrote it with a
> shorter A–E version on 2026-06-17; useful pointers from that version are reconciled in Section G (some of
> its arXiv IDs need verification before use).

---

## A. Foundations

**The problem this chapter solves.** Observational telemetry records *what happened*, never *why*. The
source's anchor example: `Duration_vod = 47s` on the safety slide is consistent with at least three
incompatible causal stories — (1) compelled/reading carefully (educationally positive), (2)
skeptical/hunting the flaw (negative, mis-logged as neutral), (3) polite-but-distracted (noise). Identical
data, opposite causal inferences. The experienced rep who has called on Dr. Martinez 40 times knows which
story is true; that knowledge is *structural information the data cannot contain* and it evaporates on
territory change. (Source essay, §"The epistemic gap.")

**Why elicitation is mathematically necessary, not a nicety.** From `living-models.md` Ch 7 Concept 3 and
the source's Markov-equivalence section: observational data identifies only the **Markov-equivalence class**
(the CPDAG), never a unique DAG (Verma–Pearl). Whenever the class has >1 member, *no procedure operating on
the data alone can choose among them* — the choice must come from outside the data. The expert is therefore
not validating the model; she is **constructing** it (Pearl's role-inversion: in ML the data scientist is
the primary actor; in causal modeling the domain expert is). `living-models.md` names this the **"Expert in
the Room" principle**: every causal model needs an expert, and every architecture must make space for one.
The rep interviews are *the source of structural information, not supplementary data.*

**KEBN — Knowledge Engineering with Bayesian Networks.** The discipline of structured expert elicitation
for building Bayesian/causal networks. Canonical text: **Korb & Nicholson, *Bayesian Artificial
Intelligence*** (Chapman & Hall/CRC; 1st ed. 2004, 2nd ed. 2010, 3rd ed. 2023). The book's Part on
"Knowledge Engineering with Bayesian Networks" prescribes building the network through elicitation from
domain experts and combining elicited structure with what is learned from data. Their methodological
recommendation: **proceed iteratively and incrementally — start with a small local structure around the
target variable, not an exhaustive enumeration of every possible factor**
([Korb & Nicholson, Google Books summary](https://books.google.com/books/about/Bayesian_Artificial_Intelligence.html?id=LxXOBQAAQBAJ);
[Routledge 2nd ed.](https://www.routledge.com/Bayesian-Artificial-Intelligence/Korb-Nicholson/p/book/9781032477657)).
A KEBN elicitation produces both **structure** (nodes + edges) and **parameters** (CPTs / conditional
probabilities) — but structure is the load-bearing output for this chapter (parameters get filled in Ch 9).

**Feigenbaum's knowledge bottleneck.** Edward Feigenbaum named the *knowledge-acquisition bottleneck*: the
construction of expert systems is choked by the difficulty of transferring an expert's knowledge into a
formal system, because the knowledge engineer knows far less of the domain than the expert and the expert's
knowledge is rarely formalized
([Feigenbaum, "Knowledge Acquisition: The Bottleneck," 1982 — Stanford archive](https://exhibits.stanford.edu/feigenbaum/catalog/sq764cf8300);
he coined "knowledge engineering" in 1977 and the bottleneck framing followed; [overview](http://penta.ufrgs.br/edu/telelab/10/kss01.htm)).
The chapter localizes the bottleneck to three concrete failure modes (named in the source essay and TIKTOC
Ch 8 Core):
- **CPT explosion** — eliciting every conditional probability for a node with many parents is
  combinatorially infeasible; a node with k binary parents needs 2^k entries. (Standard KEBN problem;
  Korb-Nicholson mitigate with canonical models like noisy-OR. `[verify]` exact term usage to Korb-Nicholson.)
- **Linguistic ambiguity** — experts speak in hedged natural language ("she usually comes around once the
  formulary clears"); mapping that onto edges/probabilities is lossy.
- **Recognition–explanation gap** — experts can *recognize* the right action without being able to
  *explain* the rule (tacit knowledge; cf. Polanyi's "we know more than we can tell" `[verify]` attribution
  in this book's framing). The rep knows Dr. Martinez will switch; she can't articulate the production rule.

**The design move: don't make the rep speak the formalism.** The chatbot is structured on Pearl's Ladder,
but the rep *never hears* "confounder / mediator / collider / counterfactual." She narrates visits; the
system does the translation. This is the chapter's central craft claim — elicitation succeeds by letting the
expert stay in her own register and pushing the formalization burden onto the system. (Source essay,
§"The Causal Interview Bot.")

### Misconception (the one to invert)
"Rep notes are soft, anecdotal, second-class data — nice color, not model input." (TIKTOC Part 2,
misconception #4.) **Wrong, and the error is structural.** Because data identifies only a Markov-equivalence
class, the rep's edge-orientation knowledge is the *only* admissible source of the missing structural
information (short of an RCT). The rep saying "I've watched Dr. Johnson switch within two weeks of a
colleague switching — never the other way" is *reporting the result of an intervention she has observed many
times but never formally ran* (`living-models.md` Ch 7: expert knowledge substitutes for interventional
data). It is first-class structural input, not anecdote. (A useful courtroom analogy: a witness does not
write the legal theory, but her testimony establishes facts no camera captured.)

### Worked example (on this dataset) — the three-rung interview
From the source essay, the bot's question ladder and what each rung extracts:

- **Rung 1 (association).** *"Walk me through the last visit with Dr. Martinez — what did she say at the
  safety data?"* → NLP extracts **candidate confounders**: formulary status, competitive detailing, a prior
  colleague switch, the practice's patient mix. (These are nodes that may affect both the message delivered
  and the prescribing — backdoor candidates.)
- **Rung 2 (intervention).** *"If you led with patient-outcomes data instead of mechanism, what would
  happen? Is there anything that would* definitely *move her?"* → extracts **susceptibility indices**:
  heterogeneous treatment-effect knowledge ("efficacy-first works on the academics, safety-first on the
  community docs") that exists in no database.
- **Rung 3 (counterfactual).** *"That account that converted — would it have without the MSL visit? The
  ones that didn't — what would you have done differently?"* → maps to **mediator weights**: how much of
  the effect ran through education (M1) vs relationship maintenance (M2) vs peer/reciprocity (M3).

**Output = an annotated prior DAG (structured prior).** A draft graph with candidate nodes and edges, each
edge tagged with: (a) the **rep evidence** that supports it (a quoted observation), (b) a **confidence**
level, and (c) any **contradictions to resolve** (two reps, or one rep across accounts, asserting opposite
orientations). This is the KEBN deliverable and the chapter's assessed artifact (TIKTOC Part 7: "Causal
Interview Bot transcript + elicited prior DAG," 105 pts, Week 8). The DAG then becomes the input to Ch 9's
parameterized SCM and the prior that constrains/orients the CPDAG from Ch 7.

### Source anchor
TIKTOC WEEK 8 entry; source essay §"The Causal Interview Bot," §"The epistemic gap," §"Markov equivalence";
`living-models.md` Ch 7 Concept 3 + "Expert in the Room"; the KEBN handoff (`living-models.md` Ch 7
Integration: "the CPDAG is the handoff between the elicitation step and the data-driven refinement step").

---

## B. Examples + failure case

**Positive examples / analogues.**
- **Medical & ecological Bayesian nets built by elicitation** — Korb & Nicholson document that *most*
  fielded BN applications were built by eliciting structure from domain experts before any data fitting.
- **BARD (Bayesian ARgumentation via Delphi)** — a structured *group* elicitation technique for building
  Bayesian networks to support analytic reasoning; relevant because the rep-knowledge case is inherently
  multi-expert (many reps, many territories). ([Nicholson et al., BARD, PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC9290058/);
  [arXiv:2003.01207](https://arxiv.org/abs/2003.01207).) Useful as a model for **how to reconcile
  contradictory reps** (the "contradictions to resolve" field in the prior DAG).
- **Korb-Nicholson incremental rule** — start local around the target (NRx), expand outward — is the
  practical antidote to CPT explosion and a concrete bot-design constraint.
- **Two concrete transcript cases** (good chapter material): (1) an *objection-handling* transcript — rep
  describes a payer objection, the message used, the physician reaction, later patient starts → yields the
  edge chain objection → education → confidence → prescribing, tagged rep-reported; (2) a *peer-influence*
  transcript — rep describes a local KOL shifting an institutional norm → bot tags a peer/KOL edge with
  explicit uncertainty.

**Failure case (the chapter must dramatize one).** The **leading-question / confirmation failure**: a bot
that asks "So the meal is what tipped Dr. Lee, right?" elicits a spurious M3 edge the rep would never have
volunteered. Elicitation imports the *interviewer's* causal assumptions, not the expert's — exactly the
implicit-assumption substitution `living-models.md` Ch 7 warns against, now committed by the bot instead of
the analyst. Mitigation: open-ended Rung-1 prompts first, contradiction probes, and never naming the
hypothesized edge in the question. Second failure mode: **the rep confabulates a clean rule when the truth
is tacit** (recognition–explanation gap) — the bot must capture the *recognition* ("you'd know it when you
saw it?") and tag the edge low-confidence rather than forcing a stated rule.

---

## C. Connections

- **← Ch 7 (Markov equivalence):** Ch 7 proves *why* elicitation is necessary (data → class, not graph);
  Ch 8 is *how* you do it. The bot's output orients the undirected edges of Ch 7's CPDAG.
- **→ Ch 9 (priors to Living Model):** the annotated prior DAG is Ch 9's structural input; confidence tags
  become priors on edge existence; the susceptibility indices seed heterogeneous-effect parameters.
- **→ Ch 6 (colliders):** elicitation must *not* invent edges into post-treatment colliders (dwell,
  reaction); the bot should flag in-visit signals as downstream-of-message, not as confounders.
- **→ Ch 13 (consent collider):** reps may also hold knowledge about *who opts out and why* — useful for
  modeling the consent/selection structure with FCI.
- **Framework:** this is the KEBN node of the Living Model pipeline (`living-models.md` Part Three:
  elicitation → CPDAG refinement → parameterization).

---

## D. Current state / refs / last-3-yr

**Foundational (verified):**
- Feigenbaum, E.A. (1982). *Knowledge Acquisition: The Bottleneck* — the canonical bottleneck statement;
  "knowledge engineering" coinage 1977.
  [Stanford archive](https://exhibits.stanford.edu/feigenbaum/catalog/sq764cf8300).
- Korb, K.B. & Nicholson, A.E. *Bayesian Artificial Intelligence* (1st 2004 / 2nd 2010 / 3rd 2023),
  Chapman & Hall/CRC — KEBN, the knowledge-engineering bottleneck framing, incremental local-structure
  elicitation. [Routledge](https://www.routledge.com/Bayesian-Artificial-Intelligence/Korb-Nicholson/p/book/9781032477657).
- Nicholson, A.E. et al. (2020/2022). **BARD** — structured group elicitation of Bayesian networks.
  [PMC](https://pmc.ncbi.nlm.nih.gov/articles/PMC9290058/) / [arXiv:2003.01207](https://arxiv.org/abs/2003.01207).
- Verma & Pearl (1990) — Markov-equivalence theorem (cited via `living-models.md`; pin primary
  `[verify]` for full bibliographic entry).

**Recent (LLM-assisted elicitation, 2024–2025) — and its risks:**
- LLMs are now used as **priors / hypothesis-generators for causal graphs**: prompt the model edge-by-edge
  or three-variables-at-a-time to propose orientations, then validate against data or as a posterior
  correction on statistical discovery output.
  ([Kıcıman et al. landscape — "LLMs for Causal Discovery: Current Landscape and Future Directions,"
  arXiv:2402.11068](https://arxiv.org/html/2402.11068v2);
  ["LLMs for Constraint-Based Causal Discovery," arXiv:2406.07378](https://arxiv.org/pdf/2406.07378);
  ["Bayesian Network Structure Discovery Using LLMs," arXiv:2511.00574](https://arxiv.org/pdf/2511.00574).)
- **The risk is hallucinated edges.** A dedicated 2024 paper targets *eliminating hallucinations in
  LLM-assisted causal discovery* — the model will assert plausible-sounding causal claims with no grounding
  ([arXiv:2411.12759](https://arxiv.org/pdf/2411.12759)). Mitigations in the literature: systematic
  verification, structured/constrained prompting, argumentation-driven approaches, fine-tuning for causal
  reasoning. **For this book the distinction is sharp:** the LLM should *transcribe and structure the rep's
  knowledge* (translate her language into candidate edges), **not supply causal claims from its own training
  prior.** An LLM edge with no rep-quoted evidence in the annotated DAG is a hallucination by definition and
  must be rejected at the "evidence" field. This is the chapter's main "When NOT to use AI" point.

**Live empirical question (TIKTOC Part 11):** "Rep knowledge can orient causal edges" is *defensible in
principle* (Verma–Pearl) but the *quality of elicitation is the open empirical question.* And TIKTOC Open
Question #4: real reps vs role-play for the elicitation chapter — affects credibility.

---

## E. Teaching

- **Assessed artifact (Week 8, 105 pts):** run an interview (real rep role-play or transcript) → produce the
  elicited prior DAG with nodes/edges + evidence + confidence + contradictions.
- **Where students get stuck:** they either *overtrust* reps (treat every rep claim as ground truth) or
  *dismiss* them (treat notes as anecdote). The chapter must hold both: rep testimony establishes structural
  facts no data can, *and* it must be cross-checked for contradictions and confidence-tagged.
- **Bloom ladder (from TIKTOC LOs):** Create (write Rung-1/2/3 prompts in rep-natural language) → Apply
  (extract confounders/susceptibility/mediator-weights from a transcript) → Evaluate (assemble the
  annotated prior DAG, surface contradictions).
- **Five-part AI block angle:** *When to use AI* = transcription + first-pass structure extraction +
  contradiction surfacing across transcripts. *When NOT* = letting the LLM assert edges from its own prior;
  any edge without rep evidence. *AI validation* = check every machine-proposed edge against a rep quote;
  hold out a transcript and see if the bot recovers the same structure.
- **Exercise seeds:** (1) given a 47-second-slide vignette, write the three Rung-1 follow-ups that
  disambiguate the three stories; (2) take a messy rep transcript, mark candidate confounders vs mediators
  vs colliders; (3) two reps disagree on an edge orientation — design the contradiction-resolution probe;
  (4) convert a mock rep transcript into an annotated prior DAG with confidence labels.

---

## F. Library files

- `_lib_counterfactuals-book-of-why_excerpt.md` — Rung-3/SCM backdrop (more central to Ch 9, but the
  ladder framing of the bot is shared).
- `MD/math-bayesian-reasoning-in-data-analysis-a-critical-introduction.md`,
  `MD/math-bayesian-statistical-methods.md`, `MD/math-bayesian-inference.md` — Bayesian/CPT background for
  the KEBN parameterization vocabulary (light touch; pull only the prior/likelihood framing if needed).
- `MD/science-the-book-of-why-...md` — Pearl's ladder + the "expert orients edges" argument (OCR-degraded;
  prefer `living-models.md` Ch 5/7).
- In-repo framework: `living-models.md` Ch 7 Concept 3 + "Expert in the Room" + KEBN handoff; Ch 5 (ladder).
- *No dedicated KEBN / Korb-Nicholson / Feigenbaum text exists in `MD/`* — those come from web search only
  (flagged below).

---

## G. Gaps / flags

- `[verify]` **Korb & Nicholson** specific page/chapter for "knowledge-engineering bottleneck" wording and
  the incremental-local-structure rule — confirmed via Google Books / publisher summary, not the page text.
- `[verify]` **noisy-OR / canonical models** as the named CPT-explosion mitigation attributed to
  Korb-Nicholson — standard KEBN content but not pinned to a page.
- `[verify]` **Polanyi "tacit knowledge / we know more than we can tell"** if the chapter invokes it for the
  recognition–explanation gap — source essay uses "recognition-explanation gap" without naming Polanyi.
- `[verify]` **Verma & Pearl (1990)** full citation for the equivalence theorem.
- **VERIFIED (was flagged):** Shaposhnyk, Zahorska & Yanushkevich (2025), "Can LLMs Assist Expert
  Elicitation for Probabilistic Causal Modeling?", [arXiv:2504.10397](https://arxiv.org/abs/2504.10397)
  — confirmed on arXiv (cs.AI, submitted 14 Apr 2025; LLM-generated Bayesian networks benchmarked vs
  expert elicitation, with a hallucinated-dependencies caveat). Safe to cite; directly on-topic for the
  LLM-assisted-elicitation discussion above.
- `[verify — citation removed: 2602.01483 unconfirmed]` A claimed "causal preference elicitation 2026"
  (arXiv:2602.01483) could not be confirmed on arXiv — the abstract page returns no author/date metadata
  and the 2602.* (Feb 2026) prefix matches the concurrent draft's own fabrication flag. Citation removed;
  do not use. No verified replacement substituted.
- **Open (empirical, not literature):** elicitation *quality* — does the bot recover the rep's true
  structure? No benchmark on rep-visit data exists (this would be original contribution). Tie to TIKTOC
  Risk: "quality of elicitation is the open empirical question."
- **Ethics flag:** rep knowledge about *which physicians are persuadable / opt out* edges toward profiling;
  route the consent implications to Ch 13. Reps describing physician psychology is fine for educational-path
  modeling; do not let it become a targeting list that the regulatory line (Ch 5/13) forbids.
- **No proprietary data used** — interview prompts and the synthetic transcript are public/synthetic; the
  partner runs real elicitation internally (IP firewall).
