# Chapter 7 — Markov Equivalence: Why the Data Can't Decide
*The data speaks only one language. Multiple causal stories speak it identically. This is not a gap in your dataset — it is a theorem.*

A martech vendor pitches your partner a "causal influence engine." The slide is gorgeous: a physician-network graph, NPIs as nodes, arrows between them, a few thick red arrows pointing into a cluster of high-prescribing cardiologists labeled "KEY INFLUENCERS — TARGET THESE." The vendor's claim is that their causal-discovery algorithm learned the graph from prescribing and referral data, found who influences whom, and that detailing the influencers will cascade through the network. The recommended budget reallocation is eight figures.

A Fellow on the project asks one question: "How did the algorithm decide the arrow points *from* the influencer *to* the followers, rather than the other way around?"

The room goes quiet, because the honest answer is: it didn't decide. It couldn't. The edge between "exposed to a peer's prescribing" and "prescribes" is — as we are about to prove — *reversible*. The data is equally consistent with "the influencer caused the followers to switch," "the followers are high prescribers who became visible and got labeled influencers," and "both groups share an underlying openness to the drug and there is no influence at all." The algorithm returned one of these because it had to draw *something* on the screen. The thick red arrow is a coin flip wearing a confidence interval.

The vendor was not lying about the math. The discovery algorithm ran correctly. The unexamined assumption — which in a procurement deck is the same as a lie — is that "the algorithm produced a DAG" implies "the DAG is the causal truth." It does not. It produced a *member of an equivalence class*, and hid the rest of the class behind a single rendering. Acting on the displayed arrows means targeting "influencers" who may simply be sure-things who would prescribe anyway — burning budget on people the drug already has.

The rest of this chapter is the machinery behind the Fellow's question.

---

## What observational data can and cannot see

Start from what a dataset actually contains. A purely observational panel — rows of physicians, columns of measurements, no experiment you ran — reveals exactly one kind of structural fact: **conditional-independence relations**. It tells you that variable A and variable C are correlated, and that the correlation vanishes once you hold B fixed (written A ⊥ C | B), or that it does not. Every pattern, partial correlation, and independence test you can compute is, in the end, a statement about which variables are independent of which others given which third sets.

That is the whole vocabulary the data speaks. And here is the trap: multiple distinct causal graphs can speak the exact same vocabulary. When two graphs imply the identical set of conditional-independence relations, no test — no matter how much data you feed it — can prefer one over the other. They are **Markov equivalent**, and the set of all such graphs is the **Markov-equivalence class**.

This is the most important sentence in the chapter: *observational data can identify a causal graph only up to its equivalence class — never the unique graph.* The rest is showing you why that is true, and what to do about it.

---

## The three-node theorem

The entire lesson lives in three variables. Forget pharma for a page and consider A, B, C.

**Chain: A → B → C.** A causes B; B causes C. The independence signature: A and C are marginally dependent (information flows from A to C through B), but A ⊥ C | B — once you know B, A tells you nothing more about C. The middle node blocks the path when you condition on it.

**Fork (common cause): A ← B → C.** B causes both A and C; A and C have no direct link. The signature: A and C are marginally dependent because they share the common cause B, but A ⊥ C | B — conditioning on the shared cause kills the correlation.

Read those two signatures again. They are identical. A ⊥ C | B, with A and C marginally dependent. The chain "A causes C through B" and the fork "B is a common cause of A and C" are *indistinguishable in the data*. They share a skeleton (the same three undirected connections: A–B, B–C) and neither creates a v-structure. They are Markov equivalent. No dataset, however large, can tell you whether B is a mediator on the path from A to C or a confounder behind both.

Now the exception that proves the rule.

**Collider: A → B ← C.** A and C both cause B; they are otherwise unrelated. The signature is *opposite*: A and C are marginally **independent** — no path connects them with B unconditioned, because B is a closed gate — but they become **dependent given B**. Conditioning on the collider opens the path. This is exactly the collider bias you met in Chapter 6. The collider has a different independence signature from the chain and fork, so it sits in its own equivalence class and *is* identifiable from data.

This ties the last two chapters together at the joint. The one three-node structure the data *can* orient is the collider — the v-structure. And the v-structure is precisely the thing you learned to fear in Chapter 6 for the bias it creates when you condition on it. The same feature that makes colliders dangerous is what makes them visible to discovery algorithms. Colliders are the only arrows the data draws for you. Every other arrow it guesses.

<!-- → [DIAGRAM: Three panels side by side — Chain (A→B→C), Fork (A←B→C), Collider (A→B←C). Under Chain and Fork: "A ⊥ C | B — data cannot distinguish these." Under Collider: "A and C independent marginally, dependent given B — data can identify this." Caption: "The three-node theorem. Two structures share an independence signature; one is unique. Colliders are the only arrows observational data orients."] -->

---

## The Verma–Pearl criterion

The three-node case generalizes into a clean theorem. Verma and Pearl (1990) proved the operational test:

> Two DAGs are Markov equivalent if and only if they have (a) the same **skeleton** — the same undirected adjacencies — and (b) the same set of **v-structures**.

Everything else about the orientation — every edge not forced by a v-structure — is free to flip across members of the class. Verma was Pearl's student; the result is the formal backbone of the intuition Pearl later popularized in *The Book of Why*: you cannot read causation off correlation, because correlation only fixes the equivalence class, and the class generally has more than one member. [verify Verma & Pearl 1990 page numbers against UAI proceedings]

---

## The CPDAG: a two-color map of what you know and don't

The equivalence class has a canonical picture called the **CPDAG** — completed partially directed acyclic graph, also called the essential graph. Think of it as a two-color map.

**Directed edges are compelled.** They point the same way in every member of the class. The data settled these — typically because they participate in a v-structure or are forced by one through propagation. You can trust them.

**Undirected edges are reversible.** They flip across members. The data is silent on their direction. Every undirected edge is a place where the data has done all it can and an expert must supply the arrow.

This picture makes the necessity of rep knowledge concrete and countable. Run a discovery algorithm honestly and it returns a CPDAG, not a DAG. Count the undirected edges. That number is exactly how many interview questions Chapter 8 has to answer. The vendor's failure in the opening case was rendering a CPDAG's undirected edge as a directed one and printing it in red.

---

## Why outside knowledge is mathematically necessary

Within an equivalence class the orientation of reversible edges is **empirically underdetermined**. This is not "hard to estimate." It is "the observational data is fully exhausted and still cannot choose." Breaking the tie requires information from outside the data, of which there are exactly three kinds.

**Temporal order.** Causes precede effects. If you know A happened before B, B cannot cause A. When the sequence is recorded in timestamps, that knowledge is available without an interview.

**Interventions.** If you can set A and watch B move, you have orientation — but that is an experiment, the expensive thing this entire book is a workaround for.

**Background or domain knowledge.** A credible structural claim from someone who understands the mechanism.

Meek (1995) showed how such knowledge enters formally: assert one edge direction as a constraint, and orientation rules propagate it — a single expert-oriented edge can cascade into several more becoming compelled. One good rep observation can orient more than one arrow. [verify Meek 1995 page numbers against UAI proceedings]

The philosophy underneath is Nancy Cartwright's slogan: **"no causes in, no causes out."** [verify exact page — Cartwright 1989, *Nature's Capacities and Their Measurement*] You cannot squeeze a causal conclusion out of purely associational premises. Some causal assumption must be an input. The broader discovery literature agrees: even the equivalence-class result itself rests on assumptions — causal Markov, faithfulness, and causal sufficiency (Spirtes, Glymour & Scheines 2000). Faithfulness — the assumption that no two real causal paths cancel so perfectly that a genuine dependence looks like zero — is usually reasonable but can be violated in systems with feedback and incentives. We carry that caveat forward.

This is the book's thesis in miniature. The rep interviews are not soft supplementary color. They are the *source* of the structural information the data provably cannot contain. Demote them and the arrows that decide where eight figures of detailing go become coin flips.

<!-- → [DIAGRAM: CPDAG with a mix of directed and undirected edges. Directed edges labeled "compelled — data settled." Undirected edges labeled "reversible — rep must orient." Count of undirected edges annotated as "N open questions for Chapter 8." Caption: "The CPDAG is a two-color map: what the data decided, and what it left open for domain knowledge to close."] -->

---

## The peer-influence puzzle: the theorem in a lab coat

Return to the panel. For each physician you have a measure of exposure to peer prescribing — a colleague in the same practice or referral network switched to the drug — and the physician's own prescribing. They are correlated: physicians whose peers prescribe are more likely to prescribe. The brand wants to know if it can buy prescribing by buying influence.

**Three Markov-equivalent stories.** The correlation is consistent with three structures, and they are the three-node theorem wearing a lab coat.

Story A — *Contagion*: peer influence → prescribing. The colleague's switch genuinely moves the physician. This is the story the brand wants to be true.

Story B — *Latent homophily*: common cause. An underlying trait — openness to new therapies, an academic practice setting, a shared formulary policy — drives both who clusters with whom and prescribing. There is no influence at all; both are downstream of the same latent factor.

Story C — *Reverse causation*: prescribing → perceived influence. High prescribers become visible, get cited, get labeled "influencers." The arrow runs from prescribing to the influence measure, not the other way.

**The dead end.** The tempting move: run a discovery algorithm, read off the graph, report "peer influence causes prescribing — target the influencers." You will get a graph with an arrow on that edge, and it will look authoritative. But the algorithm returned a CPDAG in which that edge is undirected. If your software displayed a directed edge, it picked one orientation by an internal tie-breaking rule. You did not discover the arrow. The software guessed it, and you reported the guess as a finding. This is the opening-case vendor's exact error, committed by your own hands.

**It is a theorem, not a quirk.** Shalizi and Thomas (2011) proved that contagion, latent homophily, and individual-covariate effects are **generically confounded** in observational social-network data — without strong parametric assumptions or measurement of the right covariates, they are observationally indistinguishable (Shalizi & Thomas, *Sociological Methods & Research* 40(2):211–239; arXiv:1004.4704). "Generically" is the load-bearing word. This is not an artifact you could clean up with a better estimator or more rows. The three stories are confounded as a structural fact.

**What breaks the tie.** Information from outside the panel. The experienced rep supplies it: "I've watched Dr. Johnson switch within two weeks of a colleague switching, more than once. I have never once seen it run the other way." That is a temporal-plus-structural observation. It rules out story C and favors A over B — she is reporting something that looks like a response, repeated across cases, even if she never formally ran an experiment. Fed in as a Meek constraint, it may orient neighboring edges too.

**The limit.** Rep knowledge is fallible and uneven. One rep's "I've never seen it run the other way" is a sample of her territory, her memory, and her attention. The orientation she licenses is a **prior with a confidence level**, not a certainty. That is exactly why the artifact you build in Chapter 8 is an *annotated* prior DAG — each edge tagged with the rep evidence, a confidence, and any contradictions. Where reps disagree or hedge, the honest output is a sensitivity analysis across the surviving equivalence class, not a forced single graph. Necessity-in-principle (Verma–Pearl says *some* outside knowledge must orient the edge) does not mean the rep is always right. It means the rep is the only candidate in the room.

<!-- → [DIAGRAM: Three side-by-side mini-graphs — Story A (peer_exposure → NRx), Story B (latent_trait → peer_exposure and latent_trait → NRx), Story C (NRx → peer_exposure). All three consistent with the same positive observed correlation. Caption: "The peer-influence puzzle: contagion, homophily, and reverse causation fit the data identically. The algorithm cannot choose. The rep can."] -->

---

## The named artifact: the equivalence map

Produce a one-page equivalence map for a correlation in your panel. Three columns:

**The correlation**, stated as an observational fact: which two variables are associated, and what the association looks like conditionally.

**The Markov-equivalent stories**, enumerated: each drawn as a mini-graph with its independence signature, and a sentence on why it fits the data equally well.

**The orienting evidence**, edge by edge: for each reversible edge, what kind of outside information would orient it, the specific rep observation that could supply it, and the confidence and contradictions to flag.

The deliverable is not "here is the causal graph." It is "here is exactly what the data settled, exactly what it left open, and exactly what evidence — available only from the rep — would close each open edge." That document is the input to the Chapter 8 interview design.

---

## What would change my mind

I would weaken the "rep knowledge is necessary" claim if a method emerged that orients reversible edges from observational data without an outside assumption — but that is logically impossible under Verma–Pearl, so the realistic version is: I would update on *which* outside source is best. If structured elicitation from reps turned out, on a real benchmark, to orient edges less accurately than a well-chosen literature prior or a cheap quasi-experiment, then the rep would remain *a* necessary outside source but not the *preferred* one. The necessity is a theorem; the supremacy of rep elicitation over other outside sources is a bet, and a benchmark could move it.

## Still puzzling

Faithfulness sits uneasily under all of this. The whole story assumes no near-perfect cancellation of paths. In a system with feedback and incentives — formularies that adjust to prescribing, reps who shift effort toward responsive physicians — there could be structures where two real causal effects genuinely cancel, so an edge drops out of the skeleton entirely. If the skeleton itself is missing an edge, even a perfect interview can only orient edges the skeleton contains. How would a rep's knowledge surface an edge the skeleton lost? The Shalizi–Thomas result makes this sharper, not easier: if the three peer-influence stories are generically confounded, and one of them involves a latent variable the panel cannot measure, the skeleton may never recover the full structure regardless of how the rep orients what is visible.

---

**LLM exercise (copy-paste prompt):**

> "You are helping me reason about Markov equivalence, not assert causal claims. I have an observational correlation between two variables: PEER_EXPOSURE (a physician's colleague switched to the drug) and NRX (the physician's own prescribing). They are positively correlated. Do NOT tell me which causes which. Instead: (1) enumerate every distinct causal structure over these two variables (plus any latent common cause) consistent with a positive correlation; (2) for each, state the conditional-independence relation it implies; (3) state which structures are Markov equivalent to each other and why (same skeleton, same v-structures); (4) for each reversible edge, state what KIND of outside evidence (temporal order, intervention, domain knowledge) would be needed to orient it. Label clearly that you cannot orient any reversible edge from data alone."

**CLI exercise.** On the synthetic dataset, run a discovery algorithm (PC or GES, via `causal-learn`) and dump the CPDAG adjacency matrix to disk. Write a one-line shell command that counts undirected edges in the output — that count is your "how many arrows is the model trusting reps for" metric. Then re-run after injecting the known ground-truth v-structures as background constraints and watch the undirected-edge count drop. That is Meek propagation, observed.

**AI Validation exercise.** For any oriented graph an AI tool hands you, demand the CPDAG behind it. Ask: which of these arrows are compelled — the same in every member of the class — and which are the tool's tie-break? Cross-check the compelled edges against the synthetic ground truth. Reject any displayed arrow that is reversible in the CPDAG and unsupported by an outside-the-data source.

**AI Use Disclosure**

*Write two sentences naming exactly what an AI tool did in your work for this chapter and what judgment you supplied that the AI could not. The judgment most specific to this chapter: which edges in your panel's CPDAG require rep knowledge to orient, and what specific rep observation would supply each one — a determination the data cannot make and the model cannot guess.*

---

## Exercises

**Warm-up**

1. *(Recall — tests the three-node theorem)* Write out the conditional-independence signature for each of A → B → C, A ← B → C, and A → B ← C. State which two are Markov equivalent and why, and which one the data can orient and why. No code required — this is the theorem by hand. *What this tests: whether you can derive the core result without a formula.*

2. *(Recall — tests the CPDAG)* Define a compelled edge and a reversible edge in your own words. Explain why the count of reversible edges in a CPDAG is a meaningful number for a project that relies on rep elicitation. *What this tests: whether you can connect the abstract object to the practical consequence.*

3. *(Recall — tests Verma–Pearl)* State the Verma–Pearl criterion in one sentence. Then give one example of two three-node DAGs that satisfy it (same skeleton, same v-structures) and one example of two three-node DAGs that violate it (different v-structures). *What this tests: whether you can apply the criterion, not just recite it.*

**Application**

4. *(Apply — peer-influence enumeration)* Take the correlation between "samples left" and NRx in the rep-visit panel. Enumerate at least three Markov-equivalent structures — include a common-cause story and a reverse-causation story. For each, name the conditional independence it implies and explain in one sentence why observational data cannot rule it out. *What this tests: whether you can apply the equivalence-class reasoning to a new correlation.*

5. *(Apply — discovery run)* Run a causal-discovery algorithm on the synthetic rep-visit dataset and output the CPDAG, not a single DAG. Count the undirected edges. For each undirected edge, write the one-sentence interview question whose answer would orient it. Check your oriented guesses against the known ground-truth structure afterward. *What this tests: whether you can distinguish what the algorithm settled from what it guessed.*

6. *(Apply — vendor critique)* A vendor's slide shows a DAG over five physician-network variables, all edges directed, with a note that "the graph was learned from prescribing and referral data using a state-of-the-art causal-discovery algorithm." Write the two questions you would ask the vendor — one about v-structures and one about reversible edges — to determine whether the displayed arrows are compelled by the data or chosen by a tie-breaking rule. *What this tests: whether you can translate the Verma–Pearl result into a practical due-diligence question.*

**Synthesis**

7. *(Synthesize — theorem + rep knowledge)* The chapter claims that "rep interviews are the source of structural information, not supplementary data." Connect this claim to the Verma–Pearl theorem: explain precisely what the theorem says the data leaves open, what kind of outside information closes each opening, and why rep structural observation qualifies as that kind of information while a rep's opinion about which message "seems to work" does not. *What this tests: whether you can distinguish the kind of rep knowledge that orients a causal edge from the kind that does not.*

8. *(Synthesize — faithfulness caveat)* The chapter notes that the Verma–Pearl story rests on the faithfulness assumption. Describe a plausible scenario in a rep-visit setting — involving feedback or incentives — where faithfulness could be violated, where a real causal path might cancel out and disappear from the skeleton. Explain what consequence this would have for the equivalence-map artifact you are building. *What this tests: whether you understand the limits of the framework you are applying.*

**Challenge**

9. *(Challenge — produce the named artifact)* Build the full equivalence map for the peer-influence-and-prescribing correlation in your thread's panel: the observational fact stated precisely, the three Markov-equivalent stories each drawn as a mini-graph with its independence signature, and edge-by-edge orienting evidence with confidence tags and any contradictions between reps. This artifact is the direct input to your Chapter 8 interview design. *What this tests: whether you can execute the full equivalence-map discipline as a complete, usable artifact rather than as a conceptual exercise.*
