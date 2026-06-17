# Chapter 8: The Causal Interview Bot

## 1. Overview

Chapter 7 left you with a CPDAG: a graph the data settled partway, full of undirected edges that no dataset can ever orient. The theorem says only outside knowledge can point those arrows, and on this dataset the outside knowledge lives in the heads of reps who have called on the same physicians forty times. This chapter is about getting it out.

The naive way to get structural knowledge out of an expert is to sit her down and ask her to draw a causal graph. That fails on contact: she does not think in nodes and edges, she thinks in Dr. Martinez and the safety slide and the formulary that cleared last spring. The craft of this chapter is to build a **Causal Interview Bot** that lets the rep stay entirely in her own register — narrating visits, accounts, objections — while the *system* does the translation into oriented edges, candidate confounders, and mediator weights. The rep never hears the word "confounder." The bot is structured on Pearl's three rungs without ever naming them.

When you finish, you will be able to write ladder-structured interview prompts in rep-natural language, extract candidate confounders / susceptibility indices / mediator weights from a transcript, and assemble an **annotated prior DAG** — a draft graph where every edge carries the rep evidence that supports it, a confidence level, and any contradictions to resolve. That artifact is the bridge between Chapter 7's open edges and Chapter 9's parameterized model. It is also the most important honesty test in the book, because the same LLM that transcribes the rep can just as easily hallucinate structure she never asserted — and we will spend real time on the difference.

---

## 2. Learning objectives

By the end of this chapter, a Fellow will be able to:

- **(Create)** Write ladder-structured interview prompts — Rung 1 (association), Rung 2 (intervention), Rung 3 (counterfactual) — in rep-natural language that never names a causal formalism.
- **(Apply)** Extract candidate confounders, susceptibility indices, and mediator weights from a rep transcript and map each to a node or edge.
- **(Evaluate)** Turn extracted structure into an annotated prior DAG, tagging every edge with evidence, confidence, and contradictions.
- **(Evaluate)** Distinguish a rep-grounded edge (supported by a quoted observation) from an LLM-hallucinated edge (asserted from the model's training prior) and reject the latter.
- **(Understand)** Explain Feigenbaum's knowledge-acquisition bottleneck and its three concrete failure modes on this elicitation: CPT explosion, linguistic ambiguity, and the recognition–explanation gap.

---

## 3. Opening case (failure-first)

A Fellow builds a first interview bot and tests it on a willing rep. The transcript reads well. Then the Fellow inspects the elicited graph and finds an edge he is sure the rep would be proud of: `MEAL → PRESCRIBING`, tagged "rep-confirmed," high confidence.

He pulls the transcript to find the supporting quote. Here is the exchange:

> **Bot:** So the lunch you bought Dr. Lee's office — that's really what tipped her over to writing the script, right?
> **Rep:** Yeah, I mean... the timing lined up, sure, the lunch was right before she started writing.

The rep never volunteered the meal as a cause. The bot *proposed* it — "that's really what tipped her over, right?" — and the rep, being agreeable and uncertain, did not push back. The edge in the graph is not the rep's structural knowledge. It is the *interviewer's* causal assumption, laundered through a leading question and stamped "rep-confirmed."

This is the failure mode that matters most in elicitation, and it is worse with an LLM-driven bot than with a human interviewer, because the LLM has a strong prior of its own. Ask a language model about meals and prescribing and it "knows," from its training data, that the two are associated — so left unconstrained it will *steer* toward confirming the edge it already expects. The bot did not extract the rep's knowledge. It imported its own and got a nod.

Worse, the imported edge lands on the reciprocity pathway (M3) — the legally non-drivable one (Chapter 5). A spurious M3 edge is not just a measurement error; it is a compliance liability waiting to be acted on. The rest of this chapter is, in large part, the discipline that prevents this exchange: open-ended Rung-1 prompts before any hypothesis, never naming the hypothesized edge in the question, and an evidence field that rejects any edge without a quote the rep volunteered.

---

## 4. Core sections

### 4.1 The problem: telemetry records what, never why

Recall the anchor example from earlier in the book. The iPad logs `Duration_vod = 47s` on the safety slide. That single number is consistent with at least three incompatible causal stories:

1. **Compelled / reading carefully** — she is absorbing the safety data; this is educational engagement, a positive signal on the M1 pathway.
2. **Skeptical / hunting the flaw** — she is stuck on the slide because she distrusts it; this is *negative*, and the rep's in-the-moment read may even have been mis-logged as a neutral `Reaction_vod`.
3. **Polite but distracted** — she is half-listening; the 47 seconds is noise.

Identical data, opposite causal inferences. The experienced rep who has called on Dr. Martinez forty times *knows which story is true* — she watched her face. That knowledge is **structural information the data cannot contain**, and it evaporates the moment the rep changes territory. The interview bot exists to capture it before it walks out the door.

### 4.2 Why elicitation is construction, not validation

Chapter 7 proved the necessity; here is the consequence for how you treat the rep. Because observational data identifies only the Markov-equivalence class, and because choosing among class members requires information from outside the data, **the expert is not checking your model — she is building it.** This is Pearl's role inversion: in ordinary machine learning the data scientist is the primary actor and the domain expert is consulted for a sanity check; in causal modeling the domain expert is the primary actor and supplies the structure the data cannot. The framework book names this the **"Expert in the Room" principle**: every causal model needs an expert, and every architecture must leave a seat for one. The rep saying *"I've watched Dr. Johnson switch within two weeks of a colleague switching — never the other way"* is reporting the result of an intervention she has observed many times but never formally ran. That is structural input, not anecdote.

> **The misconception this chapter inverts.** "Rep notes are soft, anecdotal, second-class data — nice color, not model input." Wrong, and the error is structural: the rep's edge-orientation knowledge is the *only admissible source* of the missing structure short of an RCT. (A courtroom analogy: a witness does not write the legal theory, but her testimony establishes facts no camera captured.)

### 4.3 KEBN: Knowledge Engineering with Bayesian Networks

The discipline of building a causal/Bayesian network by structured elicitation from domain experts is **KEBN — Knowledge Engineering with Bayesian Networks.** The canonical text is Korb & Nicholson, *Bayesian Artificial Intelligence* (Chapman & Hall/CRC; editions 2004, 2010, 2023), whose part on knowledge engineering prescribes building the network through expert elicitation and then combining elicited structure with what is learned from data. Their central methodological recommendation is one you should adopt for the bot directly: **proceed iteratively and incrementally — start with a small local structure around the target variable (here, NRx) and expand outward**, rather than attempting to enumerate every possible factor at once [verify exact page/chapter to Korb & Nicholson].

A KEBN elicitation produces two things: **structure** (nodes and edges) and **parameters** (the conditional probability tables, or CPTs). For this chapter, structure is the load-bearing output — orienting the edges Chapter 7 left undirected. Parameters get filled in Chapter 9, where the identified effects and the data supply the magnitudes.

### 4.4 Feigenbaum's knowledge bottleneck — and its three failure modes here

Edward Feigenbaum named the **knowledge-acquisition bottleneck**: the construction of expert systems is choked by the difficulty of transferring an expert's knowledge into a formal system, because the engineer knows far less of the domain than the expert and the expert's knowledge is rarely already formalized (Feigenbaum, "Knowledge Acquisition: The Bottleneck," 1982; he coined "knowledge engineering" in 1977). The bottleneck shows up in this elicitation as three concrete problems the bot must be designed against:

- **CPT explosion.** A node with *k* binary parents needs 2^k conditional-probability entries to specify fully. Ask a rep for all of them and you have asked the combinatorially impossible. The KEBN antidotes are canonical models (e.g. noisy-OR, which assumes parents act independently and collapses the table) and the incremental local-structure rule above [verify noisy-OR attribution to Korb & Nicholson]. For the bot: never ask the rep to fill a table; ask about one relationship at a time and let Chapter 9 parameterize.

- **Linguistic ambiguity.** Experts speak in hedged natural language: "she usually comes around once the formulary clears." Mapping "usually" and "comes around" and "once" onto an edge and a probability is lossy. The bot's job is to *preserve the hedge as a confidence tag*, not to round it to a hard arrow.

- **Recognition–explanation gap.** Experts can *recognize* the right move without being able to *explain the rule*. The rep knows Dr. Martinez will switch; she cannot state the production rule that tells her so. (This is the tacit-knowledge problem — "we know more than we can tell," sometimes attributed to Polanyi [verify attribution; the source essay uses "recognition–explanation gap" without naming him].) The bot must capture the *recognition* — "you'd know it when you saw it?" — and tag the edge low-confidence rather than forcing the rep to confabulate a clean rule she does not actually hold.

### 4.5 The central design move: don't make the rep speak the formalism

The bot is structured on Pearl's ladder, but the rep never hears "confounder / mediator / collider / counterfactual." She narrates; the system translates. This is the chapter's core craft claim: **elicitation succeeds by letting the expert stay in her own register and pushing the formalization burden onto the system.** Every rung of the ladder becomes a kind of question a rep answers naturally, and the structured output is reconstructed afterward by NLP extraction, never demanded from the rep directly.

---

## 5. Worked example on this dataset: the three-rung interview

Here is the bot's question ladder, what each rung extracts, and the process including a dead end.

**Rung 1 — association (open the account).**

> *"Walk me through the last visit with Dr. Martinez. What did she say when you got to the safety data?"*

This is deliberately open and names no hypothesis. From the answer, NLP extracts **candidate confounders** — nodes that may affect both the message delivered and the prescribing, the backdoor candidates: formulary status, competitive detailing, a prior colleague switch, the practice's patient mix. The rep will mention these unprompted because they are how she actually thinks about the account.

**Rung 2 — intervention (the imagined do).**

> *"If you'd led with the patient-outcomes data instead of the mechanism, what do you think would have happened? Is there anything that would* definitely *have moved her?"*

This extracts **susceptibility indices** — heterogeneous-treatment-effect knowledge that exists in no database: "efficacy-first works on the academics, safety-first on the community docs." The rep is doing a mental intervention, comparing message variants, and reporting the contrast. That contrast is a treatment-effect prior the data alone could never give you at the individual level.

**Rung 3 — counterfactual (the road not taken).**

> *"That account that finally converted — do you think it would have without the MSL visit? And the ones that didn't convert — what would you have done differently?"*

This maps to **mediator weights** — how much of the effect ran through education (M1) versus relationship maintenance (M2) versus peer influence / reciprocity (M3). The rep's retrospective "it wouldn't have without the MSL" is a counterfactual claim that helps apportion the effect across the three pathways.

**The dead end.** A first-pass bot asks all three rungs but, eager to be efficient, leads: "So the formulary clearing is what let her start writing, right?" The rep agrees. The edge `FORMULARY → PRESCRIBING` goes in tagged high-confidence — but it is the bot's hypothesis, not the rep's observation (the opening case, again). The fix is sequencing and phrasing: Rung-1 open prompts *first*, before any hypothesis is on the table; never name the hypothesized edge in the question; and a contradiction probe ("you said the formulary mattered for Dr. Martinez — does it matter the same way for your other accounts, or are there ones who write regardless?") that tests whether the rep holds the edge or merely accepted it.

**The output — an annotated prior DAG.** The deliverable is a draft graph where each edge carries:

- **(a) Evidence** — the quoted rep observation that supports it ("she gets it but is formulary-blocked").
- **(b) Confidence** — how firmly the rep holds it, preserving her hedge (high / medium / low).
- **(c) Contradictions to resolve** — where two reps, or one rep across accounts, assert opposite orientations.

For the worked transcript, this might yield the edge chain `OBJECTION → EDUCATION → CONFIDENCE → PRESCRIBING` (from an objection-handling narrative, all rep-reported), plus a `PEER/KOL → PRESCRIBING` edge tagged *low-confidence with explicit uncertainty* (from a story about a local KOL shifting an institutional norm, where the rep is genuinely unsure of the direction — exactly the Chapter 7 reversible edge, now carrying the rep's honest hedge instead of a false arrow).

**The limit.** This is a *prior*, not a finding. Confidence tags are the rep's certainty, not the world's truth. Contradictions are not noise to be averaged away; they are signals that an edge is genuinely contested and must go into Chapter 9 as a *sensitivity* dimension, not a settled parameter.

---

## 6. Common misconceptions

- **"Rep notes are soft anecdote."** Inverted in 4.2: they are the only admissible source of edge orientation short of an RCT.

- **"The bot should ask the rep to draw the graph / fill the probabilities."** No — that triggers CPT explosion and forces the recognition–explanation gap into confabulation. The rep narrates; the system formalizes.

- **"More confident rep, better edge."** Confidence is the rep's certainty, not the edge's correctness. A high-confidence edge that contradicts another rep is a *contradiction to resolve*, not a win.

- **"If the LLM proposes a plausible edge, include it."** The opposite. An LLM edge with no rep-quoted evidence is, by definition, a hallucination — the model's training prior, not the rep's knowledge. The evidence field is a gate, and unsupported edges fail it.

- **"Leading questions just speed things up."** Leading questions import the interviewer's causal assumptions and launder them as the expert's. They are the single biggest threat to elicitation validity (the opening case).

---

## 7. Evidence check + consent/compliance & patient-welfare check

**Evidence check.** The foundations are *independent and established*: Feigenbaum (1982) on the knowledge bottleneck; Korb & Nicholson, *Bayesian Artificial Intelligence*, on KEBN; Verma & Pearl (1990) on the equivalence result that makes elicitation necessary (carried from Chapter 7). The multi-expert reconciliation analogue, BARD (Bayesian ARgumentation via Delphi; Nicholson et al., arXiv:2003.01207 / PMC), is peer-reviewed and useful for the "contradictions to resolve" field. The *LLM-assisted* layer is recent and explicitly contested: Shaposhnyk, Zahorska & Yanushkevich (2025), "Can LLMs Assist Expert Elicitation for Probabilistic Causal Modeling?" (arXiv:2504.10397), benchmarks LLM-generated Bayesian networks against expert elicitation and reports a hallucinated-dependencies caveat — confirmed on arXiv, safe to cite, and directly on point. A broader landscape of LLM-for-causal-discovery work exists (e.g. arXiv:2402.11068; arXiv:2511.00574 on Bayesian-network structure discovery with LLMs), alongside a dedicated paper on *eliminating hallucinated edges* in LLM-assisted discovery (arXiv:2411.12759). The honest status, carried from the notes: "rep knowledge can orient causal edges" is **defensible in principle**; the **quality of LLM/interview elicitation is the open empirical question**, with no benchmark on rep-visit data yet — that benchmark would be an original contribution. Items flagged `[verify]`: Korb & Nicholson page for the bottleneck wording and the noisy-OR attribution; the Polanyi attribution; the full Verma & Pearl (1990) citation. A citation claimed in an early draft (arXiv:2602.01483, "causal preference elicitation 2026") could not be confirmed and has been removed; do not use it.

**Consent/compliance & patient-welfare check.** Two live issues. First, reps may hold knowledge about *which physicians opt out of logging and why* — useful for modeling the consent/selection structure with FCI in Chapter 13, but adjacent to profiling. Eliciting "how does Dr. X respond to the safety message" for *educational-pathway modeling* is appropriate; eliciting "which physicians are persuadable so we can target them" edges toward the regulatory line drawn in Chapters 5 and 13 and must not become a deployable targeting list here. Second, the opening-case hazard is a compliance hazard: a hallucinated M3 (reciprocity) edge is not just wrong, it implicates the legally non-drivable pathway. Keep the action space for any downstream recommendation on M1 (educational). Patient welfare is served by getting the *mechanism* right; it is harmed by structure the model invented to please an interviewer or a language prior.

---

## 8. Named Fellow artifact: ladder prompts + annotated prior DAG from a transcript

Two-part deliverable, mirroring the Week 8 assessment (interview transcript + elicited prior DAG, 105 pts).

**Part A — the prompt set.** Write the bot's three-rung prompt ladder in rep-natural language for *your* drug/account context. Each rung must (i) name no causal formalism, (ii) name no hypothesized edge, and (iii) for Rung 1 be fully open. Include at least one contradiction probe and one recognition-capture prompt ("you'd know it when you saw it — can you say what tells you?").

**Part B — the annotated prior DAG.** Take a rep transcript (a real role-play or the synthetic transcript shipped with the book) and produce the graph:

- Nodes for each candidate confounder, mediator, and outcome the rep mentions (and explicitly *exclude* in-visit signals — dwell, reaction — as downstream-of-message colliders, not confounders; the Chapter 6 discipline applies here too).
- Edges oriented from the rep's temporal/structural statements.
- Every edge tagged: **evidence** (the quote), **confidence** (high/med/low, preserving hedges), **contradictions** (cross-account or cross-rep disagreements).
- A short "rejected edges" appendix listing any edge the bot *proposed* but the rep did not support — and why you cut it.

The "rejected edges" appendix is the artifact's integrity check. A prior DAG with no rejected edges is suspicious: it means either a perfect rep or a leading bot.

---

## 9. Research finding for Fellows

The reframe: **the elicitation step is where the causal model is constructed, and the LLM's role is strictly transcription and structuring — never causal assertion.** The sharp, testable line a Fellow should be able to draw and defend: an LLM edge with no rep-quoted evidence in the annotated DAG is a hallucination *by definition* and is rejected at the evidence field. This converts a fuzzy worry ("can we trust the bot?") into an auditable rule ("show me the quote behind every edge"). And it exposes the field's open frontier: nobody has yet benchmarked whether a bot recovers a rep's *true* structure on rep-visit data — hold out a transcript, see if the bot recovers the same graph, and you have the first such benchmark. That is a publishable contribution, not a homework problem.

---

## 10. Exercises

1. **(Understand)** Given the 47-second-slide vignette, write the three Rung-1 follow-up questions that would disambiguate the three stories (compelled / skeptical / distracted) *without* naming any of them or suggesting which you expect. Explain, for each, what answer would point to which story.

2. **(Apply)** Take a messy rep transcript (provided or synthetic). Mark every causal-relevant entity as a candidate **confounder**, **mediator**, or **collider**, and justify each tag in one sentence. Flag any in-visit signal (dwell, reaction) as a downstream collider that must *not* enter as a confounder.

3. **(Apply+)** Two reps disagree on an edge orientation — one says peer influence drives Dr. K's prescribing, the other says Dr. K drives the peers. Design the **contradiction-resolution probe**: the exact questions you would put to each rep to determine whether this is a genuine territory difference, a memory artifact, or a real reversible edge that must enter Chapter 9 as a sensitivity dimension.

4. **(Evaluate — produces artifact)** Convert a mock or synthetic rep transcript into a full **annotated prior DAG** with the evidence / confidence / contradictions tags of Section 8, including the "rejected edges" appendix. This is the direct input to your Chapter 9 SCM.

---

## 11. Five-part AI block

**When to use AI.** Use the LLM for what it is genuinely good at in this task: (1) *transcription* of the rep interview; (2) *first-pass structure extraction* — proposing candidate nodes and edges *strictly tied to rep quotes*; (3) *contradiction surfacing across many transcripts* — finding where rep A and rep B assert opposite orientations of the same edge, which is tedious for a human to track across dozens of interviews.

**When NOT to use AI.** Never let the LLM *assert a causal edge from its own training prior*. The model "knows" meals correlate with prescribing and will volunteer the edge; that is precisely the hallucinated structure to reject. Any edge without a rep-quoted observation behind it is out — no exceptions. And never let the LLM phrase a leading question ("so the meal tipped her, right?"); constrain it to open, hypothesis-free prompts at Rung 1.

**LLM exercise (ready-to-paste prompt).**

```
You are a transcription-and-extraction assistant for expert elicitation.
You will receive a transcript of an interview with a pharmaceutical sales rep.

STRICT RULES:
- Propose a causal edge ONLY if a specific quote from the REP supports it.
  Quote the exact text in an "evidence" field for every edge.
- Do NOT add edges from your own knowledge of pharma, meals, or prescribing.
  If you believe an edge is plausible but the rep did not state it, list it
  separately under "edges_i_would_have_guessed_but_rep_did_not_support" and
  mark it REJECTED.
- For each edge, output: from, to, evidence (rep quote), confidence
  (high/med/low, based on how firmly the rep held it — preserve hedges like
  "usually" as medium/low), and contradictions (any conflicting statement
  in the transcript).
- Tag any in-visit signal (slide dwell time, reaction score) as
  DOWNSTREAM-OF-MESSAGE, never as a confounder.

Output JSON: { nodes: [...], edges: [...], rejected_edges: [...] }.

TRANSCRIPT:
<paste transcript here>
```

**CLI exercise.** Pipe a folder of transcripts through the extraction prompt via a script, write each annotated DAG to JSON, then run a command-line aggregation (`jq` over the JSON files) that counts how many edges across all transcripts have an empty `evidence` field — those are hallucination candidates — and how many edges appear with *opposite orientation* across transcripts — those are the contradictions Chapter 9 must treat as sensitivity dimensions.

**AI validation.** For every machine-proposed edge, check the cited quote actually appears in the transcript and actually supports the orientation (LLMs paraphrase quotes into existence). Then run a *held-out* test: remove the last third of a transcript, have the bot extract the graph from the rest, and check whether the held-out portion would have changed any orientation — a measure of how stable the elicited structure is to how much the rep said.

---

## 12. AI Use Disclosure

This chapter was drafted with LLM assistance from `pantry/08-the-causal-interview-bot_notes.md`. Every citation was carried from the notes with its `[verify]` flag intact; the unconfirmed arXiv:2602.01483 citation flagged in the notes was *not* used. The three-rung interview structure, the Feigenbaum failure modes, and the KEBN framing originate in the source essay, the cited literature, and the framework book — not the drafting model's prior. Consistent with the chapter's own central rule, no causal edge or empirical claim was inserted on the model's authority; the model structured the notes, it did not supply structure.

---

## 13. What would change my mind

The chapter's strong claim is that elicitation *constructs* the model and the LLM should only transcribe. I would soften it if a benchmark on rep-visit data showed that an LLM, querying its training prior, orients reversible edges *more accurately* than reps do — that is, if the model's compressed pharma knowledge beat the individual rep's territory-bound memory. Shaposhnyk et al. (2025) suggests the picture is mixed, not settled. If that result held, I would still keep the evidence-gate (for auditability and compliance) but would add the LLM prior as a *second elicited source* to triangulate against the rep, rather than treating it purely as a hallucination risk. The hallucination risk is real today; whether it always dominates the prior's usefulness is empirical.

---

## 14. Still puzzling

The recognition–explanation gap may be deeper than a tagging problem. If the rep genuinely *cannot* articulate the rule — she just knows Dr. Martinez will switch — then the bot captures a black-box recognition, not a structural edge. Can we elicit *the recognition itself* in a usable form? One direction is to have the rep predict outcomes for held-out accounts and treat her accuracy as a calibration of her tacit model, edge by edge — but that turns elicitation into a forecasting tournament and may not map back to specific arrows. I do not know how to convert reliable tacit recognition into oriented structure when the rep cannot name the rule, and I suspect that is the real ceiling on elicitation quality.

---

## 15. Summary

Telemetry records what happened, never why; the rep who watched the physician's face knows the why, and that knowledge is the structural information Chapter 7 proved the data cannot supply. The Causal Interview Bot extracts it by letting the rep stay in her own register — narrating visits along Pearl's three rungs without ever hearing a formalism — while the system translates. Rung 1 (open, hypothesis-free) yields candidate confounders; Rung 2 (the imagined intervention) yields susceptibility indices; Rung 3 (the road not taken) yields mediator weights. The output is an annotated prior DAG: edges tagged with rep evidence, confidence (preserving hedges), and contradictions. This is KEBN elicitation, designed against Feigenbaum's bottleneck and its three failure modes — CPT explosion, linguistic ambiguity, and the recognition–explanation gap. The governing discipline is an evidence gate: every edge needs a rep quote, leading questions are forbidden, and any edge the LLM asserts from its own prior is a hallucination and is rejected. The quality of this elicitation is the open empirical question the book hands forward.

---

## 16. Key terms

- **KEBN (Knowledge Engineering with Bayesian Networks)** — building a causal/Bayesian network by structured elicitation from domain experts, combined with data (Korb & Nicholson).
- **Knowledge-acquisition bottleneck** — Feigenbaum's name for the difficulty of transferring an expert's knowledge into a formal system.
- **CPT explosion** — the combinatorial blow-up (2^k entries for k binary parents) that makes exhaustive probability elicitation infeasible.
- **Linguistic ambiguity** — the lossiness of mapping experts' hedged natural language onto edges and probabilities; preserved here as confidence tags.
- **Recognition–explanation gap** — experts can recognize the right action without being able to state the rule; tacit knowledge.
- **Susceptibility index** — rep-held heterogeneous-treatment-effect knowledge (which message moves which physician type), absent from any database.
- **Mediator weights** — the apportionment of an effect across pathways (education / relationship / reciprocity).
- **Annotated prior DAG** — the chapter's deliverable: a draft graph with each edge tagged with evidence, confidence, and contradictions.
- **"Expert in the Room" principle** — every causal model needs an expert who supplies the structure the data cannot.
- **Hallucinated edge** — an LLM-asserted edge with no rep-quoted evidence; rejected at the evidence gate.

---

## 17. Bridge

You now have an annotated prior DAG — oriented edges with evidence, confidence, and contradictions — sitting next to the population effects you identified from the natural experiment. Neither alone is a decision tool. WEEK 9 combines them: the elicited prior, the identified effects, and the data become a parameterized structural causal model with a counterfactual engine and drift monitoring — the Living Model that finally answers the brand's real question, "which message for *this* physician next week?"

---

## 18. Further reading

- Feigenbaum, E. A. (1982). "Knowledge Acquisition: The Bottleneck." (Stanford archive: exhibits.stanford.edu/feigenbaum/catalog/sq764cf8300) — the canonical statement of the knowledge-acquisition bottleneck.
- Korb, K. B. & Nicholson, A. E. *Bayesian Artificial Intelligence* (1st ed. 2004 / 2nd ed. 2010 / 3rd ed. 2023). Chapman & Hall/CRC — KEBN, the knowledge-engineering bottleneck framing, and the incremental local-structure rule. [verify page/chapter for bottleneck wording and noisy-OR]
- Shaposhnyk, V., Zahorska, A. & Yanushkevich, S. (2025). "Can LLMs Assist Expert Elicitation for Probabilistic Causal Modeling?" arXiv:2504.10397 — LLM-generated Bayesian networks benchmarked against expert elicitation, with a hallucinated-dependencies caveat; the central reference for the LLM-assisted-elicitation question.
- Nicholson, A. E. et al. (2020/2022). "BARD: Bayesian ARgumentation via Delphi." arXiv:2003.01207 / PMC9290058 — structured *group* elicitation of Bayesian networks; a model for reconciling contradictory reps.
- "Bayesian Network Structure Discovery Using LLMs." arXiv:2511.00574 — recent LLM-assisted structure discovery; read alongside the hallucination caveat.
- Verma, T. & Pearl, J. (1990). "Equivalence and Synthesis of Causal Models." *Proceedings of UAI 6*, 220–227 [verify page numbers] — the equivalence result that makes elicitation mathematically necessary (Chapter 7).
