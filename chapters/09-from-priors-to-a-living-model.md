# Chapter 9: From Priors to a Living Model

## 1. Overview

This is the chapter where the book's title pays off. You have, by now, three things that have never been in the same room: an annotated prior DAG elicited from reps (Chapter 8), a set of identified population effects from the natural experiment (Chapters 4 and, ahead, 10–11), and the assembled physician-by-time panel (Chapter 2). Each is necessary. None is, by itself, a decision tool. A graph cannot tell you a magnitude; a population effect cannot tell you what to do about Dr. Martinez specifically; the panel is just rows.

This chapter assembles them into a **parameterized structural causal model (SCM)** and bolts on the two things that make a model *living*: a **counterfactual engine** that answers individual Rung-3 questions, and **drift monitoring** that watches the world change underneath the model — with one specialized alarm, opt-out-rate drift, that is this book's own contribution.

The decision the brand actually faces is not "what is the average effect of safety-first messaging?" It is "for *this* physician, which message sequence next week?" That is an individual counterfactual, and reaching it requires machinery no Next-Best-Action engine has: Pearl's abduction–action–prediction procedure run on a real SCM. When you finish, you will be able to parameterize the SCM (edges, structural equations / CPTs, and a provenance tag on every edge), run an individual counterfactual on a synthetic physician, and specify a drift monitor that knows when its own answers have gone stale.

---

## 2. Learning objectives

By the end of this chapter, a Fellow will be able to:

- **(Create)** Parameterize an SCM from the three ingredients: assign structural equations / CPTs, and tag every edge with provenance (data-estimated, rep-elicited, literature-supported, or assumed) and uncertainty.
- **(Apply)** Run an individual counterfactual via abduction → action → prediction on a synthetic physician, and compare the result to the no-abduction population prediction.
- **(Understand)** Distinguish the three granularities — population ACE, subgroup CATE, individual counterfactual — and explain why the individual case requires abduction and an SCM.
- **(Evaluate)** Specify a drift monitor, including the opt-out-rate alarm, stating its threshold and what it triggers (refit / re-elicit / FCI re-spec).
- **(Evaluate)** Recognize when an individual counterfactual is only *partially identified* and report bounds rather than a false point estimate; constrain the action step to the educational pathway for any deployable recommendation.

---

## 3. Opening case (failure-first)

A Fellow ships what looks like the finished product. The natural-experiment analysis came back clean: the staggered rollout identified an average treatment-on-treated effect of the safety-first message of, say, +4 NRx per physician over 90 days, with a tight confidence interval and a passing falsification test. The Fellow writes it up, the partner is pleased, and the recommendation goes out: *deliver the safety-first message.* To everyone.

Then the brand director asks the question that breaks the deliverable: "Great — so which message do I send Dr. Martinez next week?"

The +4 is a *population average*. Dr. Martinez is not the population. She got the safety-first message last quarter, her knowledge ticked up, and her prescribing barely moved — below what the average effect predicts. Is she a non-responder? Formulary-blocked? Someone for whom efficacy-first would have worked? The population number is silent on all of it, because it threw away exactly the case-specific information the question turns on. The Fellow can tell the director what happens *on average*; the director is asking what happens *to this physician*, and those are different rungs of Pearl's ladder.

The failure is not that the IV estimate is wrong. It is right, and it is necessary. The failure is mistaking a Rung-2 population effect for a Rung-3 individual decision. The +4 is the *background*; the recommendation is a counterfactual that has to read Dr. Martinez's specific state first. The rest of this chapter builds the machine that can.

---

## 4. Core sections

### 4.1 The transition: from population effect to individual decision

Chapter 4's instrument and Chapter 10's staggered DiD deliver a *population* / Rung-2 quantity: the average treatment-on-treated effect of a message variant on NRx. That is a real achievement — most pharma analytics never gets off Rung 1. But the decision a brand faces is Rung-3 and *individual*: "for this physician, which message sequence next week?" A Living Model closes the gap by assembling three ingredients into an SCM and running counterfactuals on it.

The student trap, stated plainly so you can avoid it: **conflating the population ATE with the individual recommendation.** The whole point of what follows is to leave the population average behind.

### 4.2 The three ingredients

1. **The elicited prior DAG** (Chapter 8) — *structure*: nodes, oriented edges, evidence and confidence tags. This orients the edges the data cannot (the Markov-equivalence result of Chapter 7; the KEBN handoff).
2. **The identified effects** (Chapter 4 IV / Chapter 10 DiD / Chapter 11 causal forest + DML) — the *magnitudes* the quasi-experiments license you to believe.
3. **The data** (the Chapter 2 panel) — fits the conditional probabilities / structural-equation parameters *and* supplies each physician's history, which the abduction step will read.

Structure from the rep, magnitude from the experiment, history from the panel. The SCM is where they meet.

### 4.3 Parameterizing the SCM

A DAG becomes a **structural causal model** when each node gets a *structural equation* of the form

> Xᵢ = fᵢ( parents(Xᵢ), Uᵢ )

where Uᵢ is an exogenous noise term — everything affecting Xᵢ that is not captured by its modeled parents. In the KEBN/Bayesian-network rendering, the same object is **edges + CPTs + provenance/confidence.** Defining the structural equations (or CPTs) is what turns Chapter 8's qualitative graph into something you can *run forward*.

The discipline that makes the model honest is **provenance.** Every edge carries a tag:

- **data-estimated** — the magnitude came from the panel and an identified effect;
- **rep-elicited** — the orientation came from a named rep observation (Chapter 8), confidence X;
- **literature-supported** — a published causal estimate;
- **assumed** — you had nothing and made a modeling choice (flag it loudest).

Provenance is what makes the model auditable: every edge orientation traces to data, an identified effect, or a named rep observation. Chapter 7's "reversible edge" becomes, here, "edge oriented by rep knowledge, confidence X" — and where edges remain genuinely unresolved (contradictory reps, low confidence), the honest output is **sensitivity analysis over the surviving equivalence class**, not a forced single graph. A Living Model that hides its assumed edges is the vendor DAG of Chapter 7 with extra steps.

### 4.4 The counterfactual engine: abduction → action → prediction

This is the heart of the chapter and the thing no NBA engine can do. Pearl's three-step procedure (the same one previewed in the framework book's counterfactual chapter; restated and applied in recent work, e.g. arXiv:2301.02499):

- **Step 1 — Abduction.** Take the observed evidence for a *specific* physician — her visit history, her rep's elicited read, her prior NRx — and infer the exogenous noise, P(U | evidence). This is where individual particularity enters. The Uᵢ terms are what distinguish Dr. Martinez from the panel average: her idiosyncratic responsiveness, the part of her behavior the structural equations with population parameters do not explain.

- **Step 2 — Action.** Apply do(message = m′): surgically set the message/sequence to the candidate, leaving every other structural equation intact. The do-operator *replaces the structural equation for that one node*, not the whole model — the rest of the mechanism still runs.

- **Step 3 — Prediction.** Run the modified SCM forward, carrying the *recovered* Uᵢ from Step 1, to compute counterfactual NRx under each candidate message. Rank the candidates → the recommendation.

The reason this beats the population number is Step 1. A correlation or effect engine has *no Uᵢ to recover* — it has no structural model, so it cannot read Dr. Martinez's specific state. It can only return the average. Abduction is the "read the current state before you simulate" step, and it is the difference between a weather model that ingests today's conditions and one that just reports the seasonal normal.

This particular counterfactual is **pre-factual**: it is evaluated *before* acting, conditioning on the physician's pre-action characteristics. It is distinct from the *retrospective* counterfactual the rep was asked in Chapter 8 ("would that account have converted without the MSL visit?"), which conditions on the observed outcome too and can therefore recover the noise more sharply. Same three steps; different thing conditioned on.

### 4.5 The three granularities

Three rungs of specificity, increasing in value and difficulty:

1. **Population ACE** (average causal effect) — no abduction; Rung-2 reachable from the IV/DiD estimate alone. The +4 of the opening case.
2. **Subgroup CATE** (conditional average treatment effect) — partial conditioning; the causal forest of Chapter 11 splits the effect by specialty, baseline, competition.
3. **Individual counterfactual** — full abduction; the hardest and most valuable, and the only one that answers "which message for *this* physician."

The research finding for Fellows lives here: **the Rung-3 individual recommendation is the operational payoff no NBA engine produces.** NBA engines stop at Rung-1 propensity ("who is likely to prescribe"); even a good one reaches a Rung-1 score, not a Rung-3 counterfactual. The Living Model's individual recommendation is a categorically different object.

### 4.6 The Living Model architecture — three components

This is where the book's title is finally literal. A Living Model has three parts:

1. **Parameterized SCM** — the model of mechanism (§4.3).
2. **Counterfactual engine** — abduction–action–prediction producing individual recommendations (§4.4).
3. **Structural-change / drift monitoring** — the part that makes it *living*. The model watches for the world shifting underneath it.

The book's signature drift target is **opt-out-rate drift.** Recall from Chapter 6 that physician consent is a Berkson/collider structure on the sampling frame: who opts out of logging is correlated with physician characteristics. That means the *selection structure* — and therefore the *bias structure of every downstream estimate* — depends on the opt-out rate. When opt-out rates shift, the bias structure shifts, even if nothing else about the world changes. So the drift monitor watches opt-out-rate movements *specifically*, not just generic covariate drift. Other live triggers: loss of exclusivity (LOE), formulary changes — anything that makes the pre-shift parameters non-transportable to the post-shift world.

Crucially, the monitor must flag *structural* change, not just parameter drift. A formulary change can flip an edge's sign; an LOE can sever a pathway; territory turnover can age out the elicited prior so its orientations rot. The alarm should distinguish "the magnitudes moved, refit" from "the structure moved, re-elicit / re-run FCI."

---

## 5. Worked example on this dataset

A schematic adaptation of the framework's worked counterfactual to rep-visit data. Numbers are illustrative and flagged `[verify]` — to be regenerated on the synthetic dataset with known ground truth.

**The SCM (simplified to three equations):**

```
Message   = U_M
Knowledge = α · Message   + U_K
NRx       = β · Knowledge + U_Rx
```

α and β are estimated from the identified effects (the natural experiment supplies them; provenance: data-estimated). The U terms are the physician-specific noise the abduction step recovers.

**Observe Dr. Martinez.** She was delivered the safety-first message. Her knowledge lift was *modest*. Her NRx came in *below* what α and β predict from her knowledge level.

**Step 1 — Abduction.** Recover her noise terms. The pattern — knowledge moved less than message would predict, NRx moved less than knowledge would predict — implies a particular U_K and U_Rx for her: she *over-responds on knowledge but under-converts knowledge into scripts.* This matches the rep's elicited read from Chapter 8: "she gets it but is formulary-blocked." The abduction recovers numerically what the rep observed qualitatively — a satisfying convergence of the two ingredients, and a sign the model is coherent.

**Step 2 — Action.** Apply do(message = efficacy-first sequence). Replace the Message equation with the candidate; leave the Knowledge and NRx equations intact.

**Step 3 — Prediction.** Run forward, carrying her recovered U_K and U_Rx, to get counterfactual NRx under efficacy-first versus status quo. Because her noise terms are carried through, the prediction is *hers*, not the panel average's. Rank the candidate messages → the recommendation for next week.

**The contrast that makes the point.** A no-abduction prediction would use population-average noise and return the same recommendation for every physician with her observed message and knowledge. The abduction step is the entire difference between "deliver safety-first to everyone" (the opening-case failure) and "Dr. Martinez is formulary-blocked, so the efficacy lift won't convert — route her to an access-education message instead, and don't waste the efficacy deck on her."

**The limit (and the failure cases this guards against).**

1. **Over-claiming point identification.** If the SCM is nonlinear or U is not uniquely recoverable, the individual counterfactual is only *partially identified* — bounded, not pinned. A Living Model that emits a confident single NRx number where only bounds exist is lying. The honest output is a *range* plus the assumption it depends on.
2. **Silent generalization off the consenting subsample.** Abduction performed on consenting physicians and applied to all generalizes wrong — and gets *worse over time as opt-out rates drift.* That is exactly why opt-out-rate monitoring is architectural, not optional.
3. **Stale priors / static model.** Without drift checks, a model trained before an LOE or formulary change recommends obsolete messages, and an aged-out elicited prior rots the orientations. The monitor must catch structural change, not just parameter drift.

---

## 6. Common misconceptions

- **"A good population effect estimate is the deliverable — once you have the IV/DiD number, you're done."** No. The population number is Rung-2 background; the *decision* is a Rung-3 individual counterfactual. The IV estimate is an ingredient, not the dish.

- **"A DAG alone is the model."** A DAG supports interventions but *not* counterfactuals. Rung 3 needs the SCM — DAG **plus** structural equations **plus** noise — and the abduction step. The graph is necessary and insufficient.

- **"A predictive/NBA model can do this if it's accurate enough."** It cannot do abduction — it has no U to recover, because it has no structural model. Accuracy on Rung-1 prediction does not buy a Rung-3 counterfactual.

- **"The individual counterfactual is always point-identified."** Only under full SCM specification with recoverable noise. Otherwise it is partially identified and must be reported as bounds.

- **"Drift monitoring is just retraining on recent data."** That handles parameter drift. It misses *structural* change — an edge flipping sign, a pathway severing, a prior aging out — which requires re-elicitation or an FCI re-spec, not a refit.

---

## 7. Evidence check + consent/compliance & patient-welfare check

**Evidence check.** The counterfactual machinery is *foundational and verified*: Pearl's SCM and the abduction–action–prediction procedure (Pearl, *Causality*, 2009; Pearl & Mackenzie, *The Book of Why*, 2018; restated/applied in arXiv:2301.02499). The "abduction strictly requires the SCM" claim is underwritten by Pearl's Causal Hierarchy and the Causal Hierarchy Theorem (Bareinboim, Correa, Ibeling & Icard) [verify full citation]. Two directly supporting papers are confirmed real and safe to cite: Ness, Paneri & Vitek (2019), "Integrating Markov processes with structural causal modeling enables counterfactual inference in complex systems" (arXiv:1911.02175), which casts a Markov process model as an SCM and runs Pearl's three-step counterfactual; and Bottou et al. (2013), "Counterfactual Reasoning and Learning Systems" (arXiv:1209.2355), which is reference [7] of the Ness paper. The *drift-monitoring* component, and especially the **opt-out-rate specialization**, is the Living Model framework's own contribution and this book's original move — it has *no external benchmark yet*, and is presented as a design proposal, not an empirically validated method. Items flagged `[verify]`: the worked-example numbers (illustrative; regenerate on the synthetic dataset); the Bareinboim et al. PCH citation; the exact mechanism by which the framework's monitor detects structural vs parametric change. The LLM's role is firewalled exactly as in Chapter 8: data + identified effects parameterize the model; an LLM may scaffold code or structure elicited language, but **never invents structural equations or parameters** (cf. arXiv:2511.00574 and the hallucination caveat).

**Consent/compliance & patient-welfare check.** Two load-bearing constraints. First, the **regulatory line**: the counterfactual engine *can* rank messages on the reciprocity (M3) or relationship-maintenance (M2) pathways, but those are not legally drivable. Constrain the **action step** to the educational pathway (M1) for any *deployable* recommendation; M2/M3 counterfactuals are analysis-only and must be labeled so. A model that recommends a message because it works through reciprocity is recommending a compliance violation. Second, the **consent collider drives the drift target**: because the model is fit and abducted on the consenting subsample, applying it to all physicians is a selection-bias hazard that *grows* as opt-out rates move — which is why the opt-out-rate alarm is welfare-protective, not just statistical. Patient welfare is served by recommending the *clinically appropriate educational* message for a physician's actual situation; it is harmed by a model that silently optimizes a non-educational pathway or generalizes off a biased frame. The full consent/FCI treatment and the handoff brief are Chapter 13.

---

## 8. Named Fellow artifact: parameterized SCM + an individual counterfactual

Build the SCM and run one individual counterfactual end to end.

**Part A — parameterize.** Take your Chapter 8 annotated prior DAG. For each node, write a structural equation (or CPT). For each edge, assign a magnitude (from an identified effect where you have one) and a **provenance tag** (data-estimated / rep-elicited+confidence / literature-supported / assumed). List every *assumed* edge explicitly — that list is your model's exposed underbelly. Where reps contradicted each other (Chapter 8), set up that edge as a *sensitivity dimension*, not a fixed value.

**Part B — run the counterfactual.** Pick one synthetic physician with a full history. Run all three steps by hand or in code:

1. **Abduction** — recover her U terms from her observed (message, knowledge, NRx).
2. **Action** — do(message = each candidate).
3. **Prediction** — carry her U forward, compute counterfactual NRx per candidate, rank.

Report the recommendation *and* the no-abduction population prediction side by side, so the value of abduction is visible. If the counterfactual is only partially identified, report **bounds**. Constrain the candidate set to M1 (educational) messages for the deployable recommendation; if you analyze M2/M3 counterfactuals, label them analysis-only.

**Part C — one drift alarm.** Specify the opt-out-rate drift monitor: the threshold, the window, and what crossing it triggers (refit / re-elicit / FCI re-spec). One alarm, fully specified, is the seed of the Living Model's monitoring component.

---

## 9. Research finding for Fellows

The reframe: **the Rung-3 individual recommendation is the operational payoff no NBA engine produces — and the reason is abduction, which strictly requires an SCM.** A predictive engine, however accurate, has no exogenous-noise term to recover and therefore cannot read an individual physician's specific state; it returns the population average dressed as a personalized score. The Living Model returns a genuinely case-specific recommendation because Step 1 ingests Dr. Martinez's history before Step 3 simulates her next week. This is testable and productizable: on the synthetic dataset with known ground-truth U, you can *measure* how much the individual recommendation diverges from the population recommendation and how often that divergence flips the chosen message — a number that quantifies exactly what abduction buys, and that no deployed pharma engine reports.

---

## 10. Exercises

1. **(Apply)** Given the three-equation rep-visit SCM in Section 5 and one physician's observed (message, knowledge, NRx), run abduction → action → prediction by hand for two candidate messages. Then compute the *no-abduction* population prediction. Report all three numbers and explain in one sentence what the abduction step changed.

2. **(Understand)** Classify each as pre-factual or retrospective, and say what abduction conditions on in each: (a) "Which message should we send Dr. K next week?" (b) "Would Dr. K have converted without last month's meal?" Explain why the retrospective case can recover the noise more sharply.

3. **(Apply+)** Build a mini-SCM with at least three edges drawn from *two* provenance sources (one data-estimated, one rep-elicited with a confidence tag) and one *assumed* edge. Run a counterfactual; then run a sensitivity sweep over the assumed edge's parameter and report how much the recommendation moves. State whether the recommendation is point-identified or should be reported as bounds.

4. **(Evaluate — produces artifact)** Design the **opt-out-rate drift alarm**: choose a threshold and window, justify them against the selection-bias mechanism from Chapter 6, and specify the decision tree for what a breach triggers (refit parameters / re-elicit the prior / re-run FCI for the consent structure). Deliver it as the monitoring spec for your Living Model prototype.

---

## 11. Five-part AI block

**When to use AI.** Use an LLM to *scaffold the SCM code* (the structural-equation skeleton, the abduction/action/prediction loop), to *run sensitivity sweeps* over unresolved edges, and to *draft the drift-monitor spec* from your stated mechanism. These are engineering and bookkeeping tasks where the model accelerates you without supplying causal content.

**When NOT to use AI.** Never let the LLM *invent structural equations, parameters, or edge magnitudes.* Those come from the identified effects and the data — full stop. An LLM that "estimates" α or β from its training prior, or proposes a structural equation you did not derive, is hallucinating the model's content. The firewall from Chapter 8 holds at every layer: the LLM structures and codes; data and experiments supply the numbers; reps supply the orientations.

**LLM exercise (ready-to-paste prompt).**

```
You are scaffolding code for a structural causal model. Do NOT supply any
parameter values, effect sizes, or structural-equation coefficients from your
own knowledge — leave every numeric parameter as a named variable I will fill
from my identified effects and data.

Given this DAG (nodes and oriented edges with provenance tags pasted below),
write Python that:
1. Defines each node's structural equation X_i = f_i(parents, U_i), with all
   coefficients as named, unfilled parameters.
2. Implements abduction: given an observed (message, knowledge, NRx) for one
   physician, recover the exogenous noise U for that physician.
3. Implements action: do(message = candidate), replacing only that equation.
4. Implements prediction: run forward with the recovered U, return
   counterfactual NRx per candidate.
5. Flags, in a comment, any place where the counterfactual is only partially
   identified and should be reported as bounds.

DAG (with provenance tags):
<paste your annotated DAG here>
```

**CLI exercise.** Run the scaffolded SCM on the synthetic dataset from the command line, looping the counterfactual over every physician, and write a CSV of (population recommendation, individual recommendation) per physician. Then a one-liner (`awk`/`csvstat`) that counts how often the individual recommendation *differs* from the population one — your "what abduction buys" metric. Re-run after perturbing the opt-out rate in the synthetic frame to see the recommendations shift, demonstrating the drift mechanism.

**AI validation.** Before trusting any real run, validate on the synthetic dataset with *known ground-truth effects*: does your SCM, parameterized from the identified effects, recover the known α and β? Does abduction recover the injected U for a physician whose true noise you set? If the model cannot recover known ground truth on synthetic data, it cannot be trusted on real data. Validate recovery first, deploy second.

---

## 12. AI Use Disclosure

This chapter was drafted with LLM assistance from `pantry/09-from-priors-to-a-living-model_notes.md`. All citations were carried from the notes with their `[verify]` flags intact; the two verified references (Ness et al. 2019, arXiv:1911.02175; Bottou et al. 2013, arXiv:1209.2355) are confirmed real per the notes, and no citation or numeric parameter was generated by the model. The worked-example numbers are illustrative and `[verify]`-flagged for regeneration on the synthetic dataset. Consistent with the chapter's own firewall, the model scaffolded prose and structure; it did not supply structural equations, effect sizes, or causal claims.

---

## 13. What would change my mind

The chapter's strong claim is that the individual counterfactual is worth the machinery — that abduction buys something a good predictive model cannot. I would weaken it if, on the synthetic dataset and then on real partner data, the individual recommendation turned out to *almost always agree* with a well-calibrated subgroup (CATE) recommendation — that is, if the full abduction rarely flipped the chosen message relative to a causal forest split by a few covariates. Then the honest position would be that Rung-3 individual reasoning is *conceptually* the right object but *operationally* dominated by Rung-2.5 subgroup effects for this decision, and the book would recommend CATE as the deployable workhorse with individual counterfactuals reserved for high-stakes accounts. The divergence metric in Section 9 is exactly the experiment that would settle it.

---

## 14. Still puzzling

The opt-out-rate drift alarm has no external benchmark, and I am not sure what the *right* threshold even is. A small opt-out shift among privacy-conscious academics changes the selection structure differently than the same-sized shift among community physicians, because they sit at different points in the graph. So a single scalar opt-out rate may be too coarse — the alarm might need to watch *who* is opting out, not just *how many*, which means the monitor itself depends on the consent collider's structure (Chapter 13's FCI territory). How to monitor a selection structure you can only partially observe — because the opt-outs are, by definition, the ones you stopped seeing — is the puzzle I would hand to the next cohort.

---

## 15. Summary

The book's title becomes literal here. Three ingredients that had never shared a room — the rep-elicited prior DAG (structure), the identified population effects (magnitude), and the panel (history) — combine into a parameterized structural causal model: each node a structural equation Xᵢ = fᵢ(parents, Uᵢ), each edge tagged with provenance and uncertainty so the model is auditable. The counterfactual engine runs Pearl's abduction → action → prediction: read the specific physician's state by recovering her exogenous noise, surgically set the candidate message, run the modified model forward with her noise carried through, and rank. That individual, Rung-3 recommendation is the payoff no NBA engine produces, because abduction strictly requires an SCM and a predictive model has no noise to recover. The model is *living* because it monitors drift — especially opt-out-rate drift, which changes the selection/bias structure the whole estimate rests on — and because it distinguishes structural change (re-elicit) from parameter drift (refit). Honesty constraints throughout: report bounds where the counterfactual is only partially identified, and constrain the deployable action to the educational pathway. Next, run it on the real projects.

---

## 16. Key terms

- **Structural causal model (SCM)** — a DAG where each node has a structural equation Xᵢ = fᵢ(parents, Uᵢ) with exogenous noise; required for counterfactuals.
- **Exogenous noise (Uᵢ)** — everything affecting a node not captured by its modeled parents; carries the individual particularity abduction recovers.
- **Abduction → action → prediction** — Pearl's three-step counterfactual procedure: infer noise from evidence, apply do(·), run forward with recovered noise.
- **Provenance tag** — the label on each edge (data-estimated / rep-elicited / literature-supported / assumed) that makes the model auditable.
- **Pre-factual vs retrospective counterfactual** — evaluated before acting (condition on pre-action characteristics) vs after (condition on observed outcome, sharper noise recovery).
- **Three granularities** — population ACE / subgroup CATE / individual counterfactual, increasing in value and difficulty.
- **Partial identification** — when noise is not uniquely recoverable and the counterfactual is bounded, not point-identified.
- **Opt-out-rate drift** — shifts in physician consent rates that change the selection/bias structure of every downstream estimate; this book's signature drift alarm.
- **Living Model** — the architecture: parameterized SCM + counterfactual engine + structural-change/drift monitoring.

---

## 17. Bridge

The engine is built: an SCM that reads an individual physician's state and answers "which message next week," wrapped in a monitor that knows when its own answers go stale. But an engine is not a study. WEEK 10 takes the most feasible, highest-value project — the staggered difference-in-differences on message-variant transitions, using the training-cohort rollout as the natural experiment — and runs it end to end, falsification test first, to produce the identified effects this model has been waiting to be parameterized with.

---

## 18. Further reading

- Pearl, J. (2009). *Causality: Models, Reasoning, and Inference*, 2nd ed. Cambridge University Press — structural causal models, the do-operator, and the abduction–action–prediction counterfactual procedure.
- Pearl, J. & Mackenzie, D. (2018). *The Book of Why*. Basic Books — the ladder and the counterfactual rung in narrative form.
- Ness, R. O., Paneri, K. & Vitek, O. (2019). "Integrating Markov Processes with Structural Causal Modeling Enables Counterfactual Inference in Complex Systems." arXiv:1911.02175 (NeurIPS 2019 causal-ML workshop) — casts a Markov process model as an SCM and runs Pearl's three-step counterfactual; directly supports this chapter's spine.
- Bottou, L. et al. (2013). "Counterfactual Reasoning and Learning Systems." arXiv:1209.2355 — counterfactual reasoning for decision systems; reference [7] of the Ness paper.
- Bareinboim, E., Correa, J. D., Ibeling, D. & Icard, T. "On Pearl's Hierarchy and the Foundations of Causal Inference." [verify full citation, 2022] — the Causal Hierarchy Theorem: why abduction strictly requires the SCM.
- "Bayesian Network Structure Discovery Using LLMs." arXiv:2511.00574 — LLM-assisted structure discovery; read with the hallucination caveat for the firewall on parameterization.
