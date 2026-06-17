# Chapter 3: Stuck on Rung 1

## Overview

You can now see the dataset (Chapter 1) and assemble it (Chapter 2). This chapter answers the question those two raised and left hanging: it has all been here for fifteen years — so why is nobody using it causally?

The answer is not incompetence and not laziness. It is that the entire pharma analytical apparatus — content optimization, rep-performance management, and the next-best-action (NBA) engines that allocate the promotional budget — lives on the bottom rung of a ladder, and the bottom rung cannot, even in principle, answer the question the budget is actually asking. By the end of this chapter you will be able to state Pearl's three rungs and the do-operator; classify any current pharma analytic practice by rung and justify the classification; and write, in do-notation, the Rung-2 question a brand's budget actually needs — and explain why the engagement metric currently optimized cannot answer it.

This is the conceptual keystone of the book. Everything in Acts II and III is machinery for climbing from Rung 1 to Rung 2. If you accept this chapter's argument, the rest follows. If you don't, nothing later will land. So we build it carefully and we source it hard.

## Learning objectives

After this chapter, you should be able to:

- **(Understand)** State Pearl's three rungs of causation and the do-operator, and explain why the rungs are "sealed" — why Rung-1 data alone cannot answer a Rung-2 question.
- **(Analyze)** Classify a current pharma analytic practice (content optimization, rep-performance management, an NBA engine) by rung, and justify the classification against how the practice describes itself.
- **(Evaluate)** State, in do-notation, the Rung-2 question a brand's budget needs, and argue why the engagement proxy currently optimized cannot answer it.

## Opening case: the engine that optimized the wrong thing

A pharmaceutical brand buys a next-best-action engine — an AI system that recommends, for each physician, which message to send through which channel at which time. The engine is real machine learning: gradient-boosted trees, maybe collaborative filtering, trained on years of interaction data. It works, in the sense that the metric it optimizes goes up. The metric is email-open rate. Someone, somewhere in the procurement chain, decided open rate was "close enough" to prescribing.

It is not close enough. It is a different question on a different rung.

Watch what the engine actually does. It learns which physicians open email, and it surfaces them — the engaged ones, the ones who click everything. The brand pours budget into reaching them. Measured conversion looks terrific: the targeted physicians prescribe at a high rate. Victory is declared. But the physicians who open everything are largely the ones who were *already* going to prescribe — the "sure things." The engine found people who like email and prescribe; it did not find people whose prescribing *changes* because of the message. The *incremental* effect of the spend is near zero. The budget confirmed loyalty it already had, and the physicians who could actually be *moved* — the persuadables — were invisible to a ranker built on engagement.

This is not a tuning failure. You cannot fix it with a better-calibrated open-rate model. A *perfect* open-rate predictor still answers the wrong question. The failure is a rung failure, and naming it precisely is what the rest of this chapter does. (The persuadables-vs-sure-things reframe is Chapter 12; here we only need to feel why an engagement target is the wrong target.)

## Core sections

### Pearl's Ladder of Causation

Judea Pearl frames causal reasoning as three tiers of *queries* — a classification, in the spirit of Turing, of a system by the kinds of questions it can answer. Each rung is strictly more powerful than the one below. Pearl names them *seeing*, *doing*, and *imagining*.

**Rung 1 — Association ("What if I *see*?").** Detect regularities in passive observation. The defining object is the conditional probability `P(Y | X)` — "the probability of Y given that you *see* X." Pearl's running example is a store manager: "how likely is a customer who bought toothpaste to also buy floss?" — `P(floss | toothpaste)`. Correlation and ordinary regression are Rung-1 tools. (Pearl & Mackenzie, *The Book of Why*, Ch. 1: "At the first level, association, we are looking for regularities in observations.")

**Rung 2 — Intervention ("What if I *do*?").** Predict the effect of a deliberate action that *changes* the world. Written `P(Y | do(X))`. Pearl's example: "what will happen to our floss sales if we *double the price* of toothpaste?" This is distinct from "what happened on past occasions when the price was high," because past high prices arose for confounded reasons — shortages, market-wide hikes — that also moved floss sales through other channels. Seeing a high price is not the same as setting one.

**Rung 3 — Counterfactual ("What if I *had done* otherwise?").** Reason about a specific unit in a world contrary to fact: "my headache is gone — was it the aspirin?" This requires taking what actually happened, negating an observed fact, and re-running the world. It is the rung the individual physician recommendation eventually needs ("for *this* doctor, which message *next week*?") — and it is Chapter 9's destination.

### Why the rungs are sealed

This is the load-bearing claim of the chapter, so we state it in Pearl's own terms.

The rungs differ *fundamentally*. Each unlocks queries the rungs below it cannot answer, and the gap is not a matter of sample size or model capacity. Pearl's operational statement:

> "We cannot answer questions about interventions with passively collected data, no matter how big the data set or how deep the neural network."

Read that twice, because it is the whole book in one sentence. A petabyte of telemetry and a frontier model do not climb the ladder. Rung-1 data alone cannot reach Rung 2. The *only* bridge is a causal model:

> "A sufficiently strong and accurate causal model can allow us to use rung-1 observational data to answer rung-2 interventional queries. Without the causal model, we could not go from rung 1 to rung 2."

This is why Pearl places present-day deep learning "squarely on rung one… sharing the wisdom of an owl." An owl is a superb predictor of where the mouse will be; it has no model of *why* the mouse moves. The implication for this book is exact: no quantity of rep-visit telemetry, fed to no model however large, produces a Rung-2 answer *unless you add a causal model*. Acts II and III are how you add one.

### The do-operator

The bridge is built on a single piece of notation. `do(X = x)` denotes an *external* manipulation that forces X to the value x. Structurally, it **replaces X's equation with the constant x and severs all incoming arrows to X**, leaving every other mechanism intact. The intervention reaches in and *sets* the variable, rather than *observing* whatever the world's usual machinery produced.

The consequence is that `E[Y | do(X = x)]` differs, in general, from the observational `E[Y | X = x]`. The observational conditional reflects every reason X took its value — including reasons that also moved Y. The interventional one wipes those reasons out by fiat. When physicians who got the safety message differ systematically from those who didn't (because reps chose where to spend the message), `E[NRx | message = safety]` is contaminated by that choice; `E[NRx | do(message = safety))` is the clean quantity the budget wants.

Pearl introduced the operator in 1995 and developed *do-calculus* — the rule set for deciding when a do-expression is *identifiable* from observational data plus a graph — in *Causality* (Cambridge University Press; 1st ed. 2000, 2nd ed. 2009). (The exact page for the `do(X=x)` definition in the 2nd edition, and the 1995 origin date, are cited from standard knowledge and carried `[verify]` for print.)

### The Rung-2 question the budget actually needs

Now make it a money decision, because the do-operator stays abstract until it is one.

A brand allocating a message budget is not asking a Rung-1 question. It is asking:

> **`P(NRx | do(message = safety-first))` vs. `P(NRx | do(message = efficacy-first))`.**

Which *deliberately assigned* message produces more new prescriptions? That is a Rung-2 query, written in do-notation, and it is the literal question a brand manager funds. Pearl's "double the price of toothpaste" is the warm-up; the message-variant budget is the load-bearing instance for the rest of the book.

The flagged claim, handled honestly. The source essay asserts "the do-operator is in no deployed pharma NBA engine." We *soften* this from a universal to a sourced inference, because vendor methods are proprietary and the negative cannot be proven from the outside. The defensible version: **no major vendor publicly describes its engine as estimating a do-expression; all describe engagement-proxy optimization.** That is plausible and consistent with the platforms' own self-descriptions (next section), but it is an inference from public material, not a disclosure. State it that way, and only that way. (`[verify — proprietary.]`)

### The misconception that sinks most projects

> "Prediction plus personalization equals causal targeting."

It does not. A model that predicts `P(NRx | X)` extremely well, and then personalizes the action to high-prediction physicians, is *still on Rung 1*. It ranks *who is likely to prescribe*, not *for whom the message causes prescribing*. A physician with a high predicted NRx may be a sure thing who prescribes regardless of any message — zero incremental effect, maximum predicted score. The better your predictor, the more confidently it points you at the people you least need to spend on. This forecast-versus-intervention gap is Pearl's entire point, and it is the conceptual root of the persuadables reframe in Chapter 12.

The analogy to keep: a forecast predicts rain; carrying an umbrella does not *cause* rain. Prediction (Rung 1) and intervention (Rung 2) are different questions. Pearl's sharper version — *seeing* smoke versus *making* smoke — is worth holding alongside it.

## The pharma status quo is all Rung 1

The source essay names three Rung-1 practices. Here is each, checked against how the field actually describes itself.

**(a) Content optimization — slide dwell, drop-off, reaction.** CLM telemetry (`Duration_vod` seconds-per-slide, `Display_Order_vod` navigation, `Reaction_vod` +/0/− button) is mined for which slides hold attention and earn positive reactions. This is pure association: what *co-occurs* with engagement, never what engagement *causes* in NRx. (And a second, distinct problem lurks: dwell and reaction are not merely Rung-1 metrics — they are post-treatment *colliders*, so conditioning on them to recover the message→NRx effect is a separate structural error. We flag the forward link to Chapter 6 and do not develop it here.)

**(b) Rep-performance management — calls vs. quota, causation assumed.** Reps are measured on call volume against quota, and prescribing lift is *attributed* to their calling. This silently assumes calls → prescribing — the canonical Rung-1-masquerading-as-Rung-2 error. The high-converting rep may simply cover already-loyal physicians; her quota attainment measures her territory, not her causal effect. (This is the classic incrementality critique; see the advertising literature below.)

**(c) Next-best-action engines — ML optimizing engagement proxies.** This is the heart of the chapter. NBA platforms recommend message/channel/timing per physician. The verified point: they optimize *engagement proxies* — email opens, click-throughs, call duration, visit acceptance, next-prescription probability — not the causal effect of the action on prescribing. From the vendors' own public material:

- **Aktana** (a Contextual Intelligence Engine: ML plus business rules) published an ML method in the *Journal of the PMSA* for "identifying message sequences that maximize **open and click-through rates**," predicting "the probability of an email being opened or clicked through," refined over "100M+ field suggestions." (Aktana was acquired by PharmaForceIQ in January 2026; the *method* citation stands regardless of the corporate change.)
- **IQVIA Next Best Action** (part of "Orchestrated Analytics") describes "omnichannel orchestration and role-specific recommendations" and "dynamic call planning," AI/ML and generative AI for *engagement* — framed throughout as engagement optimization, not prescribing causation.
- **Indegene NBA** ("powered by orchestration and decision engines") promises "right message, right channel, right time" and reports *campaign-engagement* uplift (one top-10 pharma case: "+25% campaign engagement") — again an engagement metric, not an NRx-causal estimate.
- **Analyst framing:** IntuitionLabs' NBA guide and platform comparison describe the whole category as engagement orchestration / decisioning. (`[verify — analyst, not primary.]`)

The takeaway to land: across the biggest vendors, the optimization *target* is an engagement proxy. None publicly claims to estimate `P(NRx | do(message))`. That is the empirical basis for the chapter's "no do-operator in deployed NBA" claim — stated as a sourced inference, not a proof.

### The proxy-substitution error

Why does substituting an easy-to-measure proxy (opens, clicks, duration) for the hard target (incremental NRx) fail? Three reasons:

1. **The proxy can be uncorrelated — or anti-correlated — with the target.** An open-rate model ranks physicians who *like email*, not physicians whose *prescribing changes*. The two populations need not overlap.
2. **Optimizing a proxy degrades it as a measure.** Once a metric becomes a target, it stops measuring what it did — the Goodhart/Campbell phenomenon. (Exact phrasing and citation — Goodhart 1975; Campbell 1979 — carried `[verify]` before any direct quote.)
3. **The proxy is on the wrong rung.** Even a *perfect* open-rate predictor answers a Rung-1 question and is simply mute on the Rung-2 budget question.

The formal ancestor of this critique is real, apt, and worth reading. Bottou and colleagues (2013), studying Bing ad placement, showed that a system optimizing *measured* engagement is *not* estimating the causal effect of its own actions; predicting the consequences of system changes requires counterfactual/causal estimation. The pharma-NBA critique is the same argument in a different industry: optimizing measured response overstates causal value.

## Worked example: classifying the engine, then fixing the question

Here is the process — including the seductive wrong turn — for the opening engine.

**The wrong turn.** Confronted with the under-performing engine, the natural reflex is to improve the *model*: better features, a deeper net, more training data, a tighter calibration of predicted open rate. Suppose you succeed brilliantly and predict open rate to near-perfection.

**Why it's the wrong turn.** You have climbed nowhere. You optimized harder on a Rung-1 proxy. By Pearl's sealing result, no amount of additional Rung-1 prowess produces a Rung-2 answer — "no matter how big the data set or how deep the neural network." A perfect open-rate model still cannot tell the brand which *deliberately assigned* message moves prescribing. You have made the owl wiser without giving it a model of the mouse.

**The lesson.** The fix is not a better model; it is a *better question plus a causal model to answer it*. Reclassify what you have: the engine answers `P(open | physician, message)` — Rung 1. The budget needs `P(NRx | do(message))` — Rung 2. Naming that gap is the move. Bridging it requires the identification strategy (Chapter 4) and, ultimately, the parameterized causal model (Chapter 9). The engine was not broken; it was answering a question nobody should have funded.

**The artifact this produces.** For one named decision in your project, write the do-expression the budget needs, then write the engagement metric a current tool would optimize instead, then write one sentence on why the second cannot answer the first. (This is the named artifact below.)

**The limit.** Reclassification tells you the question is Rung 2; it does not tell you the Rung-2 answer is *identifiable* in your data. That is a separate, harder argument — relevance, independence, exclusion — and it is Chapter 4's. Knowing you are asking the right question is necessary, not sufficient.

## The 47-second-slide ambiguity, formalized

We promised in Chapter 1 to return to the 47-second slide, and here is where it gets its name. A single datum — `Duration_vod` = 47 s on the safety slide — is consistent with at least three incompatible causal stories: (1) **compelled**, reading carefully (a positive educational signal); (2) **skeptical**, hunting for the flaw (negative — and often mis-tapped "neutral" by the rep); (3) **polite while distracted** (noise). Identical telemetry, opposite causal inferences. The telemetry records *what happened*, never *why*.

This is the **epistemic gap**: the data captures behavior, not the mechanism behind it. The experienced rep who has called on Dr. Martinez forty times knows which story is true — but that knowledge is in no field of the database, and it evaporates the moment the territory changes hands. The gap is the seed of the book's central argument and an explicit forward bridge: it motivates the Causal Interview Bot (Chapter 8) and the result that structural knowledge is *mathematically necessary* to orient the graph (Chapter 7). We plant it here. We do not resolve it.

## Common misconceptions

**"With enough data and a big enough model, prediction becomes causation."** No. Pearl's sealing result is explicit: no amount of passively collected data, and no depth of network, climbs from Rung 1 to Rung 2 without a causal model.

**"Prediction plus personalization equals causal targeting."** A perfect predictor of who prescribes still ranks sure-things, not persuadables. Forecast ≠ intervention.

**"Open rate / engagement is a fine stand-in for prescribing."** Proxy substitution fails three ways: the proxy can be anti-correlated with the target, optimizing it degrades it (Goodhart), and it sits on the wrong rung regardless.

**"Vendors must be doing causal estimation under the hood."** Possibly — but none publicly describes its engine as estimating a do-expression; all describe engagement optimization. The claim is a sourced inference, not a proven universal.

## Evidence check

**Independent vs. vendor vs. hypothetical vs. contested.**

- *Independent / settled:* That interventional effects require an interventional/counterfactual comparison — that Rung-1 correlation alone cannot establish an intervention effect — is Pearl's rung-separation result (*The Book of Why*; *Causality*, 2009). The proxy-substitution critique has a formal independent treatment in Bottou et al. (2013, *JMLR*), and the advertising-incrementality literature (Lewis & Rao 2015; Gordon et al. 2019) independently shows observational ad-response overstates causal return.
- *Vendor:* The Aktana, IQVIA, and Indegene descriptions are the vendors' *own public self-descriptions* — primary as to what they *say* they optimize, but vendor-sourced. The IntuitionLabs framing is analyst, not primary (`[verify]`).
- *Hypothetical:* The opening engine is an illustrative composite.
- *Contested / opaque:* How much commercial NBA is genuinely causal versus predictive is *opaque* — vendor internals are closed, so the "no do-operator deployed" claim rests on public self-descriptions, not disclosure. State it as inference.

**Consent / compliance & patient-welfare check.** The rung diagnosis carries an ethical edge. Targeting persuadables (Chapter 12) is more *effective*, which makes the welfare question sharper, not softer: the only regulatorily defensible pathway to optimize is the *educational* one (Chapter 5, Chapter 13). A Rung-2 model that increased prescribing through reciprocity rather than education would be more effective *and* less defensible. Climbing the ladder raises the stakes of getting the pathway right; it does not lower them.

## Named Fellow artifact: classify a pharma analytic practice by rung

Pick one real, currently-deployed pharma analytic practice — an NBA engine's optimization target, a content-optimization dashboard, or a rep-performance scorecard. (Reuse the public vendor description you saved in Chapter 1's stretch exercise if you have one.) Produce a one-page classification:

1. **State the practice's optimization target** in its own words (quote the vendor/source).
2. **Classify it by rung** (1 / 2 / 3) and justify the classification structurally — what query is it actually answering, `P(Y | X)` or `P(Y | do(X))`?
3. **Write the Rung-2 question the budget needs** for the same decision, in do-notation.
4. **One sentence:** why the practice's current target cannot answer your Rung-2 question.

This is the seed of the whole identification program. You will carry the Rung-2 question into Chapter 4 and try to identify it.

## Research finding for Fellows

The finding: across the major deployed pharma NBA platforms, *the do-operator does not appear in any public method description* — every one optimizes an engagement proxy. This is not a proof that no engine secretly estimates a causal effect (the internals are closed), but it is a strong, sourced, externally checkable pattern: the field's flagship "AI" tools answer Rung-1 questions and present them against the Rung-2 decision the budget actually makes. For a Fellow, that pattern *is* the opportunity. The scarce input is not a better algorithm — causal forests and double ML are available off the shelf — it is a valid *identification strategy* on this dataset. Supplying that strategy is the research program, and it begins in the next chapter.

## Exercises

1. **(Understand)** In your own words, explain Pearl's claim that "we cannot answer questions about interventions with passively collected data, no matter how big the data set or how deep the neural network." Why does adding a causal model change this?

2. **(Analyze — classification drill)** Classify each of these ten pharma metrics as Rung 1, 2, or 3, and justify: email open rate; call attainment vs. quota; slide dwell time; reaction score; visit-acceptance rate; predicted next-Rx propensity; channel-mix lift; sample-to-Rx ratio; share-of-voice; NBA-acceptance rate. Note how many land on Rung 1 — and which one *looks* like Rung 2 but isn't.

3. **(Evaluate — produces an artifact)** Complete the named artifact: take one deployed practice, classify it by rung, write the Rung-2 do-expression the budget needs, and explain in one sentence why the practice's current target cannot answer it.

4. **(Analyze, stretch)** Read the abstract of Bottou et al. (2013) or Lewis & Rao (2015). Write one paragraph mapping their advertising argument onto pharma HCP targeting: what is the "proxy," what is the "target," and what would the experiment look like?

## Five-part AI block

**When to use AI here.** Use an LLM to rehearse the rung classification — paste a vendor description and ask it to identify the optimization target and argue which rung it sits on. It is genuinely useful for surfacing the often-buried sentence where a tool states what it optimizes. Use it also as a Socratic partner on the do-operator: have it quiz you with toothpaste/floss-style examples until the seeing-vs-doing distinction is automatic.

**When NOT to use AI here.** Do not let an LLM assert that a vendor "does causal inference" or "estimates incremental effect" unless it can quote the source — the model will pattern-match marketing language ("AI-powered," "intelligent") into a causal claim that the source does not make. That is the exact proxy-for-rigor error this chapter is about. Causal classification must trace to a quotable target, not to vibes.

**LLM exercise.** Paste this prompt:

```
Here is a description of a pharmaceutical next-best-action / engagement tool:
[paste a real vendor or analyst description — include the optimization metric it names].

1. Quote the exact phrase stating what the tool OPTIMIZES or PREDICTS.
2. Classify that target on Pearl's ladder: Rung 1 (association, P(Y|X)),
   Rung 2 (intervention, P(Y|do(X))), or Rung 3 (counterfactual). Justify
   structurally — what query is it actually computing?
3. Write the Rung-2 do-expression the BUDGET needs for this decision.
4. State in one sentence why the tool's target cannot answer that do-expression.
Do NOT upgrade the tool to a higher rung than its own words support. If it never
mentions a deliberate intervention or incremental effect, it is Rung 1.
```

**CLI exercise.** In Claude Code, build a small `rung_classifier.md` rubric file and a `practices/` folder; for three vendor descriptions you collect, write one markdown file each containing the quoted target, the rung classification with justification, and the corresponding do-expression. The deliverable is a small auditable corpus you can defend in the DAG-defense workshop.

**AI validation.** Validate by adversarial check: for every "Rung 2" the model assigns, demand the quote that mentions a *deliberate intervention* or an *incremental/counterfactual* effect. If there is no such quote, the model over-climbed — downgrade it. Cross-check at least one classification against Pearl's own definition (`P(Y|X)` vs. `P(Y|do(X))`): does the tool sever incoming arrows to the action, or merely observe the action's usual occurrence? The latter is Rung 1, full stop.

## AI Use Disclosure

This chapter was drafted by a human author from research notes; an AI assistant helped organize prose and the vendor-evidence section. Pearl's quotations are sourced to *The Book of Why*; the vendor descriptions are sourced to the companies' own public material; every `[verify]` flag in the notes (the do-calculus page, the Goodhart citation, the advertising-paper pages, the proprietary-internals inference) is carried into the chapter rather than resolved by invention.

## What would change my mind

The chapter's core claim is that deployed pharma analytics live on Rung 1 and that the budget question is Rung 2. I would revise if: (1) a major vendor published a method that explicitly estimates `P(NRx | do(message))` with a stated identification strategy — not an engagement proxy dressed in causal vocabulary (the evidence check actively looks for this and does not find it); (2) Pearl's rung-separation result were shown not to apply here — e.g., if the engagement proxy were demonstrated to be a sufficient statistic for the incremental effect under defensible assumptions (it is not, in general, but a specific market could surprise us); or (3) it turned out that open rate and incremental NRx are, empirically on this data, tightly aligned — collapsing the proxy-substitution objection. Each is checkable; none is currently supported.

## Still puzzling

What *are* the vendors actually doing internally — is the absence of do-language a genuine absence of causal estimation, or merely a marketing choice? We cannot see inside, so the claim stays an inference. How tightly *is* an engagement proxy coupled to incremental NRx in real markets — is it sometimes "close enough," and if so, when? And the question that propagates forward: even granting we should ask the Rung-2 question, is it *identifiable* in this observational data at all — and if so, through what variation? The honest answer is "sometimes, through the training-cohort rollout, if its assumptions hold" — which is precisely the bet Chapter 4 places and tests.

## Summary

Pharma is not failing to analyze its rep-visit data; it is analyzing it on the wrong rung. Pearl's ladder has three levels — association (`P(Y|X)`), intervention (`P(Y|do(X))`), counterfactual — and they are sealed: no quantity of passively collected data and no depth of network climbs from Rung 1 to Rung 2 without a causal model. Content optimization, rep-performance management, and the NBA engines that allocate the budget all optimize engagement proxies — Rung-1 quantities — while the budget's real question, which message *deliberately assigned* causes more prescribing, is Rung 2. Substituting the proxy fails three ways (anti-correlation, Goodhart, wrong rung), as the advertising literature independently shows. The fix is not a better model; it is the right question plus a causal model to answer it. Naming that question, in do-notation, is your artifact — and identifying it is the rest of the book.

## Key terms

- **Pearl's Ladder of Causation:** the three tiers of causal queries — association (seeing), intervention (doing), counterfactual (imagining) — each strictly more powerful than the one below.
- **Rung 1 — Association:** regularities in passive observation; the object is `P(Y | X)`. Correlation and ordinary regression live here.
- **Rung 2 — Intervention:** the effect of a deliberate action, `P(Y | do(X))`; the rung the budget question occupies.
- **Rung 3 — Counterfactual:** reasoning about a specific unit in a world contrary to fact; the rung individual recommendations need (Chapter 9).
- **do-operator, `do(X = x)`:** an external manipulation that sets X to x, severing X's incoming arrows; `E[Y | do(X=x)]` differs in general from `E[Y | X=x]`.
- **Sealing (rung-separation):** Pearl's result that Rung-1 data alone cannot answer Rung-2 questions; only a causal model bridges the gap.
- **Proxy-substitution error:** optimizing an easy proxy (opens, clicks) for a hard target (incremental NRx); fails via anti-correlation, Goodhart degradation, and rung mismatch.
- **Engagement proxy:** an observable engagement metric (open rate, click-through, dwell, visit acceptance) used as a stand-in for the causal prescribing effect.
- **Epistemic gap:** the data records what happened, not why; the 47-second-slide ambiguity is its emblem.
- **Persuadable vs. sure-thing (preview):** a physician whose prescribing the message *changes* vs. one who would prescribe regardless (Chapter 12).

## Bridge

The data has the variation to answer the Rung-2 question — but where, exactly, and under what conditions? Chapter 4 finds it in the training-cohort rollout, sets up the instrumental-variables framework (treatment, instrument, outcome), and teaches you to test the three conditions — relevance, independence, exclusion — and to design the falsification test that must pass before any effect is believed.

## Further reading

- Pearl, J., & Mackenzie, D. (2018). *The Book of Why: The New Science of Cause and Effect.* (Chapter 1, "The Ladder of Causation" — the three rungs, the do-operator, the toothpaste/floss examples, and the rung-separation result quoted in this chapter.)
- Pearl, J. (2009). *Causality: Models, Reasoning, and Inference* (2nd ed.). Cambridge University Press. (The do-operator and do-calculus; page for the `do(X=x)` definition carried `[verify]`.)
- Bottou, L., Peters, J., Quiñonero-Candela, J., Charles, D. X., Chickering, D. M., Portugaly, E., Ray, D., Simard, P., & Snelson, E. (2013). "Counterfactual Reasoning and Learning Systems: The Example of Computational Advertising." *Journal of Machine Learning Research*, 14, 3207–3260. (Preprint arXiv:1209.2355. The formal ancestor of the proxy-substitution critique.)
- Lewis, R. A., & Rao, J. M. (2015). "The Unfavorable Economics of Measuring the Returns to Advertising." *Quarterly Journal of Economics*, 130(4), 1941–1973. (`[verify]` pages. Even very large observational samples cannot pin down ad ROI; experiments are needed.)
- Gordon, B. R., Zettelmeyer, F., Bhargava, N., & Chapsky, D. (2019). "A Comparison of Approaches to Advertising Measurement: Evidence from Big Field Experiments at Facebook." *Marketing Science*, 38(2), 193–225. (`[verify]` pages. Observational methods diverge sharply from RCT ground truth.)
- Aktana, "Aktana's Machine Learning Method… Published in PMSA Journal," and IQVIA "Orchestrated Analytics / Next Best Action" and Indegene "Next Best Action" public pages. (Vendor self-descriptions of the engagement-optimization target.)
