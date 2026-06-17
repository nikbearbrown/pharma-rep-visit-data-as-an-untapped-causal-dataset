# Chapter 3 — Stuck on Rung 1
*Why the most sophisticated AI in pharma is answering a question nobody should have funded.*

Here is what actually happened when a major pharmaceutical brand deployed a next-best-action engine. The system was real machine learning — gradient-boosted trees, years of interaction data, a clean engineering stack. It produced recommendations for each physician: which message, which channel, which moment. The metric it optimized was email open rate. Someone in the procurement chain had decided that open rate was close enough to prescribing.

Watch what the engine learned to do. It identified physicians who open email — the engaged ones, the clickers, the ones who respond to everything. The brand concentrated budget on reaching them. Measured conversion looked remarkable: the targeted physicians prescribed at a high rate. The campaign was declared a success. But the physicians who open everything are largely the ones who were already going to prescribe. The engine had found people who like email and happen to prescribe. It had not found people whose prescribing *changes* because of the message.

The incremental effect of the spend was near zero. The budget had confirmed loyalty it already had, and the physicians who could actually be moved — the ones whose behavior was genuinely contingent on the message — were invisible to a system built to find the engaged.

This is not a tuning failure. It cannot be fixed with a better-calibrated model. A perfect open-rate predictor would make this worse, not better — it would more precisely identify the physicians least worth spending on. The failure is structural, and it has a name. The engine was answering a question one level below the question the budget was actually asking, and no amount of additional sophistication on the lower level reaches the upper one.

The name for that structure is Pearl's Ladder of Causation. Learning it is the whole business of this chapter.

---

Judea Pearl introduced the ladder as a classification of *kinds of questions* — a taxonomy not of models or data but of the queries a reasoning system is capable of answering. There are three rungs. Each is strictly more powerful than the one below. And the gap between rungs is not a matter of sample size or model sophistication; it is a matter of what the query fundamentally asks.

**The first rung is association.** Detect regularities in passive observation. The defining mathematical object is the conditional probability $P(Y \mid X)$ — the probability of Y given that you *see* X. Pearl's illustrative example is a store manager asking how likely a customer who bought toothpaste is to also buy floss. Correlation lives here. Ordinary regression lives here. Every model that learns from historical data what tends to follow what lives here. The query is: *what goes with what, when I look?*

**The second rung is intervention.** Predict the effect of a deliberate action — something that *changes* the world rather than observes it. The defining object is $P(Y \mid do(X))$, where the $do(\cdot)$ notation signals that X was externally set, not merely observed. Pearl's example is asking what will happen to floss sales if you deliberately double the price of toothpaste. This is different from asking what happened on past occasions when the price was high, because high prices in the past arose for all sorts of confounded reasons — supply shortages, market-wide events — that also moved floss sales through channels having nothing to do with the price mechanism you care about. Seeing a high price is not the same as setting one. The query is: *what would happen if I made this happen?*

**The third rung is counterfactual.** Reason about a specific unit in a world contrary to fact — to ask, after the fact, about the world that was not. Pearl's example: my headache is gone; was it the aspirin? I took the aspirin, the headache resolved, but I cannot run the morning again without the aspirin to find out. The query is: *what would have happened if I had done otherwise?*

![Pearl's Ladder of Causation as a three-rung vertical ladder: association (seeing) at the bottom, intervention (doing) in the middle, counterfactual (imagining) at the top, with a hatched sealing band between rung 1 and rung 2 and one pharma example beside each rung.](../images/03-stuck-on-rung-1-fig-01.png)
*Figure 3.1 — The Ladder of Causation, and the seal Rung-1 data cannot cross*

<!-- → [DIAGRAM: Pearl's Ladder of Causation. Three rungs on a vertical ladder. Rung 1 (bottom): "Association — Seeing. P(Y|X). What goes with what?" Rung 2 (middle): "Intervention — Doing. P(Y|do(X)). What would happen if I made this happen?" Rung 3 (top): "Counterfactual — Imagining. What would have happened if I had done otherwise?" Arrow annotations on left: "Sealed — Rung-1 data alone cannot answer Rung-2 questions." Right side: pharma examples at each rung — email open rate (Rung 1), deliberate message assignment → NRx (Rung 2), for this physician, which message would have changed prescribing (Rung 3). Caption: "Each rung is strictly more powerful than the one below. The gap is not model capacity — it is the kind of question being asked."] -->

---

The load-bearing claim of this chapter — the one that makes everything in the next nine chapters necessary — is that the rungs are **sealed**. Pearl's statement of the result is worth reading precisely:

"We cannot answer questions about interventions with passively collected data, no matter how big the data set or how deep the neural network."

Read that sentence again, because it is doing real work. It is not saying that small datasets are insufficient or that current models are not powerful enough. It is saying that the *kind* of question changes at the rung boundary, and no accumulation of Rung-1 evidence bridges that change. A petabyte of telemetry and a frontier model do not climb the ladder.

But Pearl also says something that makes the result constructive rather than merely depressing:

"A sufficiently strong and accurate causal model can allow us to use rung-1 observational data to answer rung-2 interventional queries. Without the causal model, we could not go from rung 1 to rung 2."

This is the move. The causal model is the bridge. The data does not change — you still have your observational clicks and visits and prescription records. What changes is that you add structure: a graph, a set of assumptions about what causes what, that lets you reason about interventions using the observational variation that happens to exist in the data. Without that structure, no amount of observation reaches the interventional question. With it, the same data can sometimes answer questions about what would happen if you deliberately changed something.

This is why Pearl places present-day deep learning "squarely on rung one — sharing the wisdom of an owl." An owl is a superb predictor of where the mouse will be. It has no model of why the mouse moves, no representation of the mechanisms that govern the mouse's behavior. It cannot reason about what the mouse would do if the field changed. The owl predicts; it does not intervene; it cannot imagine counterfactuals. The most powerful neural network trained on the most complete observational record is, from the standpoint of causal reasoning, still an owl.

---

The notation that makes this precise is the $do$-operator.

$do(X = x)$ denotes an external manipulation that forces X to the value $x$. Structurally, it does two things simultaneously: it replaces X's usual causal equation with the constant $x$, and it **severs all incoming arrows to X** in the causal graph. Every mechanism that normally determines X's value is bypassed. X is set, not caused.

The consequence is that $\mathbb{E}[Y \mid do(X = x)]$ differs, in general, from the observational $\mathbb{E}[Y \mid X = x]$. The observational conditional reflects every reason X took its value — including reasons that also moved Y through other paths. The interventional expression wipes those reasons out by fiat. The treatment was administered regardless of everything that would normally have determined whether it was administered.

Apply this to the pharma budget decision. When physicians who received the safety message differ from those who did not — because reps chose where to deploy it, because detail-receptive physicians are systematically different from detail-resistant ones — then $\mathbb{E}[\text{NRx} \mid \text{message} = \text{safety}]$ is contaminated by that selection. It reflects who got the message and why, not what the message did. The clean quantity the budget needs is $\mathbb{E}[\text{NRx} \mid do(\text{message} = \text{safety})]$ — what prescribing would be if the safety message were assigned regardless of rep strategy, physician receptivity, or anything else that normally governs its deployment.

That is a Rung-2 quantity. It requires severing the arrows that govern message assignment. Observational data, no matter how rich, cannot do that severance. Only a causal model — or a study design that genuinely randomizes the message, which is a physical instantiation of the $do$ — can.

![Two side-by-side causal graphs. Left, observational: arrows from physician receptivity and rep strategy both point into message, which points to prescribing. Right, interventional: the two incoming arrows to message are severed with red cut marks, leaving only the message-to-prescribing arrow.](../images/03-stuck-on-rung-1-fig-02.png)
*Figure 3.2 — The do-operator severs the arrows into the treatment*

<!-- → [DIAGRAM: Two causal graphs side by side. Left graph: observational — arrows from "physician receptivity" and "rep strategy" both point into "message received," which points into "NRx." The confounding paths are visible. Right graph: interventional — the arrows from "physician receptivity" and "rep strategy" into "message" are severed (crossed out). Only the message → NRx arrow remains. Label: "do(message = safety) removes the confounding. Observational conditioning does not." Caption: "The do-operator severs the arrows that contaminate the observed association. This is what randomization does physically, and what a causal model does mathematically."] -->

---

Now name the Rung-2 question the pharma budget is actually asking, because it has a precise formulation and the precision matters.

A brand allocating a promotional budget between a safety-first message and an efficacy-first message is not asking which physicians tend to prescribe more after receiving each message. It is asking:

$$P(\text{NRx} \mid do(\text{message} = \text{safety-first})) \quad \text{vs.} \quad P(\text{NRx} \mid do(\text{message} = \text{efficacy-first}))$$

Which *deliberately assigned* message produces more new prescriptions? That is a Rung-2 query. It asks what would happen if the message were assigned — by the brand, externally, as an act of deliberate choice — not merely what tends to follow when physicians happen to receive one message or the other through the normal confounded machinery of rep deployment.

The email open rate that the NBA engine optimizes is $P(\text{open} \mid \text{physician}, \text{message})$ — the probability that a given physician opens a given message. That is a Rung-1 quantity. It answers: *given what I observe about this physician and this message, what is the probability the email gets opened?* It does not answer: *if I assign this message to this physician, what happens to prescribing?* Those are not even the same sentence. The former conditions on observed covariation; the latter asks about the consequence of a deliberate act.

A physician with a high predicted open rate may be a loyal prescriber who reads everything sent to them and prescribes the brand regardless. Zero incremental effect. Maximum predicted score. The better the open-rate predictor, the more confidently it directs budget toward the people who least need it. This is not a bug in the implementation — it is the inevitable consequence of answering the wrong question very precisely.

---

The pharma analytical apparatus has three main practices, and each one sits on Rung 1. It is worth naming them exactly, because they each feel more sophisticated than Rung 1 suggests.

**Content optimization** mines CLM telemetry — seconds per slide, navigation patterns, positive or negative reaction buttons — for which slides hold attention. The optimization target is engagement: dwell time, reaction score, drop-off rate. This is pure association: what co-occurs with engagement, not what engagement causes in prescribing. And there is a second structural problem lurking here that will be developed later: dwell time and reaction scores are *post-treatment colliders*, and conditioning on them to estimate the message-to-prescribing effect introduces a distinct kind of bias. The rung problem and the collider problem are separate, both real, and both present in every content-optimization dashboard currently deployed.

**Rep performance management** measures call volume against quota and attributes prescribing lift to the calling. The attribution is silent: it assumes calls cause prescribing. But a high-converting rep may simply cover territories full of already-loyal physicians. Her quota attainment measures her geography as much as her causal effect. The literature on advertising incrementality — which is the same argument applied to a different industry — has shown repeatedly that observational response rates overstate causal return, often by large multiples. Rep performance dashboards make this error structurally, by design.

**Next-best-action engines** recommend message, channel, and timing per physician. The NBA platforms from the major vendors — Aktana, IQVIA, Indegene — describe their optimization targets in their own public materials. Aktana's data-science team published a machine-learning method in the *Journal of the PMSA* (Marc-david Cohen, Aktana) for identifying message sequences that maximize open and click-through rates — predicting the probability of an email being opened or clicked. IQVIA's Next Best Action describes omnichannel orchestration and dynamic call planning, framed throughout as engagement optimization. Indegene's NBA materials report campaign-engagement uplift as a success metric. [verify — unconfirmed: the specific quantitative magnitudes some vendor case studies cite (e.g., "100M+ field suggestions," "+25% engagement") could not be confirmed against current public materials; the qualitative claim — every public optimization target is an engagement proxy — is what the argument rests on and is checkable.]

The pattern is consistent across the category. The optimization target in every public description is an engagement proxy. No major vendor publicly claims to estimate $P(\text{NRx} \mid do(\text{message}))$ with a stated identification strategy. This is a sourced inference from public self-descriptions, not a proof about proprietary internals — vendor methods are closed and the claim cannot be established from outside. But it is the externally checkable pattern, and it is the empirical basis for what follows.

| Practice | Stated optimization target | Actual query answered (Rung 1) | Rung-2 question it cannot answer |
| --- | --- | --- | --- |
| Content optimization | dwell time, reaction score, drop-off | `P(engagement \| message, physician)` | `P(NRx \| do(message))` |
| Rep performance management | call attainment vs. quota | `P(high conversion \| rep territory)` | `P(NRx \| do(rep assigned))` |
| Next-best-action engine | email open rate, click-through, visit acceptance | `P(open \| physician, message)` | `P(NRx \| do(message = X)) vs. do(message = Y)` |

*Table 3.1 — All three deployed practices answer Rung-1 questions; none publicly claims a do-expression, yet the budget question is Rung 2 (a sourced inference from public self-descriptions, not a proof about proprietary internals)*

---

There is a misconception so seductive it deserves its own treatment.

*Prediction plus personalization equals causal targeting.*

It does not. A model that predicts $P(\text{NRx} \mid X)$ extremely well, and then personalizes the action to high-prediction physicians, is still on Rung 1. It ranks who is likely to prescribe, not for whom the message causes prescribing. Those are different populations, and the difference is not small. A physician with a high predicted NRx propensity is precisely the one who might prescribe regardless of any message. Maximum predicted score, minimum incremental value.

The better your predictor, the more confidently it points you toward the people you least need to spend on. This is not a pathological failure mode; it is the generic consequence of confusing prediction with intervention. Pearl's framing: a forecast predicts rain. Carrying an umbrella does not cause rain. Prediction and intervention are different questions, and the gap between them is not closed by making the prediction more accurate.

Bottou and colleagues showed this formally in 2013, studying Bing ad placement. A system that optimizes measured engagement — clicks, conversions — is not estimating the causal effect of its own actions. Predicting the consequence of a system *change* requires counterfactual or causal estimation; observational optimization cannot supply it. Lewis and Rao (2015, *Quarterly Journal of Economics* 130(4):1941–1973, "The Unfavorable Economics of Measuring the Returns to Advertising") showed across 25 large field experiments that even very large observational datasets cannot pin down advertising ROI, because the variation needed to identify the causal effect is swamped by confounding that no covariate list resolves — the median confidence interval on ROI was over 100 percentage points wide. Gordon and colleagues (2019, *Marketing Science* 38(2):193–225) compared observational ad-measurement methods to randomized field experiments at Facebook and found the observational estimates diverged from the experimental ground truth — frequently overstating effects, even after conditioning on extensive covariates. The pharma-NBA critique is the same argument in a different domain. Optimizing measured engagement overstates causal value, systematically.

---

Return now to the opening case and trace the failure precisely.

The natural response to an underperforming engine is to improve the model: better features, a deeper architecture, tighter calibration. Suppose the team succeeds and predicts open rate near-perfectly. What has been achieved?

Nothing, from the standpoint of the budget question. A perfect Rung-1 model is still Rung 1. Pearl's sealing result does not have a carve-out for high-accuracy predictors. "No matter how big the data set or how deep the neural network" is not a rhetorical flourish; it is the mathematical content of the result. The owl is wiser; it still has no model of the mouse.

The fix is not a better model. It is a better question, plus a causal model to answer it. The reclassification is this: the engine answers $P(\text{open} \mid \text{physician}, \text{message})$ — Rung 1. The budget needs $P(\text{NRx} \mid do(\text{message}))$ — Rung 2. Naming that gap is the move. Bridging it requires an identification strategy — a structural argument for where in the data the variation exists that answers the Rung-2 question despite the observational origin of the data.

One thing the reclassification does not supply: proof that the Rung-2 question is *identifiable* in this specific dataset. Knowing you are asking the right question is necessary, not sufficient. Whether the variation in the data can be used — whether there is an instrument, a natural experiment, a threshold, a rollout — is a separate argument, harder to make, and it is what the next chapter builds.

---

There is a small datum that has appeared twice in this book without resolution — the physician who lingered on the safety slide for 47 seconds. The telemetry records it precisely: 47 seconds, that slide, that physician, that visit. It does not record why. The 47 seconds are consistent with at least three incompatible causal stories: careful reading, which is a positive educational signal; skeptical scrutiny, hunting for the methodological flaw, which is a negative signal; and polite inattention while distracted by a patient call, which is noise. Identical telemetry, opposite causal inferences, no way to distinguish them in the data.

This is the epistemic gap: the data captures what happened, not the mechanism behind it. The experienced rep who has called on this physician forty times has a strong intuition about which story is true. That intuition is nowhere in the database, and it evaporates when the territory changes hands. The 47-second ambiguity is the emblem of something deeper: the data records behavior, and behavior underdetermines mechanism, and mechanism is what the Rung-2 question needs. Climbing the ladder is, at bottom, the project of recovering mechanism from behavior — and it requires bringing structure to the problem that the behavior alone cannot supply.

## What Would Change My Mind

The chapter's core claim is that deployed pharma analytics live on Rung 1 and that the budget question is Rung 2. I would revise if: (1) a major vendor published a method that explicitly estimates $P(\text{NRx} \mid do(\text{message}))$ with a stated identification strategy — not an engagement proxy dressed in causal vocabulary; (2) Pearl's rung-separation result were shown not to apply here — for instance, if an engagement proxy were demonstrated to be a sufficient statistic for the incremental effect under defensible assumptions in this specific market (it is not in general, but a specific context could surprise); or (3) it turned out that open rate and incremental NRx are empirically tightly coupled on this data in a way that collapses the proxy-substitution objection. Each is checkable. None is currently supported.

## Still Puzzling

What the vendors are actually doing internally — whether the absence of do-language in public materials reflects a genuine absence of causal estimation or merely a marketing choice — is unknowable from outside. The claim stays an inference. How tightly an engagement proxy is coupled to incremental NRx in real markets is also an open empirical question: it might be "close enough" in some specific context, and knowing when would be useful. And the question that propagates forward: even granting that the Rung-2 question is the right one, is it *identifiable* in this observational data at all — through what variation, under what assumptions, with what falsification test? The honest answer is "sometimes, through the training-cohort rollout, if its assumptions hold." That is precisely the bet the next chapter places and tests.

---

## Exercises

**Warm-up**

1. *(Recall — low difficulty) What this tests: whether you can state the rung definitions and the sealing result.* In your own words, state Pearl's three rungs of causation, naming the defining mathematical object at each rung. Then state the sealing result: why does adding more data or a deeper model not climb from Rung 1 to Rung 2?

2. *(Recall — low difficulty) What this tests: whether you can explain the do-operator mechanically.* Explain in two sentences what $do(X = x)$ does to a causal graph that observational conditioning on $X = x$ does not. Give one example from the chapter — the message assignment case — illustrating how the two expressions differ.

3. *(Recall — low difficulty) What this tests: whether you can name the three proxy-substitution failure modes.* Name the three reasons why substituting an easy engagement proxy for the hard incremental target fails. For each, give a one-sentence explanation that does not use the word "proxy."

**Application**

4. *(Apply — medium difficulty) What this tests: rung classification on real vendor language.* Find a public description of a pharmaceutical NBA, content-optimization, or rep-performance platform — a vendor white paper, a product page, or an analyst report. Quote the exact phrase stating what the tool optimizes or predicts. Classify that target on Pearl's ladder (Rung 1, 2, or 3) and justify the classification by stating what mathematical query the tool is actually computing. Then write the Rung-2 do-expression the budget needs for the same decision.

5. *(Apply — medium difficulty) What this tests: applying the do-operator to a new domain.* A digital health company deploys a recommendation engine that identifies which patients are most likely to refill their prescription based on app engagement data. Classify the engine's optimization target by rung, write the Rung-2 question a pharmaceutical brand would actually want answered, and explain in one sentence why the engagement-based ranking cannot answer it.

6. *(Apply — medium difficulty) What this tests: the misconception that prediction plus personalization equals causal targeting.* A data scientist argues: "Our propensity model is 94% accurate at predicting which physicians will prescribe next month, and we personalize our outreach to the top decile. That's basically causal targeting." Write a two-paragraph response that engages with the argument rather than just asserting it is wrong — explain what the model is learning, who the top-decile physicians are likely to be, and why high prediction accuracy makes the incremental-value problem worse, not better.

**Synthesis**

7. *(Synthesis — high difficulty) What this tests: producing the named artifact.* For one real, currently-deployed pharma analytic practice (an NBA engine's optimization target, a content-optimization dashboard, or a rep-performance scorecard), produce a one-page classification: (a) the practice's optimization target in its own words, (b) the rung classification with structural justification, (c) the Rung-2 do-expression the budget needs for the same decision, and (d) one sentence explaining why the practice's current target cannot answer the do-expression.

8. *(Synthesis — high difficulty) What this tests: connecting the epistemic gap to the ladder.* The 47-second-slide ambiguity illustrates what the chapter calls the epistemic gap — the data captures behavior, not mechanism. Explain in three to four sentences how the epistemic gap and the rung-separation result are related: why does the underdetermination of mechanism by behavior mean that observational data alone cannot answer the Rung-2 question, even with a very large sample? Then name one kind of structural information — beyond more data — that would help close the gap.

**Challenge**

9. *(Challenge — open-ended) What this tests: evaluating a potential exception to the sealing result.* Pearl says Rung-1 data can answer Rung-2 questions "if you have a sufficiently strong and accurate causal model." Design a scenario in the pharma HCP marketing context where you could, in principle, use observational engagement data to answer a Rung-2 question about message assignment — specifying the causal model you would need, the assumptions it would require, and the falsification test that would tell you whether those assumptions hold. Then state honestly whether you believe the assumptions are likely to be satisfied in a real commercial dataset.

---

## Prompts

### Figure 3.1 — The Ladder of Causation, and the seal Rung-1 data cannot cross

Build a single self-contained HTML file (inline CSS; D3 7.9.0 from the cdnjs CDN) rendering Pearl's three-rung Ladder of Causation as a vertical hierarchy. Two vertical rails frame three stacked rung boxes: Rung 1 association / seeing (bottom, broadest, light gray), Rung 2 intervention / doing (middle, dark ink), Rung 3 counterfactual / imagining (top, mid-gray), each strictly more powerful than the one below. A hatched red sealing band sits between rung 1 and rung 2 labeled so no arrow crosses it. Beside each rung, a connector to one pharma example marker box: email open rate (R1), deliberate message assignment to NRx (R2), which message would have changed Rx (R3). Hover/focus any rung for a tooltip with its query form and pharma reading. Deliverable: 700x420 viewBox, one defs arrowhead, Brutalist palette via CSS variables with dark-mode media query, EB Garamond title / Inter body. Caption must state the gap is the kind of question being asked, not model capacity, and that rep-visit telemetry sits on Rung 1 while the budget question is Rung 2; structural depiction only.

### Figure 3.2 — The do-operator severs the arrows into the treatment

Build a single self-contained HTML file (inline CSS; D3 7.9.0 from the cdnjs CDN) rendering two side-by-side four-node causal graphs with identical geometry. Nodes: receptivity and rep strategy (confounders, mid-gray), message (treatment, dark ink), prescribing (outcome, red). Left graph (observational): all three edges intact — receptivity to message, rep strategy to message, message to prescribing. Right graph (interventional, do(message)): the two incoming arrows to message are drawn faint and struck through with red cut marks, leaving only message to prescribing. A dashed divider separates the panels; panel labels above. Hover/focus any node for a tooltip explaining its causal role. Deliverable: 700x480 viewBox (extra height for the four-line caption), one defs arrowhead, Brutalist palette via CSS variables with dark-mode media query, EB Garamond title / Inter body. Caption must state the do-operator severance is structure — what randomization does physically and a model does mathematically — and depicts the operation, not a proof of identifiability; red is used as the data accent, never as danger. Structural only.

---

## Chapter 3 Exercises: Stuck on Rung 1

**Project:** The Causal Interview Bot
**This chapter adds:** ladder-aware question stems — Rung-1, Rung-2, and Rung-3 versions of the bot's questions, written in rep-natural language so the rep climbs the ladder without ever hearing the word "rung."

### Exercise 1 — When to Use AI

Three drafting tasks the model is well-suited to here.

First, **generating Rung-1 / Rung-2 / Rung-3 phrasings of the same underlying question.** You want a question about a converted account in three flavors — what tends to happen (R1), what would happen if you led differently (R2), what would have happened without the visit (R3) — all in rep language. *Why AI works here:* this is parallel rephrasing in a constrained register, classic option-generation. **The tell:** you can read each version and judge which rung it actually sits on.

Second, **rehearsing the rung classification on vendor descriptions.** Paste a tool's stated optimization target and ask the model to surface the buried sentence naming what it optimizes. *Why AI works here:* it is fast at finding the load-bearing phrase in marketing prose. **The tell:** the quote is right there to check against the model's claim.

Third, **drafting Socratic do-operator examples.** Toothpaste-and-floss-style analogues until seeing-vs-doing is automatic. *Why AI works here:* generating illustrative examples is low-stakes and self-checking. **The tell:** you can verify each example actually distinguishes observation from intervention.

### Exercise 2 — When NOT to Use AI

Two judgments that resist automation.

First, **deciding which rung a bot question actually elicits.** *Why AI fails here:* the model will over-climb — it grants Rung-2 status to any question containing the word "would," the same suggestibility that makes it grant causal status to any vendor saying "outcome." A question can sound interventional and elicit only an association. **The tell:** if the model's rung label is your *reason* for filing an elicited claim as interventional knowledge, you have let it inflate the evidence; if you re-classify by asking "does this question sever the arrows into the action, or just observe the action's usual occurrence?", you have used it as a tool.

Second, **certifying that the rep's Rung-2 answer is real interventional knowledge and not a confident story.** *Why AI fails here:* there is no ground truth, and the model cannot tell a rep's genuine intervention experience from a plausible-sounding rationalization. **The tell:** the model can format the answer; it cannot vouch for it. **Series connection:** this is a **T5 (Causal)** task — distinguishing seeing from doing is the rung-separation result itself, and the bot's whole value is eliciting Rung-2/3 knowledge the data cannot reach.

### Exercise 3 — LLM Exercise

**What you're building this chapter:** the bot's *ladder-structured question bank* — for one elicitation target, the Rung-1, Rung-2, and Rung-3 stems in rep-natural language, each tagged (in a note to you, never to the rep) with the rung it occupies and what it extracts.

**Tool:** the **Claude Project** ("Causal Interview Bot"). The Project already holds the elicitation spec (Ch 1) and the field-disambiguation questions (Ch 2); this chapter organizes the bot's questions by rung, so it must build on what the bot already knows to ask.

**The Prompt:**

```
Building on the elicitation spec and field-disambiguation questions already in
this Project, write the bot's LADDER-STRUCTURED question bank for ONE target.

Pearl's three rungs, in plain terms:
- Rung 1, association: what tends to go with what, observed passively.
- Rung 2, intervention: what would happen if you deliberately did something
  differently.
- Rung 3, counterfactual: for a specific past case, what would have happened had
  you acted otherwise.

ABSOLUTE CONSTRAINT: the rep must NEVER hear the words "rung," "association,"
"intervention," "counterfactual," "confounder," or "causal." All questions are in
ordinary rep language.

Target to elicit: the rep's knowledge about whether leading with patient-outcomes
data versus mechanism-of-action data moves prescribing for a specific physician.
Concrete scenario: Dr. Pham, a community oncologist, recently converted to the
brand after a stretch of efficacy-first visits.

Produce THREE question stems:
1. A Rung-1 stem (surfaces what the rep has observed tends to happen) — extracts
   candidate confounders (formulary, competitor detailing, a colleague's switch).
2. A Rung-2 stem (asks what would happen if the rep led differently / what would
   definitely move her) — extracts heterogeneous-effect / susceptibility knowledge.
3. A Rung-3 stem (asks about the converted account: would it have converted without
   the visits? the ones that didn't — what would you have done differently?) —
   extracts mediator-weight knowledge (education vs relationship vs reciprocity).

After EACH stem, add a bracketed note to ME naming the rung and exactly what causal
information it is meant to surface. Do not over-climb: if a stem only asks what the
rep has seen happen, label it Rung 1 even if it sounds forward-looking.
```

**What this produces:** three rep-natural stems for one target, ladder-tagged for you and jargon-free for the rep — the bot's first vertical slice across all three rungs.

**How to adapt:** *For your dataset:* replace the patient-outcomes-vs-mechanism target and Dr. Pham with a real message contrast and a real converted account from your panel. *For ChatGPT/Gemini:* prepend your Ch 1–2 artifacts; without Project memory the model won't know the spec. *For a Claude Project:* keep the rung definitions and the jargon-ban in the system prompt (stable rules); send only the target and scenario as a message.

**Connection to previous chapters:** Chapter 2 gave the bot field-anchored questions; this chapter sorts the bot's questions by the *kind* of causal knowledge they reach, so the prior DAG can later distinguish associations the rep has merely seen from interventions she has effectively run.

**Preview of next chapter:** Chapter 4 adds a specific Rung-2 probe — questions that test whether the training-cohort timing was as-good-as-random for a given physician, the assumption the whole instrument rests on.

### Exercise 4 — CLI Exercise

**What you're building:** a rung-tagged question bank file the bot loads as context, plus a small auditable corpus of vendor rung-classifications — all on public/synthetic material.

**Tool:** Claude Code — because you want a versioned `question-bank.md` and a `practices/` corpus on disk, not chat output. **Skill level:** Intermediate.

**Setup:**
1. Prereq artifact: the three ladder stems from Exercise 3.
2. Tool: Claude Code in the bot repo.
3. CLAUDE.md rule: carry the synthetic-only rule; add "Rung labels are notes to the analyst only — they must never appear in any rep-facing question string."

**The Task:**

```
Work only inside this repo. Append to spec/interview-stems.md a new section
"## Chapter 3 — Ladder-structured (Rung 1/2/3)" and write in the three stems I
paste, each followed by an HTML comment <!-- rung: N; extracts: ... --> so the rung
tag is invisible to any rep-facing renderer.

Then create practices/rung-classifier.md: a short rubric with the three rung
definitions and, for THREE public vendor descriptions I will paste, one entry each
recording the quoted optimization target, the rung classification, and the
do-expression the budget actually needs.

Do not write rep-facing strings that contain the words "rung," "association,"
"intervention," or "counterfactual." Leave all other files alone.

Verification step: grep the rep-facing question strings (not the comments) for the
banned words and print any matches. If there are matches, that is a bug — list them
and stop before writing anything else.
```

**Expected output:** an updated `interview-stems.md` with rung tags hidden in comments, and a `practices/rung-classifier.md` corpus — with a clean grep showing no banned jargon in any rep-facing string.

**What to inspect:** run the grep yourself. Confirm the rung labels live only in comments/notes, and that each vendor entry's do-expression is a genuine Rung-2 `P(NRx | do(message))`, not a restatement of the engagement proxy.

**If it goes wrong:** the common failure is the agent classifying a vendor as Rung 2 because the description says "outcome optimization." Recovery: demand the quote that mentions a *deliberate intervention or incremental effect*; if there is none, downgrade to Rung 1.

**CLAUDE.md note:** keep the "rung labels are analyst-only" rule — it guarantees the bot stays in rep language as the question bank grows.

### Exercise 5 — AI Validation Exercise

**What you're validating:** the ladder stems from Exercise 3 — specifically whether each stem actually sits on the rung it claims, and whether any rep-facing wording leaks jargon.

**Validation type:** Reasoning · **Risk level:** High — a stem mislabeled Rung 2 will cause the bot to file a mere observation as interventional knowledge, the exact rung-confusion the chapter is about, and it will corrupt the prior's edge confidences.

**Setup:** use your Exercise 3 stems. If they're clean, inject a flawed one — relabel a Rung-1 "what usually happens after a meal" stem as Rung 2 — to confirm your checklist catches over-climbing.

**The Validation Task:**

```
Validation Checklist — Chapter 3 (Ladder-Structured Stems)

For each stem, mark Pass / Fail / Cannot-determine on:

1. Correctness — does the stem's actual question match its claimed rung? (R2 must
   ask about a deliberate change; R3 must ask about a specific past case otherwise.)
2. Completeness — across the three stems, are all three rungs genuinely represented,
   not two associations and one intervention?
3. Scope — is every rep-facing string free of the banned jargon (rung,
   association, intervention, counterfactual, confounder, causal)?
4. Chapter-specific: No over-climb — is any stem that merely asks "what tends to
   happen" wrongly tagged Rung 2 because it sounds forward-looking?
5. Chapter-specific: Extraction match — does each stem's "extracts" note match what
   that rung can actually surface (R1 confounders, R2 susceptibility, R3 mediator
   weights)?
6. Failure-mode check — correlation-asserted-as-cause: does any stem invite the rep
   to state that something "drove" or "caused" prescribing rather than report what
   she observed or would do? A stem can be fluent and rung-tagged and still smuggle
   in a causal assertion. Flag it. Also flag any rung label you cannot verify from
   the stem text alone (missing ground truth on what the rep actually meant).
```

**What to do with findings:** all pass — commit the bank. One fail — re-rung that stem and rewrite to match. Multiple uncertain — the rungs are blurred; regenerate from Exercise 3 with the no-over-climb rule emphasized.

**AI Use Disclosure prompt:** *Write two sentences naming what an AI tool did in your Chapter 3 work and the one judgment it could not make. The judgment most specific here: whether a stem truly reaches Rung 2 — because the model over-grants interventional status to anything phrased as "would," and only you can tell whether the rep is reporting an intervention she has effectively run or just narrating an association.*

**Series connection:** the failure mode is **correlation-asserted-as-cause** (and its cousin, over-climbing the ladder), which maps to **T5 (Causal)**: the rung boundary is the seal the whole book turns on, and the bot's job is to elicit knowledge from above the seal that the data below it can never supply.

---

## References (fact-check pass)

1. Pearl, J. & Mackenzie, D., *The Book of Why* (2018) — Ladder of Causation (association/intervention/counterfactual), the sealing result, the do-operator, the "owl" and rain/umbrella illustrations, and the quoted passages. [CONFIRMED.]
2. Lewis, R. A. & Rao, J. M., "The Unfavorable Economics of Measuring the Returns to Advertising," *Quarterly Journal of Economics* 130(4):1941–1973 (2015) — 25 large field experiments; median ROI confidence interval >100 percentage points wide. [CONFIRMED verbatim.]
3. Gordon, B. R., Zettelmeyer, F., Bhargava, N. & Chapsky, D., "A Comparison of Approaches to Advertising Measurement: Evidence from Big Field Experiments at Facebook," *Marketing Science* 38(2):193–225 (2019) — observational estimates diverge from / overstate vs. experimental ground truth. [CONFIRMED.]
4. Bottou, L. et al., "Counterfactual Reasoning and Learning Systems: The Example of Computational Advertising," *Journal of Machine Learning Research* 14:3207–3260 (2013) — Bing ad placement; engagement optimization ≠ causal effect of system actions. [CONFIRMED.]

*Unverified (correctly hedged in-text): specific vendor magnitudes ("100M+ field suggestions," "+25% engagement") — already `[verify — unconfirmed]`; the argument rests on the checkable qualitative claim, not these numbers. See factchecks/03-stuck-on-rung-1.md.*
