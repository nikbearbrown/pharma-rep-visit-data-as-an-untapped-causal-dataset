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

![Structure from the reps, magnitude from the experiment, history from the panel. The structural causal model is where they meet, feeding a counterfactual engine and a drift monitor.](images/09-from-priors-to-a-living-model-fig-01.png)
*Figure 9.1 — Three-ingredient assembly into the SCM*

<!-- → [DIAGRAM: Three-ingredient assembly diagram. Three boxes on the left: "Elicited Prior DAG (Chapter 8) — Structure: oriented edges, confidence tags," "Identified Effects (Chapters 4, 10, 11) — Magnitude: IV/DiD/causal forest estimates," "Physician Panel (Chapter 2) — History: visit records, NRx, reactions." All three boxes point into a central box: "Structural Causal Model — DAG + Structural Equations + Provenance Tags." From the SCM, two outputs: "Counterfactual Engine (individual recommendations)" and "Drift Monitor (structural-change alarms)." Caption: "Structure from the reps. Magnitude from the experiment. History from the panel. The SCM is where they meet."] -->

---

The discipline that makes the model honest is **provenance**. Every edge carries a tag that says where it came from:

- **data-estimated** — the magnitude came from the panel and an identified quasi-experimental effect
- **rep-elicited** — the orientation came from a named rep observation in the elicitation session, at a specified confidence level
- **literature-supported** — a published causal estimate supplied the prior
- **assumed** — you had nothing and made a modeling choice

That last tag is the most important. List every assumed edge explicitly. That list is the model's exposed underbelly — the assumptions a critic can challenge, the parameters a sensitivity sweep should vary. Where reps contradicted each other in the elicitation, do not force a single resolution; set up the contested edge as a *sensitivity dimension* and report how much the downstream recommendation moves as you vary it. A model that hides its assumed edges is a vendor DAG with extra steps.

Provenance makes the model auditable. Every edge orientation traces to data, an identified effect, or a named human observation. Where edges remain genuinely unresolved — where the data cannot distinguish between two orientations and the reps offered contradictory accounts — the honest output is sensitivity analysis over the surviving equivalence class, not a false commitment to one graph.

| Provenance type | Source | Confidence level | What a breach triggers |
| --- | --- | --- | --- |
| data-estimated | identified quasi-experimental effect | interval from estimate | refit if magnitude drifts outside bound |
| rep-elicited | named rep observation with confidence tag | subjective | re-elicit if rep knowledge ages or territory turns over |
| literature-supported | published causal estimate | per-paper confidence | update when new evidence appears |
| assumed | no source | explicitly low | sensitivity sweep across plausible range; flag for partner review |

*Table 9.1 — Edge provenance types. The assumed edges, listed explicitly, are the model's exposed underbelly.*

---

Now the engine.

Pearl's three-step counterfactual procedure is the heart of the chapter and the thing no next-best-action engine can do. It goes by the names **abduction, action, prediction**, and each step does something specific.

**Step 1 — Abduction.** Take the observed evidence for a specific physician — her visit history, the rep's read of the call, her prior prescribing record — and infer the exogenous noise: compute $P(U \mid \text{evidence})$. This is where individual particularity enters the model. The $U_i$ terms are what distinguish Dr. Martinez from the panel average, and abduction recovers them from the evidence of her actual behavior. She received the safety message. Her knowledge moved modestly. Her prescribing moved less than knowledge would predict from the population parameters. That pattern implies specific values of $U_K$ and $U_{Rx}$ — residuals that encode something about her that the structural equations with population parameters do not capture.

**Step 2 — Action.** Apply $do(\text{message} = m')$: surgically set the message to the candidate, replacing only that one structural equation and leaving every other mechanism intact. The do-operator is not a global surgery on the model. It changes one equation. The rest of the causal machinery still runs as before.

**Step 3 — Prediction.** Run the modified SCM forward, carrying the **recovered** $U_i$ from Step 1, to compute counterfactual prescribing under each candidate message. Rank the candidates. That ranking is the recommendation.

The reason this beats the population number is Step 1. A propensity engine or a next-best-action ranker has no $U_i$ to recover — it has no structural model, so it cannot read a specific physician's state. It can only return the average prediction. Abduction is the "read the current conditions before you simulate" step, and it is precisely the difference between a weather model that ingests today's atmospheric pressure and temperature and a model that simply returns the seasonal normal. The seasonal normal is informative. It is not what you use to decide whether to carry an umbrella tomorrow.

This counterfactual is **pre-factual**: it is evaluated before acting, conditioning on the physician's pre-action characteristics. It is distinct from the *retrospective* counterfactual the rep was asked to reason about in the elicitation session — "would that account have converted without the MSL visit?" — which conditions on the observed outcome and can therefore recover the noise more sharply. Same three steps. Different conditioning set.

![Step 1 reads her state (abduction recovers U_K, U_Rx); Step 2 sets the candidate message (action, do-operator on one equation); Step 3 predicts her response carrying her specificity through.](images/09-from-priors-to-a-living-model-fig-02.png)
*Figure 9.2 — Abduction, action, prediction. The individual counterfactual is often only partially identified, so the output is a ranking or bounds, not a point estimate.*

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

![The parameterized SCM is the mechanism; the counterfactual engine runs abduction-action-prediction on it; the drift monitor watches for parameter drift, structural drift, and opt-out-rate shifts.](images/09-from-priors-to-a-living-model-fig-03.png)
*Figure 9.3 — The Living Model architecture. The opt-out-rate alarm is a selection-collider safeguard, watching the variable whose movement changes the model's structural validity.*

<!-- → [DIAGRAM: Living Model architecture. Three components in a horizontal chain. Left box: "Parameterized SCM — DAG + structural equations + provenance tags." Center box: "Counterfactual Engine — Abduction → Action → Prediction — individual Rung-3 recommendations." Right box: "Drift Monitor — Parameter drift (refit) | Structural drift (re-elicit / FCI re-spec) | Opt-out-rate alarm (selection structure review)." Arrows connecting left to center (feeds equations to engine) and right to left (triggers model updates). Caption: "The SCM is the mechanism. The engine runs counterfactuals on it. The monitor watches for the world changing underneath both."] -->

One more constraint on the action step. The counterfactual engine can rank messages across all pathways — including messages that work through relationship maintenance or reciprocity rather than through education. It is analytically informative to understand those effects. It is not legally deployable to recommend them. Constrain the action step in any deployable recommendation to the educational pathway. Messages that work through physician learning — accurate clinical information about efficacy, safety, patient selection — are the regulatorily defensible channel. Counterfactuals estimated over the other pathways are for analysis, labeled analysis-only, and kept away from the recommendation layer.

---

The model is now assembled. Three ingredients that had never shared a room in pharma analytics — structure, magnitude, history — are combined into a parameterized SCM. The counterfactual engine reads each physician's specific state before simulating next week, returning a Rung-3 individual recommendation that no propensity ranker or NBA engine produces. The drift monitor watches for the world changing underneath the model, distinguishing parameter drift from structural change, and watching opt-out-rate movement specifically because it changes the selection structure on which every estimate depends.

What this architecture does not do is prove that abduction pays off operationally. That is an empirical question, and it is the right question to ask. On the synthetic dataset with known ground-truth noise, you can measure how often the individual recommendation diverges from the population recommendation — and how often that divergence flips the chosen message. If the flip rate is near zero, the full abduction machinery adds little over a well-calibrated subgroup effect, and the honest recommendation is to use the causal forest and save the SCM for high-stakes individual accounts. If the flip rate is substantial, the individual counterfactual is doing real work, and the machinery is worth the investment.

Run that measurement. Report the flip rate. The answer belongs in the deliverable.

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

## Prompts

### Figure 9.1 — Three-ingredient assembly into the SCM

Build a left-to-right box-and-arrow assembly diagram on a 700x420 viewBox. Data: three input boxes (STRUCTURE / oriented prior DAG; MAGNITUDE / IV-DiD estimates; HISTORY / physician panel) stacked on the left; one central SCM box (DAG + equations + provenance) drawn in red as the assembled object; two output boxes on the right (COUNTERFACTUAL engine; DRIFT MONITOR). Marks: rounded-free rects for boxes; two-line text labels (Inter for titles, JetBrains Mono for sublabels); straight lines with a drawn triangular marker arrowhead. Channels: position encodes role (inputs left, mechanism center, outputs right); red stroke encodes the central assembled object. Layout: three convergence arrows from inputs into the SCM, two fan-out arrows from the SCM to the outputs. Annotations: title, three-line caption, source line. Boxes are interactive (tabindex, aria-label, hover/focus tooltip). Deliverable: single self-contained HTML, inline CSS with light/dark CSS variables, D3 7.9.0 from cdnjs, no hardcoded hex. Box labels and arrow connections must match the static SVG. Under 200 words.

### Figure 9.2 — Abduction, action, prediction

Build a three-panel horizontal procedure diagram on a 700x440 viewBox. Data: the same three-node causal graph shown in three states. Panel 1 ABDUCTION — graph with two red noise circles and red back-inference arrows ("recover U_K, U_Rx"). Panel 2 ACTION — same graph with a red `do` intervention box on the top node and a dashed severed upstream stub, other edges intact ("replace one equation"). Panel 3 PREDICTION — forward run carrying a red noise circle into a stacked ranked-list of three rects ("rank candidates"). Marks: circles for nodes, rects for panels and ranked list, lines with two marker arrowheads (ink + red). Channels: panel position encodes step order; red encodes recovered noise and intervention. Annotations: panel labels, title, caption stating output is a ranking/bounds (partially identified), source line. Panels interactive (tabindex, aria-label, tooltip). Deliverable: single self-contained HTML, inline CSS with light/dark CSS variables, D3 7.9.0 from cdnjs, no hardcoded hex. Data must match the static SVG. Under 200 words.

### Figure 9.3 — The Living Model architecture

Build a horizontal architecture chain on a 700x420 viewBox. Data: three boxes left-to-right — PARAMETERIZED SCM (DAG + equations + provenance), ENGINE (abduction-action-prediction), DRIFT MONITOR holding three stacked bands (parameter drift -> refit; structural -> re-elicit; opt-out alarm in red with a red dot marker). Marks: rects for boxes and bands, two-line text labels, straight connector lines with a drawn arrowhead. Channels: position encodes pipeline order; red encodes the signature opt-out-rate alarm. Layout: forward arrows SCM->engine->monitor, plus a feedback arrow that drops from the monitor, runs right-to-left along the bottom, and turns up into the SCM ("detected drift triggers a model update"). Annotations: title, multi-line caption explaining the selection-collider opt-out alarm and the closed loop, source line. Boxes and bands interactive (tabindex, aria-label, tooltip). Deliverable: single self-contained HTML, inline CSS with light/dark CSS variables, D3 7.9.0 from cdnjs, no hardcoded hex. Labels, the three bands, and the feedback arrow must match the static SVG. Under 200 words.

---

## Chapter 9 Exercises: From Priors to a Living Model

**Project:** The Causal Interview Bot
**This chapter adds:** Wiring the bot's elicited prior into a parameterized SCM — every edge gets a structural equation and a provenance tag, so the bot's output supports individual (Rung-3) counterfactuals and the opt-out drift check.

### Exercise 1 — When to Use AI

The bot has handed you an annotated prior DAG; this chapter turns it into running code. AI is the right tool for the engineering scaffold — never for the numbers. Two places it earns its keep:

- **Scaffold the SCM code** — the structural-equation skeleton, the abduction loop, the action step (do-operator on one equation), the prediction run, and the sensitivity sweep over contested edges. *Why AI works here:* (code generation against a spec) — these are mechanical implementations of a fixed three-step procedure, and you can run them on synthetic ground truth to confirm they recover known parameters.
- **Draft the drift-monitor specification** from your stated mechanism — parameter drift vs structural drift vs the opt-out alarm. *Why AI works here:* (translation of a stated rule into a spec) — you supply the mechanism and thresholds; the model formats the decision tree, which you check against the chapter.

**The tell:** you can independently evaluate the output — the scaffolded code is verifiable by running it against synthetic data with planted $\alpha$, $\beta$, and noise, and the drift spec is checkable line-by-line against your stated mechanism.

### Exercise 2 — When NOT to Use AI

- **Never let the LLM supply parameter values, effect sizes, or structural-equation coefficients.** *Why AI fails here:* (causal-ID + ground truth) — $\alpha$ and $\beta$ come from the identified quasi-experimental effects and the data; a model that "estimates" them from its training prior is hallucinating the causal content the whole architecture exists to protect.
- **Never let the model decide the action step may recommend a reciprocity-pathway message, or report a confident point estimate where the counterfactual is only partially identified.** *Why AI fails here:* (values + causal-ID) — constraining the action space to the educational pathway is a compliance/values call, and claiming point identification where only bounds exist is a structural error the model will state fluently.

**The tell:** AI as reason vs tool — if the *reason* a structural equation has the coefficient it does is "the model filled it in," the SCM's content is invented. The identified effect is the reason for the magnitude; the rep observation is the reason for the orientation; the LLM is the tool that codes and books the scaffold around them.

**Series connection:** tier **T5 (Causal)** — assigning magnitudes to edges and reading a physician's individual state via abduction is irreducible causal-identification work, and **T7 (Compliance/Values)** for the action-space constraint and the partial-identification honesty, both of which decide what the model is permitted to claim and recommend.

### Exercise 3 — LLM Exercise

**What you're building:** The counterfactual engine that consumes the bot's annotated prior DAG — code that parameterizes each edge (coefficients left unfilled), runs abduction-action-prediction for one physician, and flags partial identification.

**Tool:** Claude, as a **Claude Project** — the project holds the SCM spec, the provenance vocabulary, and the firewall (no model-supplied numbers) as persistent context, so the bot's DAG and the engine code stay under one consistent discipline across iterations.

**The Prompt:**

```
You are scaffolding code for a structural causal model that consumes an
elicited prior DAG from an expert-elicitation bot. Do NOT supply any
parameter values, effect sizes, or coefficients from your own knowledge —
leave EVERY numeric parameter as a named, unfilled variable I will fill from
my identified quasi-experimental effects and data.

Given this annotated prior DAG (oriented edges with provenance tags), write
Python that:
1. Defines each node's structural equation X_i = f_i(parents, U_i) with all
   coefficients as named, unfilled parameters (e.g. ALPHA, BETA).
2. Implements ABDUCTION — given observed (message, knowledge, NRx) for one
   physician, recovers her exogenous noise U.
3. Implements ACTION — do(message = candidate), replacing ONLY that equation,
   leaving every other mechanism intact.
4. Implements PREDICTION — runs forward with the recovered U, returns
   counterfactual NRx per candidate message, and RANKS them.
5. Constrains the candidate set to EDUCATIONAL-pathway messages for any
   deployable recommendation; label reciprocity/relationship-pathway
   counterfactuals "analysis-only".
6. Flags in a comment any place where the counterfactual is only PARTIALLY
   IDENTIFIED and should be reported as bounds, not a point estimate.

Annotated prior DAG (with provenance tags):
FORMULARY --> PRESCRIBING        [rep-elicited, high]   "couldn't write it
                                  until formulary cleared"
SAFETY_EDU --> CONFIDENCE        [rep-elicited, high]   "gave her the
                                  language to defend it"
CONFIDENCE --> PRESCRIBING       [data-estimated]       beta from IV rollout
MESSAGE   --> SAFETY_EDU         [data-estimated]       alpha from IV rollout
PEER --> PRESCRIBING (Dr. K)     [assumed, contested]   reps disagree on
                                  orientation
```

**What this produces:** Runnable SCM scaffolding with every coefficient as a named blank you fill from your identified effects, an abduction-action-prediction loop, an educational-pathway constraint on the action step, and an explicit partial-identification flag on the contested `PEER → PRESCRIBING` edge.

**How to adapt:** paste your own Chapter 8 bot output as the DAG; on **ChatGPT or Gemini**, prepend the no-numbers firewall to every prompt; in a **Claude Project**, store the SCM spec and provenance vocabulary once.

**Connection to previous chapters:** Structure and provenance come from the Chapter 8 bot; magnitudes come from the natural-experiment chapters; the contested edge is a Chapter 7 reversible edge the reps could not settle, now a sensitivity dimension.

**Preview of next chapter:** The flip-rate metric you measure here — how often abduction changes the chosen message — sets up the deployment question: when is the full SCM worth it versus a cheaper subgroup (CATE) estimate.

### Exercise 4 — CLI Exercise

**What you're building:** The full Living Model loop on synthetic data — parameterized SCM, individual counterfactuals over every physician, the "what abduction buys" flip-rate metric, and the opt-out drift demonstration.

**Tool:** Claude Code — because this is a multi-file pipeline (SCM, counterfactual loop, perturbation harness, report) you iterate on. **Skill level:** intermediate-to-advanced (comfortable with structural equations and a CSV pipeline).

**Setup:**
- [ ] Python 3.11+ with `numpy`, `pandas`.
- [ ] A synthetic dataset with PLANTED ground-truth $\alpha$, $\beta$, and per-physician noise (generate it — synthetic only).
- [ ] A `CLAUDE.md` stub: "synthetic-only; coefficients come from planted ground truth, never from the model; deployable recommendations are educational-pathway only."

**The Task:**

```
Work only in src/ and data/. Synthetic data only — no real prescriber data
exists in this repo.

Build the Living Model loop:
1. src/scm.py — the three-equation SCM (Message, Knowledge, NRx) with
   coefficients loaded from data/ground_truth.json (NEVER hardcoded by you).
2. src/counterfactual.py — for each synthetic physician: abduction recovers U,
   action applies do(message=candidate) over the EDUCATIONAL candidate set,
   prediction ranks candidates. Writes out/recommendations.csv with columns:
   physician_id, population_recommendation, individual_recommendation.
3. src/flip_rate.py — prints "FLIP RATE: <fraction>" = share of physicians
   whose individual recommendation differs from the population one.
4. src/drift.py — perturbs the opt-out rate in the sampling frame by +10pp,
   re-runs, and prints how many recommendations change.

Stopping condition: recommendations.csv is written, flip rate prints, drift
delta prints.
Verification step: assert the SCM recovers planted alpha and beta within 0.05
on a no-noise check before any counterfactual runs; print "GROUND TRUTH
RECOVERED" if so. Leave ground_truth.json read-only.
```

**Expected output:** A per-physician recommendation CSV, a flip rate (the abduction payoff), and a count of recommendations that shift when the opt-out rate moves — the drift mechanism made numeric.

**What to inspect:** Confirm "GROUND TRUTH RECOVERED" prints *before* trusting any counterfactual. If the SCM cannot recover planted $\alpha$/$\beta$ on synthetic data, every downstream number is meaningless.

**If it goes wrong:** If the flip rate is ~0, abduction is adding nothing on this dataset — that is a *finding*, not a bug (it means a causal forest would do); report it. If ground truth is not recovered, your structural equations are misspecified — check the equation order against `scm.py`. (Recovery: ground truth is planted, so recovery is always checkable.)

**CLAUDE.md note:** Add "Coefficients come from planted ground truth / identified effects, never from the model. Validate ground-truth recovery before any counterfactual. Deployable recommendations are educational-pathway only; reciprocity-pathway counterfactuals are analysis-only. Report partial identification as bounds."

### Exercise 5 — AI Validation Exercise

**What you're validating:** The SCM and its individual recommendations from Ex3/Ex4 — checking that no coefficient was model-invented, no counterfactual over-claims point identification, and the action step stayed on the educational pathway.

**Validation type:** Causal-content and identification audit against synthetic ground truth. **Risk level:** **High** — a confident point recommendation where only bounds exist, or a model-supplied coefficient, drives an individual targeting decision on false precision.

**Setup:** Use your Ex3/Ex4 output. Or practice on this pre-flawed artifact: an SCM whose `CONFIDENCE → PRESCRIBING` coefficient is filled with a plausible-looking number tagged "data-estimated" but with no identified effect behind it, and an individual recommendation reported as a single message with no uncertainty despite a nonlinear, non-uniquely-recoverable noise term.

**The Validation Task:**

```
Validation Checklist — Chapter 9 (From Priors to a Living Model)

For the SCM and its recommendation below, mark each item
PASS / FAIL / CANNOT-DETERMINE with a one-line reason:

1. Correctness — Does every coefficient trace to an identified effect or the
   data (provenance: data-estimated / literature-supported), and are
   rep-elicited and assumed edges tagged as such — with NO coefficient
   supplied by the model?
2. Completeness — Is every assumed edge listed explicitly with a sensitivity
   range, and is each contested (rep-disagreed) edge set up as a sensitivity
   dimension rather than silently resolved?
3. Scope — Is the deployable recommendation constrained to the educational
   pathway, with reciprocity-pathway counterfactuals labeled analysis-only?
4. Chapter-specific: Abduction integrity — Does the individual
   recommendation actually recover physician-specific noise (Rung-3), or is
   it a population/subgroup average (Rung-2) wearing an individual label?
5. Chapter-specific: Identification honesty — Where the counterfactual is
   only partially identified, is it reported as bounds with the tightening
   assumption named — not a confident point estimate?
6. Failure-mode check — Scan for the fluent-but-wrong tell: a counterfactual
   OVER-CLAIMED beyond partial identification (a point estimate where bounds
   are required), or a coefficient presented as data-estimated with no
   identified effect behind it. Either is an automatic FAIL. Flag any claim
   of correctness that lacks a synthetic ground-truth recovery check.

SCM + recommendation:
<paste Ex3/Ex4 output here>
```

**What to do with findings:** All pass — the model's claims are sourced and honest; proceed. One fail — fix that coefficient or downgrade the over-claimed estimate to bounds and re-audit. Multiple fails — the SCM's content is partly invented or over-precise; rebuild with the no-numbers firewall and validate ground-truth recovery first.

**AI Use Disclosure prompt (mandatory):** *Write two sentences naming what an AI tool did in this exercise and the one judgment it could not make. For example: "I used Claude to scaffold the abduction-action-prediction code and draft the drift-monitor spec; I decided myself what coefficient each structural equation receives, because those come from the identified quasi-experimental effects, and a model that supplies its own estimates is hallucinating the causal content this chapter is built to protect."*

**Series connection:** The signature failure mode is a **counterfactual over-claimed beyond partial identification** (a point estimate where only bounds exist) plus **missing ground truth** (no synthetic recovery check). This is a **T5 (Causal)** validation task with a **T7 (Compliance/Values)** edge for the action-space constraint that keeps recommendations on the defensible educational pathway.
