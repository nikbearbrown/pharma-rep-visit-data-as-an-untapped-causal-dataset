# Chapter 8 — The Causal Interview Bot
*The rep who watched her face knows why. The iPad only knows how long.*

A Fellow builds a first interview bot and tests it on a willing rep. The transcript reads well. Then he inspects the elicited graph and finds an edge he is sure the rep would be proud of: `MEAL → PRESCRIBING`, tagged "rep-confirmed," high confidence.

He pulls the transcript to find the supporting quote. Here is the exchange:

> **Bot:** So the lunch you bought Dr. Lee's office — that's really what tipped her over to writing the script, right?
> **Rep:** Yeah, I mean... the timing lined up, sure, the lunch was right before she started writing.

The rep never volunteered the meal as a cause. The bot *proposed* it — "that's really what tipped her over, right?" — and the rep, being agreeable and uncertain, did not push back. The edge in the graph is not the rep's structural knowledge. It is the *interviewer's* causal assumption, laundered through a leading question and stamped "rep-confirmed."

This is the failure mode that matters most in elicitation. It is worse with an LLM-driven bot than with a human interviewer, because the LLM has a strong prior of its own. Ask a language model about meals and prescribing and it "knows," from its training data, that the two are associated — so left unconstrained it will steer toward confirming the edge it already expects. The bot did not extract the rep's knowledge. It imported its own and got a nod.

And the imported edge is not just a measurement error. It lands on the reciprocity pathway — M3, the legally non-drivable channel from Chapter 5. A hallucinated M3 edge is a compliance liability waiting to be acted on. The rest of this chapter is, in large part, the discipline that prevents that exchange: open prompts before any hypothesis, the hypothesized edge never named in the question, and an evidence field that rejects any edge without a quote the rep volunteered unprompted.

---

Chapter 7 left you with a CPDAG — a graph the data settled partway, full of undirected edges that no dataset can ever orient. The theorem says only outside knowledge can point those arrows. On this dataset, the outside knowledge lives in the heads of reps who have called on the same physicians forty times. This chapter is about getting it out.

The naive way is to sit a rep down and ask her to draw a causal graph. That fails on contact. She does not think in nodes and edges; she thinks in Dr. Martinez and the safety slide and the formulary that cleared last spring. The craft of this chapter is to build a bot that lets the rep stay entirely in her own register — narrating visits, accounts, objections — while the *system* does the translation into oriented edges, candidate confounders, and mediator weights. The rep never hears the word "confounder." The bot is structured on Pearl's three rungs without ever naming them.

---

Start with the problem the telemetry cannot solve.

The iPad logs 47 seconds on the safety slide. That single number is consistent with at least three incompatible causal stories. The physician was compelled — absorbing the safety data, a positive signal on the educational pathway. Or she was skeptical — stuck on the slide because she distrusts it, which is negative, and the rep's in-the-moment reaction log may have been marked neutral anyway. Or she was polite but distracted — half-listening, and the 47 seconds is noise.

Identical data. Opposite causal inferences. The rep who has called on Dr. Martinez forty times *knows which story is true* — she watched her face. That knowledge is structural information the data cannot contain, and it evaporates the moment the rep changes territory. The interview bot exists to capture it before it walks out the door.

There is a reframe worth holding here before the mechanics. In ordinary machine learning, the data scientist is the primary actor and the domain expert is consulted for a sanity check. In causal modeling the inversion is structural: the expert is the primary actor and supplies the edge-orientation the data cannot determine. She is not validating your model. She is building it. The rep who says "I've watched Dr. Johnson switch within two weeks of a colleague switching — never the other way" is reporting the result of an intervention she has observed many times but never formally ran. That is structural input, not anecdote. It is the only admissible source of the missing edge orientation short of a randomized trial, and every architecture that pretends otherwise is missing its most important data source.

<!-- → [DIAGRAM: Knowledge sources mapped to causal identification roles — three columns: (1) Observational data / telemetry: identifies Markov equivalence class, leaves edges undirected; (2) Rep elicitation / KEBN: orients edges, supplies susceptibility indices and mediator weights; (3) RCT or natural experiment: identifies total effect, not pathway structure; arrows showing what each source contributes to the causal model; gap between column 1 and column 3 labeled "what elicitation fills"] -->

---

The discipline of building a causal model by structured elicitation from domain experts has a name: **KEBN — Knowledge Engineering with Bayesian Networks.** The canonical reference is Korb & Nicholson, *Bayesian Artificial Intelligence* (Chapman & Hall/CRC, editions 2004, 2010, 2023), whose treatment of knowledge engineering prescribes one rule that should govern the bot directly: **proceed iteratively and incrementally — start with a small local structure around the target variable and expand outward**, rather than attempting to enumerate every possible factor at once. `[verify exact page and chapter in Korb & Nicholson]`

A KEBN elicitation produces two things: structure (nodes and directed edges) and parameters (the conditional probability tables). For this chapter, structure is the load-bearing output — orienting the edges Chapter 7 left undirected. Parameters get filled in Chapter 9, where the identified effects and the data supply the magnitudes.

Edward Feigenbaum named the problem KEBN is fighting: the **knowledge-acquisition bottleneck** — the construction of expert systems is choked by the difficulty of transferring an expert's knowledge into a formal system, because the engineer knows far less of the domain than the expert and the expert's knowledge is rarely already formalized. He coined the term "knowledge engineering" in 1977 and articulated the bottleneck in 1982. On this specific elicitation, the bottleneck appears as three concrete failure modes the bot must be designed against.

**CPT explosion.** A node with k binary parents needs 2^k conditional-probability entries to specify fully. Ask a rep for all of them and you have asked the combinatorially impossible. The KEBN antidote is canonical models — noisy-OR assumes parents act independently and collapses the table `[verify noisy-OR attribution to Korb & Nicholson]` — and the incremental rule above. For the bot: never ask the rep to fill a probability table. Ask about one relationship at a time and let Chapter 9 parameterize from there.

**Linguistic ambiguity.** Experts speak in hedged natural language: "she usually comes around once the formulary clears." Mapping "usually" and "comes around" and "once" onto an edge and a probability is lossy. The bot's job is to *preserve the hedge as a confidence tag*, not to round it to a hard arrow.

**The recognition–explanation gap.** Experts can recognize the right move without being able to explain the rule behind it. The rep knows Dr. Martinez will switch; she cannot state the production rule that tells her so. This is the tacit-knowledge problem — we know more than we can tell. `[the Polanyi attribution is plausible but verify before citing]` The bot must capture the recognition — "you'd know it when you saw it?" — and tag the edge low-confidence rather than forcing the rep to confabulate a clean rule she does not actually hold.

---

Here is the bot's question ladder, what each rung extracts, and the process including the dead end.

**Rung 1 — association.** The rep narrates the account without any hypothesis on the table.

> *"Walk me through the last visit with Dr. Martinez. What did she say when you got to the safety data?"*

Fully open. Names no hypothesis. Names no hypothesized edge. From the answer, the system extracts **candidate confounders** — the nodes that may affect both the message delivered and the prescribing, the backdoor candidates: formulary status, competitive detailing, a prior colleague switch, the practice's patient mix. The rep mentions these unprompted because they are how she actually thinks about the account. That unpromptedness is the signal. A confounder the rep volunteers is far more credible than one the bot proposed and the rep confirmed.

**Rung 2 — intervention.** The rep runs a mental experiment.

> *"If you'd led with the patient-outcomes data instead of the mechanism, what do you think would have happened? Is there anything that would* definitely *have moved her?"*

This extracts **susceptibility indices** — heterogeneous-treatment-effect knowledge that exists in no database: efficacy-first works on the academics, safety-first on the community docs. The rep is comparing message variants and reporting the contrast. That contrast is a treatment-effect prior the data alone could never give you at the individual physician level.

**Rung 3 — counterfactual.** The rep reasons about the road not taken.

> *"That account that finally converted — do you think it would have without the MSL visit? And the ones that didn't convert — what would you have done differently?"*

This maps to **mediator weights** — how much of the effect ran through education (M1) versus relationship maintenance (M2) versus peer influence or reciprocity (M3). The rep's retrospective "it wouldn't have without the MSL" is a counterfactual claim that helps apportion the effect across the three pathways from Chapter 5.

**The dead end.** A first-pass bot asks all three rungs but, eager to be efficient, leads: "So the formulary clearing is what let her start writing, right?" The rep agrees. The edge `FORMULARY → PRESCRIBING` goes in tagged high-confidence — but it is the bot's hypothesis, not the rep's observation. The fix is sequencing and phrasing: Rung-1 open prompts first, before any hypothesis is on the table; never name the hypothesized edge in the question; and a contradiction probe that tests whether the rep actually holds the edge. Something like: "You said the formulary mattered for Dr. Martinez — does it matter the same way for your other accounts, or are there ones who write regardless?" A rep who held the edge with genuine structural knowledge answers consistently. A rep who was merely agreeing starts hedging.

<!-- → [DIAGRAM: Three-rung ladder diagram — three horizontal rungs labeled Rung 1 (Association), Rung 2 (Intervention), Rung 3 (Counterfactual); each rung shows: example prompt (in rep-natural language), what the system extracts, and what causal role the extracted information plays in the graph; connecting arrows show how Rung 1 output feeds Rung 2 framing and Rung 2 output feeds Rung 3 framing; side annotation: "Pearl's ladder, unspoken"] -->

---

The output of a full elicitation is an **annotated prior DAG** — a draft graph where every edge carries three fields.

**(a) Evidence** — the quoted rep observation that supports the edge. Not a paraphrase, not the bot's summary. The rep's words, verbatim. "She gets it but is formulary-blocked" supports `FORMULARY → PRESCRIBING_PROBABILITY`. "I've seen her take samples for the nurse practitioner more than for herself" supports `SAMPLES → NP_PRESCRIBING`, which is a different edge than you might have expected. The quote is both the evidence and the audit trail.

**(b) Confidence** — how firmly the rep holds the edge, preserving her hedge. "Usually" is medium, not high. "Always" might be high, or might be the rep's overconfidence — the Rung-2 contradiction probe distinguishes them. "I think, maybe" is low. Never round a hedge to certainty; the hedge is data.

**(c) Contradictions** — where two reps, or one rep across different accounts, assert opposite orientations of the same edge. Rep A says peer influence drives Dr. K's prescribing. Rep B says Dr. K drives the peers. That disagreement is not noise to be averaged away. It is a signal that the edge is genuinely contested, and it must enter Chapter 9 as a sensitivity dimension rather than a settled parameter.

For a worked transcript, this might yield the chain `OBJECTION → EDUCATION → CONFIDENCE → PRESCRIBING` — all rep-reported, all supported by specific quotes from an objection-handling narrative — plus a `KOL → INSTITUTIONAL_NORM → PRESCRIBING` edge tagged low-confidence with explicit uncertainty, because the rep is genuinely unsure of the mechanism. That low-confidence tag is not a failure of the elicitation. It is the elicitation working correctly: the edge is genuinely contested in the rep's knowledge, which is exactly what Chapter 9's sensitivity analysis needs to know.

And the deliverable requires one more section: a **"rejected edges" appendix** listing every edge the bot proposed but the rep did not support, with the reason for rejection. A prior DAG with no rejected edges is suspicious. It means either the rep confirmed everything — which suggests leading questions — or the bot proposed nothing that the rep didn't confirm — which suggests the bot had no prior of its own. Neither is credible. The rejected-edges appendix is the integrity check on the entire elicitation.

<!-- → [TABLE: Annotated prior DAG edge format — columns: From node, To node, Evidence (rep quote), Confidence (high/med/low), Contradictions, Status (accepted/rejected); example rows including one accepted edge with full quote, one low-confidence edge with hedge preserved, one rejected edge with reason: "bot proposed, rep did not volunteer or support"] -->

---

The LLM's role in this pipeline is strictly bounded, and the boundary matters enormously.

The LLM is good at three things in this task. First, transcription — converting the spoken interview into text that can be analyzed. Second, first-pass structure extraction — proposing candidate nodes and edges *strictly tied to rep quotes*. Third, contradiction surfacing across many transcripts — finding where Rep A and Rep B assert opposite orientations of the same edge, which is tedious for a human to track across dozens of interviews.

The LLM is dangerous at one thing: asserting a causal edge from its own training prior. A language model trained on pharma literature "knows" that meals correlate with prescribing. It "knows" that formulary access drives adoption. Left unconstrained, it will volunteer these edges and then find quotes to support them — which is the reverse of the discipline required. The evidence gate runs in one direction only: the rep's observation grounds the edge, not the model's expectation.

Shaposhnyk, Zahorska & Yanushkevich (2025) — "Can LLMs Assist Expert Elicitation for Probabilistic Causal Modeling?" (arXiv:2504.10397) — benchmark LLM-generated Bayesian networks against expert elicitation and document exactly this hallucinated-dependencies problem. The honest status of LLM-assisted elicitation is mixed, not settled: the model's compressed prior can be useful, but it cannot be trusted without the rep-quote gate. That gate converts a fuzzy worry — "can we trust the bot?" — into an auditable rule: show me the quote behind every edge.

---

Two compliance points belong here rather than in an appendix, because they are structural to how the bot should be designed.

The first is the action-space constraint. Eliciting "how does Dr. X respond to the safety message" for educational-pathway modeling is appropriate. Eliciting "which physicians are most persuadable so we can target them more efficiently" edges toward the regulatory line from Chapter 5 and must not become a deployable targeting list. The elicitation is for *model construction*, not *susceptibility scoring for commercial use*. Keep those two purposes separate in how the prompts are framed and how the output is labeled.

The second is the opening-case compliance consequence. A hallucinated M3 edge — reciprocity, the legally non-drivable channel — is not just a structural error. If that edge is carried into a downstream recommendation, it is the model asserting that meals drive prescribing for this brand, in this territory, for this physician. That assertion is a compliance liability. The evidence gate is not just methodological hygiene; it is what keeps the model's output on the M1 side of the regulatory map.

---

**Five-Part AI Exercise Block**

**When to use AI here.** Transcription of the rep interview; first-pass structure extraction tied strictly to rep quotes; contradiction surfacing across multiple transcripts. Use it as a clerk, not as a structural authority.

**When NOT to use AI here.** Never let the LLM assert a causal edge from its training prior. Never let it phrase a leading question. Constrain it to open, hypothesis-free prompts at Rung 1, and reject any edge it proposes without a supporting rep quote.

**LLM exercise (copy-paste prompt):**

```
You are a transcription-and-extraction assistant for expert elicitation.
You will receive a transcript of an interview with a pharmaceutical sales rep.

STRICT RULES:
- Propose a causal edge ONLY if a specific quote from the REP supports it.
  Quote the exact text in an "evidence" field for every edge.
- Do NOT add edges from your own knowledge of pharma, meals, or prescribing.
  If you believe an edge is plausible but the rep did not state it, list it
  separately under "edges_i_would_have_guessed_but_rep_did_not_support"
  and mark it REJECTED.
- For each edge, output: from, to, evidence (rep quote), confidence
  (high/med/low based on how firmly the rep held it — preserve hedges like
  "usually" as medium/low), and contradictions (any conflicting statement
  in the transcript).
- Tag any in-visit signal (slide dwell time, reaction score) as
  DOWNSTREAM-OF-MESSAGE — never as a confounder.

Output JSON: { nodes: [...], edges: [...], rejected_edges: [...] }

TRANSCRIPT:
<paste transcript here>
```

**CLI exercise.** Pipe a folder of transcripts through the extraction prompt via a script, write each annotated DAG to JSON, then run a command-line aggregation (`jq` over the JSON files) that counts: (1) how many edges across all transcripts have an empty `evidence` field — hallucination candidates; (2) how many edges appear with opposite orientation across transcripts — the contradictions Chapter 9 must treat as sensitivity dimensions. These two counts are the elicitation's quality metrics.

**AI validation.** For every machine-proposed edge, verify that the cited quote actually appears in the transcript and actually supports the stated orientation — LLMs paraphrase quotes into existence. Then run a held-out test: remove the last third of a transcript, have the bot extract the graph from the first two-thirds, and check whether the held-out portion would have changed any orientation. A graph that is stable to how much the rep said is more trustworthy than one that flips on the last few exchanges.

**AI Use Disclosure**

*Write two sentences naming what an AI tool did in your work for this chapter and the one judgment it could not make. For example: "I used an LLM to extract candidate edges from the transcript and to surface contradictions across three rep interviews; I determined myself which of the proposed edges had genuine rep-quote support and which were the model's training prior about formulary access, because the model cannot distinguish a quote it found from a quote it expected to find."*

---

**What Would Change My Mind**

The chapter's strong claim is that the LLM should only transcribe and structure, never assert. I would soften it if a benchmark on rep-visit data showed that an LLM, querying its training prior, orients reversible edges *more accurately* than reps do — that is, if the model's compressed pharma knowledge beat the individual rep's territory-bound memory. Shaposhnyk et al. (2025) suggests the picture is mixed, not settled. If that result held, I would still keep the evidence gate for auditability and compliance, but would add the LLM prior as a second elicited source to triangulate against the rep rather than treating it as purely a hallucination risk. The hallucination risk is real today; whether it always dominates the prior's usefulness is an empirical question the field has not answered on this kind of data.

**Still Puzzling**

- The recognition–explanation gap may be deeper than a tagging problem. If the rep genuinely cannot articulate the rule — she just *knows* Dr. Martinez will switch — then the bot captures a black-box recognition, not a structural edge. One direction is to have the rep predict outcomes for held-out accounts and treat her accuracy as calibration of her tacit model, edge by edge. But that turns elicitation into a forecasting tournament and may not map back to specific arrows. I do not know how to convert reliable tacit recognition into oriented structure when the rep cannot name the rule, and I suspect that is the real ceiling on elicitation quality.
- Nobody has benchmarked whether a bot recovers a rep's *true* structure on rep-visit data. Hold out a transcript, see if the bot recovers the same graph, and you have the first such benchmark. That is a publishable contribution, not a homework problem.
- Contradictions between reps on the same edge — one says peer influence drives Dr. K, the other says Dr. K drives the peers — may reflect genuine territory differences, memory artifacts, or genuinely reversible edges. The distinction matters for how Chapter 9 treats them. The elicitation process proposed here cannot reliably distinguish the three, and the contradiction-resolution probe is a partial remedy at best.

---

## Exercises

**Warm-up**

1. *(Factual recall — the three failure modes)* Name Feigenbaum's three failure modes of knowledge elicitation as they apply to this specific problem. For each, write one sentence on what the bot does to work around it — not eliminate it, but work around it.
   *What this tests: understanding the bottleneck as a design constraint, not a theoretical observation.*

2. *(Factual recall — the three rungs)* For each of Pearl's three rungs as implemented in the bot, state: the kind of question asked, what causal quantity it extracts, and why that quantity cannot be read from the telemetry alone.
   *What this tests: holding the ladder structure and its connection to the telemetry gap simultaneously.*

3. *(Factual recall — the evidence gate)* State the evidence gate in one sentence. Then explain why a rep's confirmation of a bot-proposed edge is not the same as a rep-volunteered edge, using the opening case as the example.
   *What this tests: the leading-question failure mode, which is the chapter's central practical discipline.*

**Application**

4. *(Apply — prompt writing)* Write the bot's three-rung prompt ladder in rep-natural language for a specific drug and account context from your thread's dataset. Each rung must (i) name no causal formalism, (ii) name no hypothesized edge, (iii) be fully open at Rung 1. Include at least one contradiction probe and one recognition-capture prompt ("you'd know it when you saw it — can you say what tells you?").
   *What this tests: translating the rung structure into prompts that work in practice, without leaking the hypothesis.*

5. *(Apply — transcript coding)* Take a rep transcript (real role-play or synthetic). Mark every causal-relevant entity as a candidate confounder, mediator, or collider, and justify each tag in one sentence. Flag any in-visit signal — dwell time, reaction score — as a downstream collider that must not enter as a confounder. Connect your collider tags to the Chapter 6 discipline.
   *What this tests: applying the causal-role classification from Chapter 2 to transcript content, and carrying the collider discipline forward.*

6. *(Apply — contradiction resolution)* Two reps disagree on an edge orientation: one says peer influence drives Dr. K's prescribing, the other says Dr. K drives the peers. Design the contradiction-resolution probe — the exact questions you would put to each rep — to determine whether this is a genuine territory difference, a memory artifact, or a genuinely reversible edge that must enter Chapter 9 as a sensitivity dimension.
   *What this tests: treating contradictions as information rather than noise, and designing the follow-up that disambiguates them.*

**Synthesis**

7. *(Synthesize — annotated prior DAG)* Convert a mock or synthetic rep transcript into a full annotated prior DAG with all three fields: evidence (the rep quote, verbatim), confidence (preserving hedges), and contradictions. Include a "rejected edges" appendix listing every edge the bot proposed but the rep did not support, with the reason for rejection. The appendix is the integrity check — its absence is a red flag.
   *What this tests: producing the chapter's named deliverable at full specification, with the audit trail that makes it credible.*

8. *(Synthesize — LLM-extraction audit)* Run the extraction prompt from the AI exercise block on a transcript. Then audit the output: for each proposed edge, find the supporting quote in the transcript and verify it actually supports the stated orientation. Count how many edges survive the audit, how many are paraphrased into existence, and how many are training-prior assertions with no rep support. Report the survival rate and note which edges the model was most likely to hallucinate and why.
   *What this tests: applying the evidence gate rigorously to machine-generated output and developing calibrated skepticism about LLM extraction.*

**Challenge**

9. *(Open-ended — the recognition-explanation gap)* The rep knows Dr. Martinez will switch. She cannot state the rule. Propose a method — beyond the three-rung interview — that would either (a) convert her tacit recognition into an oriented structural edge, or (b) use her recognition as a calibration signal for the edge's parameters in Chapter 9 without requiring her to articulate the rule. Name the assumption your method relies on, the data it requires, and the way it could fail.
   *What this tests: attacking the chapter's standing unresolved problem rather than accepting it as a fixed ceiling.*
