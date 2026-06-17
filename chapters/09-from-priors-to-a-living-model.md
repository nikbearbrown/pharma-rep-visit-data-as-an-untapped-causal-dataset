# Chapter 9 — From Priors to a Living Model
*Why the population average is the beginning of the answer, not the end of it — and what the individual counterfactual requires that no prediction engine has.*

The natural experiment came back clean. The staggered rollout identified an average treatment-on-treated effect of the safety-first message: call it +4 new prescriptions per physician over 90 days. The confidence interval was tight. The falsification test passed. The Fellow wrote it up, the partner was pleased, and the recommendation went out.

Deliver the safety-first message.

Then the brand director asked the question that broke the deliverable.

"Great — so which message do I send Dr. Martinez next week?"

The +4 is a population average. Dr. Martinez is not the population. She received the safety-first message last quarter. Her knowledge of the drug's safety profile ticked up — the educational effect was real. Her prescribing barely moved. Below what the average effect predicts. Is she a non-responder? Formulary-blocked? Someone for whom the efficacy-first message would have been more effective? The population number is silent on all of it, because producing that number required averaging away exactly the case-specific information the director's question turns on.

The failure is not that the IV estimate is wrong. It is right, and it was necessary to get. The failure is mistaking a Rung-2 population effect for a Rung-3 individual decision. The +4 is the background. The recommendation is a counterfactual that has to read Dr. Martinez's specific state first — before it can say anything useful about next week. And the machinery that does that reading is what this chapter builds.

---

Three things have been assembled across this book that have never, in the pharma analytical apparatus, been in the same room.

The first is **structure**: the annotated prior DAG elicited from experienced reps in the previous chapter. Oriented edges, confidence tags, evidence notes — the qualitative skeleton of the causal story about how messages move physicians.

The second is **magnitude**: the identified population effects from the natural experiment. Numbers the quasi-experimental design licenses you to believe — effects that survive a falsification test and come with honest uncertainty bounds.

The third is **history**: the physician-by-time panel from the assembled dataset. Every visit, every reaction score, every prescribing record — the raw material that tells you what this specific physician has done.

Structure from the reps. Magnitude from the experiment. History from the panel. A **structural causal model** is where they meet. A DAG becomes an SCM when each node in the graph receives a *structural equation* — a functional relationship specifying what the node's value is in terms of its parents and an exogenous noise term:

$$X_i = f_i(\text{parents}(X_i),\ U_i)$$

The $U_i$ term is everything affecting $X_i$ that the modeled parents do not capture — the residual particularity of each unit. For physician prescribing, it is everything about Dr. Martinez that the structural equation with population parameters does not explain: her idiosyncratic responsiveness, her formulary situation, her relationship with her rep, everything that makes her different from the panel average.

That noise term is the key. Everything that follows depends on it.

<!-- → [DIAGRAM: Three-ingredient assembly diagram. Three boxes on the left: "Elicited Prior DAG (Chapter 8) — Structure: oriented edges, confidence tags," "Identified Effects (Chapters 4, 10, 11) — Magnitude: IV/DiD/causal forest estimates," "Physician Panel (Chapter 2) — History: visit records, NRx, reactions." All three boxes point into a central box: "Structural Causal Model — DAG + Structural Equations + Provenance Tags." From the SCM, two outputs: "Counterfactual Engine (individual recommendations)" and "Drift Monitor (structural-change alarms)." Caption: "Structure from the reps. Magnitude from the experiment. History from the panel. The SCM is where they meet."] -->

---

The discipline that makes the model honest is **provenance**. Every edge carries a tag that says where it came from:

- **data-estimated** — the magnitude came from the panel and an identified quasi-experimental effect
- **rep-elicited** — the orientation came from a named rep observation in the elicitation session, at a specified confidence level
- **literature-supported** — a published causal estimate supplied the prior
- **assumed** — you had nothing and made a modeling choice

That last tag is the most important. List every assumed edge explicitly. That list is the model's exposed underbelly — the assumptions a critic can challenge, the parameters a sensitivity sweep should vary. Where reps contradicted each other in the elicitation, do not force a single resolution; set up the contested edge as a *sensitivity dimension* and report how much the downstream recommendation moves as you vary it. A model that hides its assumed edges is a vendor DAG with extra steps.

Provenance makes the model auditable. Every edge orientation traces to data, an identified effect, or a named human observation. Where edges remain genuinely unresolved — where the data cannot distinguish between two orientations and the reps offered contradictory accounts — the honest output is sensitivity analysis over the surviving equivalence class, not a false commitment to one graph.

<!-- → [TABLE: Edge provenance types. Columns: provenance type, source, confidence level, what a breach triggers. Rows: data-estimated (identified quasi-experimental effect; interval from estimate; refit if magnitude drifts outside bound), rep-elicited (named rep observation with confidence tag; subjective; re-elicit if rep knowledge ages or territory turns over), literature-supported (published causal estimate; per-paper confidence; update when new evidence appears), assumed (no source; explicitly low; sensitivity sweep across plausible range; flag for partner review). Caption: "Every edge gets a provenance tag. The assumed edges, listed explicitly, are the model's exposed underbelly."] -->

---

Now the engine.

Pearl's three-step counterfactual procedure is the heart of the chapter and the thing no next-best-action engine can do. It goes by the names **abduction, action, prediction**, and each step does something specific.

**Step 1 — Abduction.** Take the observed evidence for a specific physician — her visit history, the rep's read of the call, her prior prescribing record — and infer the exogenous noise: compute $P(U \mid \text{evidence})$. This is where individual particularity enters the model. The $U_i$ terms are what distinguish Dr. Martinez from the panel average, and abduction recovers them from the evidence of her actual behavior. She received the safety message. Her knowledge moved modestly. Her prescribing moved less than knowledge would predict from the population parameters. That pattern implies specific values of $U_K$ and $U_{Rx}$ — residuals that encode something about her that the structural equations with population parameters do not capture.

**Step 2 — Action.** Apply $do(\text{message} = m')$: surgically set the message to the candidate, replacing only that one structural equation and leaving every other mechanism intact. The do-operator is not a global surgery on the model. It changes one equation. The rest of the causal machinery still runs as before.

**Step 3 — Prediction.** Run the modified SCM forward, carrying the **recovered** $U_i$ from Step 1, to compute counterfactual prescribing under each candidate message. Rank the candidates. That ranking is the recommendation.

The reason this beats the population number is Step 1. A propensity engine or a next-best-action ranker has no $U_i$ to recover — it has no structural model, so it cannot read a specific physician's state. It can only return the average prediction. Abduction is the "read the current conditions before you simulate" step, and it is precisely the difference between a weather model that ingests today's atmospheric pressure and temperature and a model that simply returns the seasonal normal. The seasonal normal is informative. It is not what you use to decide whether to carry an umbrella tomorrow.

This counterfactual is **pre-factual**: it is evaluated before acting, conditioning on the physician's pre-action characteristics. It is distinct from the *retrospective* counterfactual the rep was asked to reason about in the elicitation session — "would that account have converted without the MSL visit?" — which conditions on the observed outcome and can therefore recover the noise more sharply. Same three steps. Different conditioning set.

<!-- → [DIAGRAM: Abduction-action-prediction procedure for Dr. Martinez. Three panels arranged horizontally. Panel 1 (Abduction): DAG with observed evidence highlighted — message delivered, knowledge lift observed, NRx observed. Arrow labeled "recover U_K, U_Rx from evidence." Panel 2 (Action): same DAG with do(message = efficacy-first) shown as a box replacing the message node's usual equation. Arrow: "replace message equation, leave Knowledge and NRx equations intact." Panel 3 (Prediction): DAG running forward with recovered U_K and U_Rx, producing counterfactual NRx for each candidate message. Output: ranked recommendations. Caption: "Step 1 reads her state. Step 2 sets the candidate. Step 3 predicts her response, carrying her specificity through."] -->

---

The three steps produce a granularity of inference no other component of the analytical stack reaches. There are three levels, and it is worth naming them precisely because they are often conflated.

**Population ACE** — the average causal effect — requires no abduction. It is the Rung-2 quantity the IV estimate produces: the +4 of the opening case. It is what happens on average when you assign the safety-first message to the population. Necessary. Not sufficient for the decision.

**Subgroup CATE** — the conditional average treatment effect — conditions on observed characteristics. A causal forest splits the population by specialty, baseline prescribing level, competitive environment, and returns effect estimates within cells. This is Rung-2 with more resolution: the effect for high-baseline cardiologists in the Northeast is different from the effect for low-baseline internists in the Midwest. Useful. Still not the individual.

**Individual counterfactual** — the Rung-3 object — conditions on the specific physician's history and recovers her noise before predicting. This is what answers the brand director's question. It is also the only object in this stack that is genuinely case-specific rather than a population statistic applied to a cell.

The research finding for Fellows lives here: the Rung-3 individual recommendation is the operational payoff no next-best-action engine produces. NBA engines stop at Rung-1 propensity — they predict who is likely to prescribe — and even a sophisticated one that reaches a good subgroup estimate is still a Rung-2 average applied to a categorical. The Living Model's individual recommendation is a categorically different object, and its categorical difference is abduction: the step that reads before it simulates.

---

Work through the machinery on a concrete case to see what abduction actually buys.

Simplify the SCM to three structural equations:

$$\text{Message} = U_M$$
$$\text{Knowledge} = \alpha \cdot \text{Message} + U_K$$
$$\text{NRx} = \beta \cdot \text{Knowledge} + U_{Rx}$$

The parameters $\alpha$ and $\beta$ are estimated from the identified effects — the natural experiment supplies the magnitudes; provenance: data-estimated. The $U$ terms are the physician-specific noise the abduction step recovers.

Dr. Martinez received the safety-first message. Her knowledge of the drug's safety profile moved — but modestly, less than $\alpha$ predicts. Her prescribing moved less still, below what $\beta$ applied to her knowledge would project. That pattern implies specific values of $U_K$ and $U_{Rx}$: she under-responds on knowledge given the message, and under-converts knowledge into scripts given what she knows.

What the rep said in the Chapter 8 elicitation session: "She gets it, but she's formulary-blocked." The abduction recovers numerically what the rep observed qualitatively. That convergence — the structural equation and the rep interview pointing at the same underlying state — is not guaranteed by the method. It is a sign the model is coherent. When they diverge, you have a genuine diagnostic: either the rep's read is wrong, the structural equation is misspecified, or something in the physician's situation has changed since the elicitation.

Now apply $do(\text{message} = \text{efficacy-first})$. Replace the Message equation with the candidate. Leave Knowledge and NRx intact. Run forward with her recovered $U_K$ and $U_{Rx}$. Her knowledge-to-NRx conversion is still blocked by formulary — that is encoded in her $U_{Rx}$, which the abduction recovered and which carries forward into the prediction. The efficacy-first message may produce better knowledge uptake than safety-first for someone who already knows the safety profile, but if formulary is the binding constraint, neither message lifts NRx much. The model now has something to say: route her to an access-education message — something that addresses the formulary barrier — rather than cycling through safety and efficacy content that runs into the same wall.

A no-abduction prediction would use population-average noise and return the same recommendation for every physician with her observed message and knowledge triplet. Abduction is the entire difference between "deliver safety-first to everyone" and "Dr. Martinez is formulary-blocked, so literacy-building messages are probably wasted on her — she needs access help first."

The limit, stated honestly. If the SCM is nonlinear or the noise is not uniquely recoverable from the evidence, the individual counterfactual is only **partially identified** — bounded, not pinned to a single value. A model that emits a confident point estimate where only bounds exist is producing a false precision. The honest output is a range, the assumption it depends on, and a statement of what additional evidence would tighten the bound.

---

An SCM is not living unless something watches for the world shifting underneath it.

The drift monitor is the third component of the architecture, and it has two distinct jobs that are easy to conflate. **Parameter drift** is when the magnitudes change — $\alpha$ or $\beta$ moves because the market has changed, the competitive landscape has shifted, or the population of physicians has evolved. The response to parameter drift is refit: re-estimate the structural equations from new data. **Structural drift** is when the causal *mechanism* changes — an edge flips sign, a pathway severs, a new pathway opens. The response to structural drift is re-elicitation or a fresh FCI run. Triggering a refit on structural change is worse than doing nothing, because it fits the old graph to new data and produces parameters that are wrong by construction.

The monitor must distinguish these. It must watch for signals that indicate mechanism change — a formulary shift, a loss of exclusivity, a label update — rather than simply triggering a retrain whenever recent data diverges from predictions.

This book's signature drift target is **opt-out-rate drift**. Recall from Chapter 6 that physician consent is a selection collider: the consenting sample is not a random sample from the prescriber population, because the decision to consent is correlated with both engagement and prescribing propensity. The entire SCM — its structural equations, its population parameters, its individual counterfactuals — is estimated from and applied to the consenting subsample. The validity of those estimates for the full physician population depends on an assumption about the selection structure. When the opt-out rate shifts — more physicians declining to have their visits logged — the composition of the consenting sample changes, and the selection structure changes with it. The bias structure of every downstream estimate shifts, even if nothing about the underlying causal mechanisms has changed.

That is why the opt-out alarm is architectural rather than optional. It is not a generic covariate drift check. It is specifically monitoring the variable whose movement changes the structural validity of the model's estimates. Specify it concretely: a defined threshold — say, a 10-percentage-point shift in the opt-out rate within any 90-day window — triggers a review of the consent structure before anything else. Not a refit. A review of whether the selection mechanism has changed enough that the model's estimates no longer transport to the full population.

And more specifically: not just *how many* physicians are opting out, but *who*. A shift in opt-out rates among academic physicians changes the selection structure differently from the same-sized shift among community physicians, because they sit at different positions in the causal graph. The scalar opt-out rate is a coarse alarm; the monitor should watch the composition of opt-outs as well as the rate.

<!-- → [DIAGRAM: Living Model architecture. Three components in a horizontal chain. Left box: "Parameterized SCM — DAG + structural equations + provenance tags." Center box: "Counterfactual Engine — Abduction → Action → Prediction — individual Rung-3 recommendations." Right box: "Drift Monitor — Parameter drift (refit) | Structural drift (re-elicit / FCI re-spec) | Opt-out-rate alarm (selection structure review)." Arrows connecting left to center (feeds equations to engine) and right to left (triggers model updates). Caption: "The SCM is the mechanism. The engine runs counterfactuals on it. The monitor watches for the world changing underneath both."] -->

One more constraint on the action step. The counterfactual engine can rank messages across all pathways — including messages that work through relationship maintenance or reciprocity rather than through education. It is analytically informative to understand those effects. It is not legally deployable to recommend them. Constrain the action step in any deployable recommendation to the educational pathway. Messages that work through physician learning — accurate clinical information about efficacy, safety, patient selection — are the regulatorily defensible channel. Counterfactuals estimated over the other pathways are for analysis, labeled analysis-only, and kept away from the recommendation layer.

---

The model is now assembled. Three ingredients that had never shared a room in pharma analytics — structure, magnitude, history — are combined into a parameterized SCM. The counterfactual engine reads each physician's specific state before simulating next week, returning a Rung-3 individual recommendation that no propensity ranker or NBA engine produces. The drift monitor watches for the world changing underneath the model, distinguishing parameter drift from structural change, and watching opt-out-rate movement specifically because it changes the selection structure on which every estimate depends.

What this architecture does not do is prove that abduction pays off operationally. That is an empirical question, and it is the right question to ask. On the synthetic dataset with known ground-truth noise, you can measure how often the individual recommendation diverges from the population recommendation — and how often that divergence flips the chosen message. If the flip rate is near zero, the full abduction machinery adds little over a well-calibrated subgroup effect, and the honest recommendation is to use the causal forest and save the SCM for high-stakes individual accounts. If the flip rate is substantial, the individual counterfactual is doing real work, and the machinery is worth the investment.

Run that measurement. Report the flip rate. The answer belongs in the deliverable.

**Five-part AI exercise block**

**When to use AI here.** Use an LLM to scaffold the SCM code — the structural-equation skeleton, the abduction loop, the action and prediction steps, the sensitivity sweep over unresolved edges. These are engineering and bookkeeping tasks where the model accelerates you without supplying causal content. Use it also to draft the drift-monitor specification from your stated mechanism.

**When NOT to use AI here.** Never let the LLM invent structural equations, edge magnitudes, or parameter values. Those come from the identified effects and the data. An LLM that "estimates" $\alpha$ or $\beta$ from its training prior, or proposes a structural equation you did not derive from the quasi-experiment, is hallucinating the model's content. The firewall from the previous chapter holds here at every layer: the LLM structures and codes; data and experiments supply the numbers; reps supply the orientations.

**LLM exercise (copy-paste prompt):**
> "You are scaffolding code for a structural causal model. Do NOT supply any parameter values, effect sizes, or structural-equation coefficients from your own knowledge — leave every numeric parameter as a named, unfilled variable I will fill from my identified effects and data.
>
> Given this DAG (nodes and oriented edges with provenance tags pasted below), write Python that: (1) defines each node's structural equation $X_i = f_i(\text{parents}, U_i)$ with all coefficients as named, unfilled parameters; (2) implements abduction — given observed (message, knowledge, NRx) for one physician, recovers the exogenous noise $U$ for that physician; (3) implements action — do(message = candidate), replacing only that equation; (4) implements prediction — runs forward with the recovered $U$, returns counterfactual NRx per candidate; (5) flags in a comment any place where the counterfactual is only partially identified and should be reported as bounds.
>
> DAG (with provenance tags): [paste your annotated DAG here]"

**CLI exercise.** Run the scaffolded SCM on the synthetic dataset, looping the counterfactual over every physician, and write a CSV of (population recommendation, individual recommendation) per physician. Then count how often the individual recommendation differs from the population one — your "what abduction buys" metric. Re-run after perturbing the opt-out rate in the synthetic sampling frame to show the recommendations shifting, making the drift mechanism visible in numbers.

**AI validation.** Before trusting any real run, validate on the synthetic dataset with known ground-truth effects. Does the SCM, parameterized from the identified effects, recover the known $\alpha$ and $\beta$? Does abduction recover the injected $U$ for a physician whose true noise was set by the data-generating process? If the model cannot recover known ground truth on synthetic data, it cannot be trusted on real data. Validate recovery first; deploy second.

## AI Use Disclosure

*Write two sentences naming what an AI tool did in your work for this chapter and the one judgment it could not make. For example: "I used an LLM to scaffold the abduction-action-prediction code and to draft the drift-monitor specification; I decided myself what parameter values to assign to each structural equation, because those come from the identified quasi-experimental effects, and a model that supplies its own estimates for those parameters is hallucinating the causal content the chapter is designed to protect."*

## What Would Change My Mind

The chapter's strong claim is that the individual counterfactual is worth the machinery — that abduction buys something a good predictive model cannot. I would weaken it if, on the synthetic dataset and then on real partner data, the individual recommendation almost always agreed with a well-calibrated subgroup (CATE) estimate — if the flip rate was near zero, meaning the full abduction rarely changed the chosen message relative to a causal forest split by a few covariates. Then the honest position would be that Rung-3 individual reasoning is conceptually the right object but operationally dominated by Rung-2.5 subgroup effects for this decision, and the recommendation would shift: use CATE as the deployable workhorse, reserve individual counterfactuals for high-stakes accounts. The divergence metric in the CLI exercise is exactly the experiment that would settle it.

## Still Puzzling

The opt-out-rate drift alarm has no external benchmark, and the right threshold is genuinely unclear. A shift among academic physicians changes the selection structure differently from the same shift among community physicians, because they occupy different positions in the causal graph. A single scalar threshold may be too coarse — the alarm might need to watch who is opting out, not just how many, which means the monitor itself depends on the consent collider's structure that this framework can only partially observe. How to monitor a selection structure whose key feature is that you stopped seeing the people who most affect it — the opt-outs, by definition, are the ones who disappeared — is the puzzle I would pass to the next cohort of Fellows.

---

## Exercises

**Warm-up**

1. *(Recall — low difficulty) What this tests: whether you can state the three steps of the counterfactual procedure and what each does.* In your own words, describe what abduction, action, and prediction each accomplish in Pearl's three-step procedure. Then explain in one sentence why Step 1 is the step that no propensity ranker or NBA engine can perform, and what structural feature it requires.

2. *(Recall — low difficulty) What this tests: whether you can distinguish the three granularities.* Name the three granularities of causal inference (population ACE, subgroup CATE, individual counterfactual), state which rung of Pearl's ladder each occupies, and state in one sentence what additional ingredient moves you from each level to the next. Then explain why the +4 average effect in the opening case is necessary but not sufficient for the brand director's question.

3. *(Recall — low difficulty) What this tests: whether you understand the distinction between parameter drift and structural drift.* Explain the difference between parameter drift and structural drift in one sentence each. Name one event in the pharma market that would constitute parameter drift and one that would constitute structural drift, and state what the correct response is to each.

**Application**

4. *(Apply — medium difficulty) What this tests: running the three-step procedure by hand.* Given the three-equation SCM in Section 5 and one synthetic physician with observed (message = safety-first, knowledge lift = 0.3, NRx lift = 0.1), with population parameters $\alpha = 0.6$ and $\beta = 0.5$: (a) recover her noise terms $U_K$ and $U_{Rx}$ via abduction; (b) apply do(message = efficacy-first) and run forward with her recovered noise to compute counterfactual NRx; (c) compute the no-abduction population prediction for the same candidate message; (d) state what the difference between (b) and (c) reveals about this physician.

5. *(Apply — medium difficulty) What this tests: provenance tagging and sensitivity analysis.* Take the three-equation SCM and assign a provenance tag to each structural equation — one data-estimated, one rep-elicited at a stated confidence level, and one assumed. For the assumed edge, specify a plausible range for the parameter and run a sensitivity sweep reporting how much the recommendation for the physician in Exercise 4 changes across the range. State whether the recommendation is robust to the assumption or sensitive to it.

6. *(Apply — medium difficulty) What this tests: specifying the opt-out-rate drift alarm.* Design the opt-out-rate drift monitor for your Living Model prototype: choose a threshold (percentage-point shift) and window (days), justify them against the selection-bias mechanism from Chapter 6, and specify the decision tree for what a breach triggers — refit parameters, re-elicit the prior, or re-run FCI for the consent structure. Then explain why this alarm is distinct from a generic covariate-drift check and why a scalar opt-out rate may be insufficient.

**Synthesis**

7. *(Synthesis — high difficulty) What this tests: building the parameterized SCM artifact.* Starting from your Chapter 8 annotated prior DAG, parameterize the SCM: write a structural equation for each node, assign a magnitude to each edge from an identified effect where you have one, and tag every edge with its provenance type. List every assumed edge explicitly with a plausible range for sensitivity analysis. Where reps contradicted each other in the elicitation session, set up the contested edge as a sensitivity dimension. Then run a complete abduction-action-prediction counterfactual on one synthetic physician and report: the individual recommendation, the no-abduction population prediction, whether the recommendation is point-identified or bounded, and — if bounded — what assumption would tighten the bound.

8. *(Synthesis — high difficulty) What this tests: measuring what abduction buys.* On the synthetic dataset with planted ground-truth noise, loop the counterfactual over all physicians and compute: (a) the population recommendation (no abduction) for each physician, (b) the individual recommendation (with abduction) for each physician, and (c) the flip rate — the fraction of physicians for whom the individual recommendation differs from the population recommendation. Report the flip rate and interpret it: does abduction buy a substantial operational payoff on this synthetic dataset, or does it mostly agree with the population recommendation? State what this result implies for when to invest in the full SCM versus settling for a causal forest.

**Challenge**

9. *(Challenge — open-ended) What this tests: partial identification and honest reporting.* Design a scenario in the rep-visit context where the individual counterfactual would be only partially identified — where abduction cannot uniquely recover the physician's noise terms and the prediction is bounded rather than point-identified. Specify: what feature of the SCM or the evidence produces the partial identification, what the bound looks like (an interval, a worst-case and best-case), what additional evidence would tighten the bound, and how you would report this result to a partner who wants a single recommendation. Then state what the compliance implication is if a model reports a confident point recommendation in a case that is only partially identified.
