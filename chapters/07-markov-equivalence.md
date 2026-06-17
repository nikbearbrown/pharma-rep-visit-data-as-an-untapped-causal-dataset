# Chapter 7: Markov Equivalence — Why the Data Can't Decide

## 1. Overview

By the end of the last two chapters you could draw the message-to-prescribing graph, name its three pathways, and spot the colliders — dwell time, reaction score, consent — that turn a careful analyst into a confidently wrong one. You learned that *conditioning* can break an estimate. This chapter delivers a harder and more disorienting lesson: even when you condition perfectly, even when you have a clean panel and infinite rows, the data alone cannot tell you which way some of the arrows point.

That is not a limitation of your dataset. It is a theorem.

This chapter teaches **Markov equivalence**: the result that observational data identifies a *class* of causal graphs that all fit the data equally well, never a single graph. We will work it on the three-node case until it is obvious, then dress it in a lab coat with the peer-influence puzzle that recurs throughout pharma targeting. The payoff is the load-bearing claim of the whole book: the rep interviews you will design in Chapter 8 are not soft supplementary color. They are the *only admissible source* of the structural information the data is mathematically incapable of supplying. When you finish, you will be able to take a correlation in this dataset, enumerate the causal stories that fit it equally well, and state precisely what evidence — and only what evidence — could orient each edge.

---

## 2. Learning objectives

By the end of this chapter, a Fellow will be able to:

- **(Understand)** Define a Markov-equivalence class and state the Verma–Pearl criterion (same skeleton, same v-structures) for when two DAGs are indistinguishable from observational data.
- **(Understand)** Read a CPDAG (essential graph) as a two-color picture: directed edges the data settled, undirected edges an expert must orient.
- **(Analyze)** Enumerate the Markov-equivalent stories behind a correlation in this dataset — specifically the peer-influence-and-prescribing case — and explain why each fits the data equally well.
- **(Evaluate)** For each reversible edge, state what evidence (temporal order, intervention, or domain knowledge) would orient it, and why rep structural knowledge qualifies.
- **(Evaluate)** Critique a vendor "causal" DAG that displays a single orientation, identifying which displayed arrows are artifacts of an algorithm's tie-breaking rather than facts in the data.

---

## 3. Opening case (failure-first)

A martech vendor pitches your partner a "causal influence engine." The slide is gorgeous: a physician-network graph, NPIs as nodes, arrows between them, a few thick red arrows pointing *into* a cluster of high-prescribing cardiologists labeled "KEY INFLUENCERS — TARGET THESE." The vendor's claim is that their causal-discovery algorithm learned the graph from prescribing and referral data, found who influences whom, and that detailing the influencers will cascade through the network. The recommended budget reallocation is eight figures.

A Fellow on the project asks one question: "How did the algorithm decide the arrow points *from* the influencer *to* the followers, rather than the other way around?"

The room goes quiet, because the honest answer is: it didn't decide. It couldn't. The edge between "exposed to a peer's prescribing" and "prescribes" is — as we are about to prove — *reversible*. The data is equally consistent with "the influencer caused the followers to switch," "the followers are high prescribers who became visible and got labeled influencers," and "both groups share an underlying openness to the drug and there is no influence at all." The algorithm returned one of these because it had to draw *something* on the screen. The thick red arrow is a coin flip wearing a confidence interval.

The vendor was not lying about the math. The discovery algorithm ran correctly. The lie — or the unexamined assumption, which in a procurement deck is the same thing — is that "the algorithm produced a DAG" implies "the DAG is the causal truth." It does not. It produced a *member of an equivalence class*, and hid the rest of the class behind a single rendering. Acting on the displayed arrows means targeting "influencers" who may simply be sure-things who would prescribe anyway — burning budget on people the drug already has (a preview of the persuadables-vs-sure-things mistake we dissect in Chapter 12).

The rest of this chapter is the machinery behind the Fellow's question.

---

## 4. Core sections

### 4.1 What observational data can and cannot see

Start from what a dataset actually contains. A purely observational panel — rows of physicians, columns of measurements, no experiment you ran — reveals exactly one kind of structural fact: **conditional-independence relations**. It tells you that variable A and variable C are correlated, and that the correlation vanishes once you hold B fixed (written A ⊥ C | B), or that it does not. Every pattern, partial correlation, mutual-information estimate, and independence test you can compute is, in the end, a statement about which variables are independent of which others given which third sets.

That is the whole vocabulary the data speaks. And here is the trap: **multiple distinct causal graphs can speak the exact same vocabulary.** When two graphs imply the identical set of conditional-independence relations, no test — no matter how much data you feed it — can prefer one over the other. They are, by definition, **Markov equivalent**.

> **Markov-equivalence class.** The set of all DAGs that encode the same conditional-independence relations over the observed variables. Because observational data reveals only those relations, data can identify a graph *only up to its equivalence class* — never the unique graph.

This is the most important sentence in the chapter. The rest is showing you it is true, and what to do about it.

### 4.2 The three-node theorem (chain, fork, collider)

The entire lesson lives in three variables. Forget pharma for a page and consider A, B, C.

**Chain: A → B → C.** A causes B, B causes C. The independence signature: A and C are marginally dependent (information flows A to C through B), but **A ⊥ C | B** — once you know B, A tells you nothing more about C. The path is blocked by conditioning on the middle.

**Fork (common cause): A ← B → C.** B causes both A and C; A and C have no direct link. The signature: A and C are marginally dependent (they share the common cause B), but **A ⊥ C | B** — conditioning on the shared cause kills the correlation.

Read those two signatures again. **They are identical.** A ⊥ C | B, marginally dependent. The chain "A causes C through B" and the fork "B is a common cause of A and C" are *indistinguishable in the data*. They share a skeleton (the same three edges, undirected: A–B, B–C) and neither contains a v-structure. They are Markov equivalent. No dataset, however large, can tell you whether B is a mediator on the path from A to C or a confounder behind both.

Now the exception that proves the rule.

**Collider: A → B ← C.** A and C both cause B; they are otherwise unrelated. The signature is *opposite*: A and C are marginally **independent** (no path connects them with B unconditioned — B is a "closed gate"), but they become **dependent given B** (conditioning on the collider opens the path — this is exactly the collider bias of Chapter 6). The collider has a different independence signature from the chain and fork, so it sits in its own equivalence class and **is identifiable from data**.

This is worth pausing on, because it ties Chapter 6 and Chapter 7 together at the joint. The one three-node structure the data *can* orient is the collider — the v-structure. And the v-structure is precisely the thing you learned to fear in Chapter 6 for the bias it creates. The same feature that makes colliders dangerous to condition on is what makes them visible to discovery algorithms. Colliders are the only arrows the data draws for you. Every other arrow it guesses.

> **v-structure (unshielded collider).** A triple i → k ← j where i and j are *not* adjacent. The presence and pattern of v-structures, together with the skeleton, is all the data can pin down.

### 4.3 The Verma–Pearl criterion

The three-node case generalizes into a clean theorem. Verma and Pearl (1990) proved the operational test:

> **Verma–Pearl criterion.** Two DAGs are Markov equivalent if and only if they have (a) the *same skeleton* (the same undirected adjacencies) and (b) the *same set of v-structures*.

Everything else about the orientation — every edge not forced by a v-structure — is free to flip across members of the class. Verma was Pearl's student; the result is the formal backbone of the intuition Pearl later popularized in *The Book of Why* (Pearl & Mackenzie 2018): you cannot read causation off correlation, because correlation only fixes the equivalence class, and the class generally has more than one member.

### 4.4 The CPDAG: a two-color map of what you know and don't

The equivalence class has a canonical picture, the **CPDAG** (completed partially directed acyclic graph), also called the essential graph. Think of it as a two-color map:

- **Directed edges are *compelled*.** They point the same way in *every* member of the class. The data settled these — typically because they participate in a v-structure or are forced by one through propagation. You can trust them.
- **Undirected edges are *reversible*.** They flip across members. The data is silent on their direction. Every undirected edge is a place where the data has done all it can and an expert must supply the arrow.

(The class characterization is rigorous in Andersson, Madigan & Perlman 1997; learning over equivalence classes is Chickering 2002.)

This picture is the chapter's central tool, because it makes the necessity of rep knowledge *concrete and countable*. Run a discovery algorithm honestly and it returns a CPDAG, not a DAG. Count the undirected edges. That number is exactly how many interview questions Chapter 8 has to answer. The vendor's failure in the opening case was rendering a CPDAG's undirected edge as a directed one and printing it in red.

### 4.5 Why outside knowledge is mathematically necessary

Within an equivalence class the orientation of reversible edges is **empirically underdetermined**. This is not "hard to estimate." It is "the observational data is fully exhausted and still cannot choose." Breaking the tie requires information from *outside* the data, of which there are exactly three kinds:

1. **Temporal order.** Causes precede effects; if you know A happened before B, B cannot cause A.
2. **Interventions / experiments.** If you can set A and watch B move, you have orientation — but that is an RCT, the expensive thing this entire book is a workaround for.
3. **Background / domain knowledge.** A credible structural claim from someone who understands the mechanism.

Meek (1995) showed how such knowledge enters formally: assert one edge as a constraint, and orientation rules *propagate* it — a single expert-oriented edge can cascade into several more becoming compelled. One good rep observation can orient more than one arrow.

The philosophy underneath is Nancy Cartwright's slogan: **"no causes in, no causes out"** [verify exact page — Cartwright 1989, *Nature's Capacities and Their Measurement*]. You cannot squeeze a causal conclusion out of purely associational premises. Some causal assumption must be an *input*. And the broader discovery literature agrees that even the equivalence-class result itself rests on assumptions — causal Markov, faithfulness, and causal sufficiency (Spirtes, Glymour & Scheines 2000). We will flag the faithfulness assumption again below, because honesty requires it.

This is the book's thesis in miniature. The rep interviews are not extra data layered on top of the analysis. They are the *source* of the structural information the data provably cannot contain. Demote them and the arrows that decide where eight figures of detailing go become coin flips.

---

## 5. Worked example on this dataset (process, dead ends, lesson, limit)

Let us run the peer-influence puzzle the way a Fellow actually would, dead ends included.

**The setup.** You have a panel. For each physician you have a measure of exposure to peer prescribing (a colleague in the same practice or referral network switched to the drug) and the physician's own prescribing. They are correlated: physicians whose peers prescribe are more likely to prescribe. The brand wants to know if it can buy prescribing by buying influence — detail the influential, let the network do the rest.

**The three Markov-equivalent stories.** The correlation is consistent with three structures, and they are the three-node theorem in disguise:

- **(A) Contagion — peer influence → prescribing** (a *chain*). The colleague's switch genuinely moves the physician. This is the story the brand wants to be true.
- **(B) Latent homophily — common cause** (a *fork*). An underlying trait — openness to new therapies, an academic practice setting, a shared formulary — drives *both* who clusters with whom (peer exposure) *and* prescribing. There is no influence at all; both are downstream of the same trait.
- **(C) Reverse — prescribing → perceived influence** (a *reverse chain*). High prescribers become visible, get cited, get labeled "influencers." The arrow runs from prescribing to the influence measure, not the other way.

**The dead end.** The tempting move: run a causal-discovery algorithm (PC or GES) on the panel, read off the single returned graph, and report "peer influence causes prescribing — target the influencers." You will get a graph with an arrow on that edge, and it will look authoritative. But the algorithm returned a *CPDAG*; that edge is *undirected* in it (chain and fork are equivalent, and depending on the local structure the reverse may be too). If your software displayed a directed edge, it picked one orientation by an internal tie-breaking rule. You did not discover the arrow. The software guessed it, and you reported the guess as a finding. This is the opening-case vendor's exact error, committed by your own hands.

**The lesson — it's a theorem, not a quirk of your data.** Shalizi & Thomas (2011) proved that contagion, latent homophily, and individual-covariate effects are **generically confounded** in observational social-network data: without strong parametric assumptions or measurement of the right covariates, they are observationally indistinguishable. "Generically" is the load-bearing word. This is not an artifact you could clean up with a better estimator or more rows. The three stories are confounded *as a structural fact about network observational data*.

**The orientation.** What breaks the tie is information from outside the panel. The experienced rep supplies it: *"I've watched Dr. Johnson switch within two weeks of a colleague switching, more than once. I have never once seen it run the other way — him switch and then his colleagues follow."* That is a temporal-plus-structural observation. It rules out story C (the arrow does not run prescribing → influence, at least for Johnson) and favors A over B (she is reporting something that looks like a response to an intervention she has witnessed repeatedly, even if she never formally ran it). Fed in as a Meek constraint, it may orient neighboring edges too.

**The limit.** Rep knowledge is fallible and uneven. One rep's "I've never seen it run the other way" is a sample of her territory, her memory, and her attention — not a randomized trial. So the orientation she licenses is a **prior with a confidence level, not a certainty.** That is exactly why the artifact you build in Chapter 8 is an *annotated* prior DAG — each edge tagged with the rep evidence, a confidence, and any contradictions — and why Chapter 9's SCM carries a *provenance* tag on every edge. The honest output, where reps disagree or hedge, is a *sensitivity analysis across the surviving equivalence class*, not a forced single graph. Necessity-in-principle (Verma–Pearl says *some* outside knowledge must orient the edge) does not mean the rep is always right. It means the rep is the only candidate in the room.

---

## 6. Common misconceptions

- **"If a tool's DAG fits the data, it's the right DAG."** The misconception this chapter exists to kill. *Every* member of the equivalence class fits the data equally well. A tool that shows one oriented DAG is hiding the reversible edges it guessed. "Fits the data" means "is in the class," not "is the causal truth."

- **"More data will resolve the ambiguity."** No. The ambiguity is between graphs that are *observationally identical* — they predict the same correlations. Infinite data sharpens your estimate of those correlations; it never breaks a tie the correlations cannot break.

- **"Rep notes are soft, anecdotal — nice color, not model input."** Structurally backwards. Because data identifies only the equivalence class, the rep's edge-orientation knowledge is the *only admissible source* of the missing structural information short of an RCT. It is first-class input, not garnish (developed fully in Chapter 8).

- **"Causal discovery is causal inference."** Discovery proposes a skeleton and orients what it can; it leaves a CPDAG full of undirected edges. Inference — estimating an effect — needs those edges oriented. Discovery hands you the questions; it does not answer them.

- **"The equivalence-class result is assumption-free."** It rests on *faithfulness* (no two causal paths that cancel each other out so perfectly that a real dependence shows up as zero correlation). Faithfulness usually holds but can be violated. The map from data to equivalence class is itself conditional.

---

## 7. Evidence check + consent/compliance & patient-welfare check

**Evidence check.** The core results here are *independent and mathematically settled*, with the lowest aging risk in the book. Verma & Pearl (1990) is the foundational theorem; Andersson, Madigan & Perlman (1997) and Chickering (2002) are peer-reviewed characterizations of the class and its learning; Meek (1995) is the orientation-propagation result; Spirtes, Glymour & Scheines (2000) is the standard discovery monograph. The peer-influence application rests on Shalizi & Thomas (2011), an independent peer-reviewed proof — not a vendor claim and not hypothetical. The one *contested* status is the book's recurring caveat: "rep knowledge can orient causal edges" is **defensible in principle** (Verma–Pearl makes *some* outside orientation mathematically necessary) but the **quality of rep elicitation is an open empirical question** carried into Chapters 8 and 9. Hold both halves. Items flagged `[verify]`: the exact page for Cartwright's slogan; Verma & Pearl (1990) page numbers (220–227 per dblp, confirm against the UAI proceedings); Frydenberg (1990) page range.

**Consent/compliance & patient-welfare check.** This chapter's stakes are mostly epistemic, but two welfare points are real. First, the opening-case failure — orienting an influence edge by an algorithm's tie-break — is not just wasteful; if it routes detailing toward physicians on the basis of a hallucinated network position, it can distort which patients hear which clinical message for reasons unrelated to those patients' needs. Getting the arrow right is a welfare matter, not only a budget one. Second, the peer-influence model touches physician profiling. Using rep knowledge to *understand mechanism* (which way influence runs) is legitimate and feeds the educational pathway; using it to assemble a "who is persuadable" targeting list edges toward the regulatory line we draw in Chapter 13. Orient edges here; defer who-to-target to Chapter 12 and the compliance boundary to Chapter 13.

---

## 8. Named Fellow artifact: the equivalence map

Produce a one-page **equivalence map** for a correlation in your panel. It has three columns:

1. **The correlation,** stated as an observational fact ("peer-exposure and NRx are positively associated; the association attenuates conditional on practice setting").
2. **The Markov-equivalent stories,** enumerated — for the peer case, the three (A contagion / B homophily / C reverse), each drawn as its mini-graph with its independence signature, and a sentence on why it fits the data equally well.
3. **The orienting evidence,** edge by edge: for each reversible edge, what *kind* of outside information (temporal order, intervention, domain knowledge) would orient it, the *specific* rep observation that could supply it, and the confidence and contradictions to flag.

The deliverable is not "here is the causal graph." It is "here is exactly what the data settled, exactly what it left open, and exactly what evidence — available only from the rep — would close each open edge." That document is the input to the Chapter 8 interview design and, eventually, the provenance tags of the Chapter 9 SCM.

---

## 9. Research finding for Fellows

The reframe a Fellow should leave with: **the rep interviews are the source of structural information, not supplementary data.** Most of the field treats expert elicitation as a sanity check layered on top of a data-driven graph — run the algorithm, then ask a human if it looks right. Verma–Pearl inverts that. The data-driven step *cannot* produce the oriented graph; it produces an equivalence class with undirected edges, and the human is the one who turns those edges into arrows. Elicitation is not validation. It is construction. For this dataset, the implication is concrete and publishable: the number of reversible edges in the discovered CPDAG of the message-prescribing graph is a *measurable quantity* — it tells you precisely how much of your causal model is riding on rep knowledge rather than data, and no existing pharma analytics pipeline reports it.

---

## 10. Exercises

1. **(Understand)** Write out the conditional-independence signature for each of A → B → C, A ← B → C, and A → B ← C. State which two are Markov equivalent and why, and which one the data can orient and why. (No code required; this is the theorem by hand.)

2. **(Analyze)** Take a different correlation in the rep-visit panel — say, "samples left" and NRx. Enumerate at least three Markov-equivalent structures (include a common-cause story and a reverse-causation story). For each, name the conditional independence it implies and explain why observational data cannot rule it out.

3. **(Apply+)** Run a causal-discovery algorithm (PC or GES, e.g. via `causal-learn`) on the synthetic rep-visit dataset. Output the CPDAG, *not* a single DAG. Count the undirected edges. For each undirected edge, write the one-sentence interview question whose answer would orient it. (The known ground-truth structure of the synthetic dataset lets you check, afterward, which oriented edges discovery got right and which it could only have guessed.)

4. **(Evaluate — produces artifact)** Build the full **equivalence map** of Section 8 for the peer-influence-and-prescribing correlation in your thread's dataset, including the three stories, their signatures, and edge-by-edge orienting evidence with confidence tags. This artifact is the direct input to your Chapter 8 interview prompts.

---

## 11. Five-part AI block

**When to use AI.** Use an LLM or `causal-learn`/`pgmpy` to *enumerate* the equivalence class — to ask "given this skeleton and these v-structures, what are all the orientations consistent with the data?" That is mechanical bookkeeping the machine does faithfully. Use an LLM to draft the independence signatures and to sanity-check that you have not missed a member of the class.

**When NOT to use AI.** Never let an LLM *orient* a reversible edge from its own training prior. If you ask "does peer influence cause prescribing or vice versa?" the model will answer fluently and confidently — and that answer is, by the theorem, ungrounded in your data. An LLM asserting an arrow with no rep-quoted evidence behind it is producing exactly the hallucinated structure Chapter 8 warns against. The machine enumerates the class; only the rep orients within it.

**LLM exercise (ready-to-paste prompt).**

```
You are helping me reason about Markov equivalence, not assert causal claims.
I have an observational correlation between two variables: PEER_EXPOSURE
(a physician's colleague switched to the drug) and NRX (the physician's own
prescribing). They are positively correlated.

Do NOT tell me which causes which. Instead:
1. Enumerate every distinct causal structure over these two variables (plus
   any latent common cause) that is consistent with a positive correlation.
2. For each, state the conditional-independence relation it implies.
3. State which structures are Markov equivalent to each other and why
   (same skeleton, same v-structures).
4. For each reversible edge, state what KIND of outside evidence (temporal
   order, intervention, domain knowledge) would be needed to orient it.
Label clearly that you cannot orient any reversible edge from data alone.
```

**CLI exercise.** On the synthetic dataset, run a discovery algorithm from the command line (e.g. a short `causal-learn` script invoked via `python -m`), dump the CPDAG adjacency to disk, and write a one-line shell command (`grep`/`awk`) that counts undirected edges in the output. The count is your "how many arrows is the model trusting reps for" metric. Re-run after injecting the known ground-truth v-structures as background constraints and watch the undirected-edge count drop — that is Meek propagation, observed.

**AI validation.** For any oriented graph an AI tool hands you, demand the CPDAG behind it. Ask: which of these arrows are *compelled* (the same in every member of the class) and which are the tool's tie-break? Cross-check the compelled edges against the synthetic ground truth. Reject any displayed arrow that is reversible in the CPDAG and unsupported by an outside-the-data source.

---

## 12. AI Use Disclosure

This chapter's prose was drafted with LLM assistance from the research notes in `pantry/07-markov-equivalence_notes.md`. All citations were carried from those notes with their `[verify]` flags intact and none were generated by the model. The peer-influence three-story structure, the chain/fork/collider treatment, and the CPDAG framing originate in the source essay and the cited literature, not the model's prior. Per the own-thread bonus rule, a Fellow's lab disclosure should name at least one edge orientation in their graph that required rep or human structural knowledge no dataset could supply — this chapter is the reason that rule exists.

---

## 13. What would change my mind

I would weaken the "rep knowledge is necessary" claim if a method emerged that orients reversible edges from observational data *without* an outside assumption — but that is logically impossible under Verma–Pearl, so the more realistic version is: I would update on *which* outside source is best. If structured elicitation from reps turned out, on a real benchmark, to orient edges *less* accurately than a well-chosen literature prior or a cheap quasi-experiment, then the rep would remain *a* necessary outside source but not the *preferred* one, and the book's emphasis would shift from "interview the rep" to "interview the rep *and* triangulate." The necessity is a theorem; the supremacy of rep elicitation is a bet, and a benchmark could move it.

---

## 14. Still puzzling

Faithfulness sits uneasily under all of this. The whole "data identifies the equivalence class" story assumes no near-perfect cancellation of paths. In a system with feedback and incentives — formularies that adjust to prescribing, reps who shift effort toward responsive physicians — are there structures where two causal effects genuinely cancel, so a real edge reads as independence and drops out of the skeleton entirely? If so, the equivalence class the data points to is missing an edge, and even a perfect interview can only orient edges the skeleton contains. How would a rep's knowledge surface an edge the *skeleton itself* lost? I do not have a clean answer, and it is the place I would push a senior causal researcher next.

---

## 15. Summary

Observational data speaks only the language of conditional independence, and multiple causal graphs speak that language identically. The three-node case is the whole theorem: a chain (A → B → C) and a fork (A ← B → C) have the same independence signature and are indistinguishable; only the collider (A → B ← C), the v-structure, has its own signature and is identifiable. Verma & Pearl (1990) generalized this — two DAGs are Markov equivalent iff they share a skeleton and a set of v-structures — and the class is pictured by a CPDAG whose directed edges the data settled and whose undirected edges an expert must orient. Within a class, reversible edges are empirically underdetermined; breaking the tie requires outside information (temporal order, intervention, or domain knowledge), per Meek (1995) and Cartwright's "no causes in, no causes out." On this dataset, the peer-influence puzzle is the theorem in a lab coat: contagion, homophily, and reverse causation are generically confounded (Shalizi & Thomas 2011), and the rep's lived structural observation is the only admissible tiebreaker — a prior with confidence, not a certainty. The interviews are the source of structural information, not supplementary data.

---

## 16. Key terms

- **Markov-equivalence class** — the set of DAGs encoding the same conditional-independence relations; the most the data can identify.
- **Conditional independence** — A ⊥ C | B: A and C carry no information about each other once B is fixed; the only structural fact observational data reveals.
- **Skeleton** — the undirected adjacency structure of a DAG (which nodes are connected, ignoring direction).
- **v-structure (unshielded collider)** — a triple i → k ← j with i, j non-adjacent; the feature, with the skeleton, that fixes the equivalence class.
- **Verma–Pearl criterion** — two DAGs are Markov equivalent iff they share a skeleton and a set of v-structures.
- **CPDAG (essential graph)** — the canonical representation of an equivalence class; directed edges compelled, undirected edges reversible.
- **Compelled / reversible edge** — an edge oriented identically in every class member (compelled, data-settled) vs one that flips across members (reversible, expert-oriented).
- **Faithfulness** — the assumption that no causal paths cancel to hide a real dependence; required for the data-to-class map.
- **"No causes in, no causes out"** — Cartwright's slogan: causal conclusions require at least one causal assumption as input.

---

## 17. Bridge

The data has handed you a CPDAG full of undirected edges, and the theorem says only the rep can orient them. That raises the obvious operational question: how do you actually extract that knowledge from a person who thinks in patients and accounts, not in arrows and v-structures? WEEK 8 builds the apparatus — the Causal Interview Bot, where the rep talks like a rep and the system listens like a causal scientist, turning lived observation into oriented edges and an annotated prior DAG.

---

## 18. Further reading

- Verma, T. & Pearl, J. (1990). "Equivalence and Synthesis of Causal Models." *Proceedings of the 6th Conference on Uncertainty in Artificial Intelligence (UAI)*, 220–227. [verify page numbers against UAI proceedings] — the skeleton-plus-v-structures theorem this chapter is named for.
- Shalizi, C. R. & Thomas, A. C. (2011). "Homophily and Contagion Are Generically Confounded in Observational Social Network Studies." *Sociological Methods & Research* 40(2): 211–239 (arXiv:1004.4704) — the empirical anchor for the peer-influence example; proves the three stories are confounded, not merely hard to separate.
- Meek, C. (1995). "Causal Inference and Causal Explanation with Background Knowledge." *Proceedings of the 11th Conference on Uncertainty in Artificial Intelligence (UAI)*, 403–410 — how an expert-oriented edge propagates to orient others.
- Andersson, S. A., Madigan, D. & Perlman, M. D. (1997). "A Characterization of Markov Equivalence Classes for Acyclic Digraphs." *Annals of Statistics* 25(2): 505–541 — the rigorous class characterization and essential graph.
- Chickering, D. M. (2002). "Learning Equivalence Classes of Bayesian-Network Structures." *Journal of Machine Learning Research* 2: 445–498 — learning over equivalence classes (GES).
- Spirtes, P., Glymour, C. & Scheines, R. (2000). *Causation, Prediction, and Search*, 2nd ed. MIT Press — causal Markov, faithfulness, causal sufficiency; the discovery monograph.
- Pearl, J. & Mackenzie, D. (2018). *The Book of Why*. Basic Books — the narrative origin of the Verma–Pearl result and the "correlation fixes only the class" intuition.
- Cartwright, N. (1989). *Nature's Capacities and Their Measurement*. Oxford University Press — "no causes in, no causes out." [verify exact page for the slogan]
