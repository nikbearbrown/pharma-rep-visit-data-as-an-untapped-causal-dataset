# Chapter 5: The Causal Graph and the Three Pathways

## 1. Overview

Chapter 4 handed you a number: a defensible, falsification-tested estimate of the *total* effect of a message on prescribing, identified from the cohort-rollout instrument. A total effect answers "does the message move NRx, and by how much, for compliers." It is a real achievement and, for many purposes, it is not enough.

The reason is that a message can move prescribing through more than one channel, and the channels are not interchangeable. A physician might prescribe more because she *learned* something — the clinical case for the drug changed in her mind. Or because her rep left *samples and co-pay cards* that lowered the friction of trying it. Or because she accepted a sponsored *meal* and now feels a faint social obligation she would deny if you asked her. The total effect lumps all three together into one number. But the three carry sharply different ethical and regulatory weight: the educational channel is the only one a brand can lawfully and openly try to drive; the reciprocity channel is the one that draws regulatory scrutiny and accounts for the bulk of physician-directed promotional spend. Two visits with *identical* NRx lift can be, ethically and legally, completely different events.

This chapter teaches you to draw the message→prescribing relationship as a **three-pathway mediation graph** and to reason about pulling the total effect apart. You will learn the counterfactual vocabulary of mediation — direct, indirect, and controlled effects — and, crucially, you will learn the chapter's hardest and most load-bearing lesson: with three correlated mediators, the clean "share through education" you would like to report is **generally not point-identified**. The honest tool is the **interventional (in)direct effect** under stated assumptions, not the regression-coefficient shrinkage you may have been taught. Getting this right is what separates a defensible pathway claim from a confident wrong one.

What you will be able to do: draw the educational / relationship-maintenance / reciprocity pathways for a real message, tie each to its regulatory status, explain why naive mediation analysis fails here, and state what *can* honestly be claimed about the decomposition and under what assumptions.

## 2. Learning objectives

By the end of this chapter you will be able to:

- **(Apply)** Draw the message→prescribing graph as a three-pathway mediation — educational (M1), relationship-maintenance (M2), reciprocity (M3) — marking which arrows you actually have data for.
- **(Analyze)** Predict how conflating the pathways misleads both investment and compliance, using the two-identical-lifts case.
- **(Evaluate)** Tie each pathway to its regulatory status and judge whether a proposed decomposition method (Baron–Kenny vs interventional effects) is defensible for correlated mediators.
- **(Create)** State the estimand and assumptions for an honest education-vs-non-education contribution claim on this dataset.

## 3. Opening case (failure-first)

A brand runs two tactics in parallel and measures both against NRx. Tactic one is a redesigned clinical message — a tightened educational deck on the drug's outcomes data. Tactic two is a routine of sponsored lunches for the same physicians. At quarter's end the analytics team reports the headline: both tactics produced **the same NRx lift** per physician reached. The lunches are cheaper per contact. The recommendation writes itself: shift budget toward lunches.

The recommendation is a disaster waiting to be discovered, and the reason is that the team treated the *total* effect as the decision variable. The two lifts are equal in magnitude and opposite in kind. The deck's lift, if it is real, ran through the **educational** pathway — the physician changed her prescribing because the clinical case changed. The lunch's lift ran through the **reciprocity** pathway — a sense of obligation, not new information. By doubling down on lunches the brand has (a) bought heavier exposure on the legally and ethically scrutinized channel, the one that draws Sunshine Act attention and reputational risk, and (b) congratulated itself for "the message working" when the message had nothing to do with it.

The error is not a bad estimate. The total-effect number was fine. The error is asking a number that *cannot distinguish channels* to make a decision that *depends entirely on the channel*. The fix is not a better total-effect estimate; it is a decomposition — and, as we will see, an honest decomposition is far harder than the textbook mediation analysis the team would reach for.

## 4. Core sections

### 4.1 The three-pathway graph

Start by drawing what we believe is happening. Carrying the instrument forward from Chapter 4, training cohort (Z) shifts the message variant (D), and the message then reaches prescribing (Y = NRx) through three mediating channels:

```
                              ┌──▶ M1 educational ──┐
                              │   (knowledge/        │
 cohort (Z) ──▶ message (D) ──┼──▶  attitude)        ├──▶ NRx (Y)
                              │                      │
                              ├──▶ M2 relationship ──┤
                              │   (samples, co-pay,  │
                              │    follow-up)        │
                              │                      │
   Open Payments meals (M3 reciprocity) ────────────┘
```

A **mediator** is a variable that lies on the causal path *between* treatment and outcome — treatment affects the mediator, and the mediator affects the outcome — so that part of the treatment's effect flows *through* it. The three mediators here:

- **M1 — educational.** D → knowledge/attitude → Y. The physician learns something; the clinical case for the drug changes in her assessment, and prescribing follows. **This is the only legally drivable pathway** — a brand may lawfully and openly try to inform prescribing decisions with accurate clinical data.
- **M2 — relationship-maintenance.** D → samples, co-pay/coupon cards, follow-up → Y. The rep lowers the friction of trying the drug. Amber, not red: regulated, sometimes legitimate (samples can help patients who cannot afford a trial), but not "education."
- **M3 — reciprocity.** Open Payments meals/gifts → Y. The mechanism is social obligation, not information. This is the channel behind the bulk of physician-directed promotional spend — treated in the source essay as the "$40B pathway" (see §7 on that figure) — and the one that carries the heaviest regulatory and ethical exposure.

A practical note you must mark on your own graph: **which arrows do you actually have data for?** M2 is relatively measurable from rep-entered fields (samples and co-pay cards left). M3 is measurable from public Open Payments data (meals by NPI and date). M1 is the hardest — "educational attitude" lives in rep free-text call notes (the objection resolved, the clinical question answered), not in a clean structured field. That M1 is the legally important pathway *and* the hardest to measure is not a coincidence you can wish away; it is part of why the rep-knowledge elicitation of Chapter 8 exists, and it is a standing limitation on any education-share claim.

### 4.2 The vocabulary of mediation (counterfactual, not Baron–Kenny)

To talk precisely about "how much goes through education" you need counterfactual notation. Write Y(d, m) for the prescribing a physician *would* show if the message were set to d and the mediator set to m, and M(d) for the mediator value the message d would induce. Then:

- **Total Effect (TE):** Y(1, M(1)) − Y(0, M(0)). The message switched on (with its mediator wherever the message pushes it) versus off. This is what the Chapter 4 IV estimate targets.
- **Natural Direct Effect (NDE):** Y(1, M(0)) − Y(0, M(0)). The effect of the message while *holding the mediator at the value it would have taken with no message*. The part of the effect that does **not** flow through M.
- **Natural Indirect Effect (NIE):** Y(1, M(1)) − Y(1, M(0)). The effect of moving the mediator from its no-message value to its message-induced value, with the message on. The part that flows *through* M. These two add up cleanly: **TE = NDE + NIE**.
- **Controlled Direct Effect (CDE):** Y(1, m) − Y(0, m). The message effect when the mediator is *fixed by intervention* at some chosen value m. This is the policy-relevant quantity — "what would the message do if we *blocked* the sample channel and set samples to zero?" — but it does **not** generally sum to TE.

The NDE/NIE split is the natural way to phrase "how much went through education," and the CDE is the natural way to phrase "what if we turned a pathway off." Hold both; they answer different questions.

### 4.3 Teach Baron–Kenny, then break it

Almost everyone arrives with one mediation method in hand, and it is the wrong one here:

> "Run a Baron–Kenny mediation: regress NRx on the message, add the mediator, watch how much the message coefficient shrinks — that shrinkage is the pathway."

Baron & Kenny (1986, *Journal of Personality and Social Psychology* 51(6):1173–1182) is the product-of-coefficients method, and it is foundational in the social sciences. It is also wrong, and sometimes dangerous, for this problem, for reasons that are structural rather than a matter of tuning. It assumes **no exposure–mediator interaction** (that the message's effect on prescribing is the same regardless of how much the mediator moved) and **no nonlinearity** — but NRx is a count, the relationships are plausibly nonlinear, and interaction is exactly what you would expect (a sample matters more to a physician the message has already half-persuaded). It **conflates a statistical event with a causal one**: a coefficient shrinking when you add M is a fact about two regressions, not proof that the effect *flows through* M. And it silently assumes **no unmeasured mediator–outcome confounding** — an assumption it never states and that is wildly implausible here, since a physician's general openness drives engagement, prescribing, *and* willingness to take a meal all at once.

The counterfactual framework (Robins & Greenland 1992, *Epidemiology* 3(2):143–155; Pearl 2001, *Proc. UAI 17*:411–420, "the Mediation Formula"; VanderWeele 2015, *Explanation in Causal Inference*, Oxford UP; VanderWeele 2016, *Annual Review of Public Health* 37:17–32) replaces "watch the coefficient shrink" with "define NDE/NIE/CDE as counterfactual contrasts and state the assumptions under which the data identify them." It does not make the problem easy. It makes the difficulty *visible*, which is the whole point.

### 4.4 Why mediation is harder than the total effect — the cross-world assumption

Here is the conceptual core of the chapter. Identifying NDE/NIE for even a *single* mediator requires **four** no-unmeasured-confounding assumptions, all conditional on measured covariates (VanderWeele 2016): (1) no unmeasured exposure–outcome confounding, (2) no unmeasured mediator–outcome confounding, (3) no unmeasured exposure–mediator confounding, and (4) **no mediator–outcome confounder that is itself affected by the exposure**. The fourth is the **cross-world** assumption, and it has no analogue in total-effect estimation.

Why "cross-world"? Because the NDE/NIE contrasts compare quantities like Y(1, M(0)) — the outcome in a world where the message is *on* but the mediator sits at its *message-off* value. That combination never occurs in any real or randomized world; you are reasoning across two counterfactual worlds at once. As a consequence, the assumption is **untestable even in a perfect RCT**. This is the hinge: randomizing the message (as the IV did, in effect) secures assumptions (1) and (3), which is enough for the *total* effect — but it does **nothing** for (2) and (4), because the *mediator* is never randomized. Robins & Greenland (1992) proved exactly this: direct and indirect effects are **not separately identifiable** from randomization of the exposure alone.

The upshot for this book, and you should state it to any partner in these words: **the total message effect from the IV is far more credible than any single pathway share.** The total effect leaned on randomization-like variation we tested. The pathway split leans on additional, untestable, cross-world assumptions about mediators we never manipulated. The decomposition is worth doing — the ethics and economics demand it — but it is a different epistemic creature, and conflating its credibility with the total effect's is the subtler cousin of the opening case's error.

### 4.5 Three correlated mediators: not point-identified

Now stack the difficulty. We do not have one mediator; we have three (M1 education, M2 relationship, M3 reciprocity), and they are **correlated and mutually causal**. A physician's general openness drives engagement with the message *and* acceptance of samples *and* acceptance of meals *and* prescribing. Worse, the mediators plausibly affect each other: a persuasive educational exchange may make a physician more receptive to a sample; a meal may open the door to a longer detailing conversation.

This wrecks the clean per-pathway split. When one mediator is a **post-exposure confounder of another** — when M2 sits on the path between the exposure and M1's effect on Y — the natural three-way "share through M1, share through M2, share through M3" decomposition is **generally not point-identified**, full stop. No amount of regression recovers it; the data are consistent with many different splits. This is the single most important honesty point in the chapter, and the source essay's phrasing — "estimate each pathway's direct contribution" — must be read with this caveat or it oversells what is possible.

What *is* identifiable, under stated assumptions, is coarser and still useful: the **joint** indirect effect through the *set* {M1, M2, M3}, and the **direct** effect not through any of them. And there is a defensible tool for going further than that: **interventional (randomized-interventional) (in)direct effects** (VanderWeele, Vansteelandt & Robins 2014, *Epidemiology* 25(2):300–306 [verify pages]), extended to multiple mediators with an *exact* decomposition even under unknown mediator ordering and between-mediator confounding by **Vansteelandt & Daniel (2017, *Epidemiology* 28(2):258–265)**. The trade-off you accept in return: interventional effects drop the untestable cross-world assumption, but they answer a slightly different question — they ask what happens if you set each mediator to a *random draw* from its message-induced distribution (a "stochastic intervention") rather than to its natural value — and the 2014 version does not sum exactly to TE. You buy weaker assumptions with a more contingent interpretation. That is the honest trade, and naming it is the methodological point of the chapter.

## 5. Worked example on this dataset

**Step 1 — Draw the graph and mark the data.** For a specific message transition, lay out D (message, from `Call2_Key_Message_vod__c`) → M1 (educational attitude, proxied by rep-noted objection resolution and clinical questions in free-text notes); D → M2 (samples and co-pay cards left, from rep-entered fields); and M3 (Open Payments meals, by NPI and date) → Y (NRx). Annotate each arrow with its data source and confidence. You will immediately notice M1 has the weakest data — a free-text proxy — and is the legally most important. Write that down as a limit, not a footnote.

**Step 2 — A dead end, walked through.** You want "the education pathway," so you run a Baron–Kenny mediation: regress NRx on the message; add the educational proxy; control for samples and meals; read the shrinkage as the education share. Stop and look at what you assumed. You assumed the three mediators do not confound each other — but a physician's openness drives all three, and an educational exchange can change sample uptake. M2 and M3 are post-exposure confounders for M1's effect. **Lesson:** the clean "share through education" you just reported is not point-identified; the shrinkage number is an artifact of which regression you happened to run, and a colleague running the mediators in a different order would get a different "education share."

**Step 3 — The defensible move.** Retreat to what the data can support. Estimate the **joint** indirect effect through {M1, M2, M3} and the direct effect not through any of them — both identifiable under stated no-confounding assumptions. To go further, apply the **interventional indirect effects** of Vansteelandt & Daniel (2017), which give an exact additive decomposition across the three mediators without the cross-world assumption, and report the result *as* an interventional (stochastic-intervention) quantity, not as a natural pathway share. Run a sensitivity analysis: how strong would an unmeasured mediator–outcome confounder have to be to overturn the education-vs-non-education conclusion?

**The limit.** Even the interventional decomposition is assumption-heavy, and the M1 proxy is weak. The honest claim is bounded: "under these stated assumptions, an estimated [X] of the message's effect is attributable to non-educational channels (relationship + reciprocity), with sensitivity analysis showing the conclusion survives confounders up to strength [Y]." That is enough to drive the investment-and-ethics argument — it tells the brand whether it is buying education or obligation — without pretending to a precision the data cannot deliver. The bridge: some of the variables you might be tempted to condition on here are not mediators at all but traps → Chapter 6.

## 6. Common misconceptions

- **"Total effect is the decision variable."** It is the *detection* variable. The decision depends on the channel; two identical totals can demand opposite actions.
- **"Baron–Kenny coefficient shrinkage measures the pathway."** Shrinkage is a statistical event under strong, often-violated assumptions (no interaction, no mediator–outcome confounding). It is not a causal pathway share.
- **"Randomizing the message lets me decompose the effect."** Randomization secures the total effect; it does nothing for the mediator–outcome assumptions (2) and (4), which the mediator's non-randomization leaves wide open (Robins & Greenland 1992).
- **"With three mediators I can still get the share through each."** Generally false: with correlated, mutually-causal mediators the natural per-pathway split is not point-identified. Use interventional effects and report the joint/direct split honestly.
- **"Meals cause prescribing — the odds ratios prove it."** The DeJong et al. odds ratios are *associations*; the authors say so. Reciprocity is association-strong and causal-contested (see §7).

## 7. Evidence check and consent/compliance & patient-welfare check

**Evidence check — classify what you are leaning on.**

- *Independent / peer-reviewed (strong on method).* The counterfactual mediation framework (Robins & Greenland 1992; Pearl 2001; VanderWeele 2015, 2016) and the interventional-effects results (VanderWeele, Vansteelandt & Robins 2014; Vansteelandt & Daniel 2017; Daniel et al. 2015, *Biometrics* 71(1):1–14) are peer-reviewed and current.
- *Independent but association-strong / causal-contested.* DeJong et al. (2016, *JAMA Internal Medicine* 176(8):1114–1122) linked Open Payments meals (Aug–Dec 2013) to Medicare Part D prescribing across ~279,669 physicians: a single sponsored meal (often <$20) was *associated with* higher promoted-brand prescribing (rosuvastatin OR ≈ 1.18; nebivolol OR ≈ 1.70), rising with more and larger meals. The authors explicitly caution this is **association, not cause** — confounding by indication/preference and reverse causation (reps target high-volume prescribers). Treat the M3 pathway as real-association, contested-causation. Newham & Valente (2022, arXiv:2203.01778 [verify title/venue]) is a more recent econometric treatment of detailing/promotion useful as a complement.
- *Hypothetical / author-constructed.* The two-identical-lifts opening case and the synthetic-data demonstration are illustrative, not observed.
- *Contested / [verify].* The **"$40B" promotional-spend figure is approximate/[verify]**: primary sources cluster lower — Schwartz & Woloshin (2019, *JAMA* 321(1) [verify exact cite]) report U.S. medical marketing rising from $17.7B (1997) to $29.9B (2016), of which roughly $5.3B was detailing and ~$16.4B free samples (≈$20B+ physician-directed). Cite Schwartz & Woloshin and footnote the $40B as approximate. Exact pages for VanderWeele, Vansteelandt & Robins (2014) and VanderWeele & Vansteelandt (2014, *Epidemiologic Methods* 2(1):95–115) are [verify].

**Consent / compliance & patient-welfare check.** The pathways *are* the regulatory map, so this check is central, not appended. **M1 (education) is the only legally drivable pathway** — a brand may openly try to inform prescribing with accurate clinical data. **M3 (reciprocity)** is the channel under Sunshine Act / anti-kickback scrutiny; driving it deliberately is where commercial activity becomes legal exposure. **M2 (relationship)** sits in between. This means the decomposition is not a nicety — a brand that cannot tell whether its lift runs through M1 or M3 cannot tell whether it is doing medicine or buying obligation. On patient welfare: a pathway split is also a welfare instrument. Education-driven prescribing is defensible to the extent the education is accurate and the drug is warranted; reciprocity-driven prescribing moves what patients receive *without* a clinical reason, which is the welfare harm the regulation exists to prevent. Estimating M3 honestly is therefore a patient-welfare act, not only a compliance one. (Public/IP firewall as always: meals are public Open Payments data; the M1 free-text proxy and M2 fields are taught on synthetic data, with the partner replicating internally.)

## 8. Named Fellow artifact: the three-pathway graph

Produce an annotated **three-pathway mediation graph** for a specific message transition in your thread's dataset. It must contain:

1. **The graph** (ASCII or drawn): Z → D fanning into M1, M2, M3, all into Y, with each arrow labeled by (a) its data source and (b) a confidence mark for whether you can measure it.
2. **The regulatory color-coding**: M1 marked drivable (green), M2 regulated (amber), M3 reciprocity/$40B-pathway (red), with one line each on the regulatory status — this color map carries forward to the Chapter 13 handoff.
3. **The estimand statement**: write out, in counterfactual notation, the *honest* quantity you will report — the joint indirect effect through {M1,M2,M3}, the direct effect, and (if attempted) the interventional decomposition — *not* a naive per-pathway product-of-coefficients.
4. **The assumptions and the M1 caveat**: list the no-confounding assumptions your estimand needs, name the cross-world / interventional trade-off, and flag explicitly that the M1 educational signal is a weak free-text proxy whose improvement depends on the elicitation work of Chapter 8.

A graph that promises a clean "share through education" is marked incomplete: the artifact must encode the non-identification honesty.

## 9. Research finding for Fellows

Rung-1 NBA engines cannot separate the pathways — they optimize an engagement proxy and are structurally blind to whether a lift is education or reciprocity. A Living Model built on the parameterized three-pathway graph can, under stated assumptions, estimate the joint and (interventionally) the per-channel contributions, making the education-vs-reciprocity split *visible* for the first time as a decision variable. That visibility is the finding: it converts the ethics from a compliance afterthought into a number the brand sees alongside the lift. The open empirical question is not whether the decomposition is worth having — it plainly is — but how much the weak M1 proxy and the interventional-effect assumptions degrade its trustworthiness in practice; that is a question the rep-elicitation work (Chapters 7–8) is designed to attack.

## 10. Exercises

1. **(Understand)** Define NDE, NIE, and CDE in one sentence each in plain language, and state which one answers "what if we banned sponsored meals for this brand?"
2. **(Analyze)** Take the two-identical-lifts opening case. Write the recommendation the brand *should* make after a decomposition shows the lunch lift runs through M3, and contrast it with the recommendation the total effect alone produced.
3. **(Apply+)** On the synthetic dataset (which has a known ground-truth pathway split), run a Baron–Kenny mediation for the education share, then re-run with the mediators entered in a different order. Report how much the "education share" changes, and explain why the change demonstrates non-identification.
4. **(Create — produces an artifact)** Build the §8 three-pathway graph for a message transition in your thread's data, including the regulatory color map, the honest estimand statement, and the assumptions-and-M1-caveat list.

## 11. Five-part AI block

**When to use AI.** Use an LLM to render and label the DAG, to recall the precise definitions and identifying assumptions of NDE/NIE/CDE and the interventional effects, and to scaffold mediation code (e.g., the `mediation` family of estimators, or a Vansteelandt–Daniel interventional-effects implementation). It is also useful for drafting the sensitivity-analysis setup once you have stated your estimand.

**When NOT to use AI.** Do not let an LLM tell you that your three-pathway split is identified, or hand you a Baron–Kenny "education share" as if it were causal — both are the exact errors this chapter exists to prevent, and a model trained on a literature full of naive mediation will reproduce them confidently. Do not outsource the regulatory classification of a pathway; M1-vs-M3 status is a legal/domain judgment routed to counsel (Chapter 13), not a model output.

**LLM exercise (ready-to-paste prompt):**

> I am decomposing the effect of a sales message on prescribing into three correlated, mutually-causal mediators: educational attitude (M1), samples/co-pay cards (M2), and sponsored meals (M3). A colleague wants to use Baron–Kenny product-of-coefficients to report the "share through education." Do three things: (1) explain, structurally, why the natural per-pathway split is generally not point-identified when mediators are correlated and affect each other; (2) name the cross-world assumption and why randomizing the message does not rescue the decomposition; (3) describe the interventional (in)direct effects approach (Vansteelandt & Daniel 2017) as the defensible alternative, stating exactly what interpretation I trade away. Do not produce a numeric education share.

**CLI exercise (Claude Code / terminal).** In your repo, ask Claude Code to write a script that estimates the education share two ways on the synthetic data — naive Baron–Kenny with the mediators in two different orders, and a joint-indirect-vs-direct decomposition — and prints all results side by side with the known ground-truth split. The script's purpose is to *demonstrate* the non-identification: the two Baron–Kenny orderings should disagree, and both should miss ground truth, while the joint/direct split should be honest about what it does and does not pin down.

**AI validation.** Validate any decomposition code against the synthetic dataset's known pathway structure. The correct behavior is *not* that the method nails the per-pathway shares — it should not, because they are not identified — but that the *joint* indirect effect is recovered and the naive per-pathway numbers visibly fail to match ground truth and disagree across orderings. If an AI-generated pipeline reports a stable, confident education share, treat that stability as a bug.

## 12. AI Use Disclosure

This chapter was composed from human-prepared research notes with citations already gathered; an AI assistant helped turn notes into prose. The central identification-honesty claim — that the natural three-pathway split is generally not point-identified and that interventional effects are the defensible tool — is carried directly from the notes and the cited literature, not generated. Empirical claims are sourced or flagged [verify]; the $40B figure is explicitly approximate. The regulatory classification of pathways is flagged as a counsel matter, not adjudicated here. No proprietary data was used.

## 13. What would change my mind

I would soften the non-identification verdict if the three mediators turned out to be, in a given dataset, effectively un-correlated and non-interacting — if a physician's openness did *not* jointly drive education, samples, and meals, and the mediators did not affect each other — because then the per-pathway split would be much closer to identified and a careful natural-effects analysis would be defensible. I would harden the chapter's caution in the opposite direction if sensitivity analyses on real data routinely showed the education-vs-reciprocity conclusion flipping under modest unmeasured confounding, which would mean even the joint/interventional claims are too fragile to hand a partner. And I would revise the M3 framing if a credible quasi-experimental study (not an association) established a clean causal meal effect, moving reciprocity from "contested" toward "established."

## 14. Still puzzling

The M1 measurement problem is genuinely unresolved. "The physician learned something" is the legally central quantity and the one with the worst data — a free-text proxy that conflates a thoughtful clinical exchange with a rep's optimistic note. Whether the Chapter 8 elicitation can produce an M1 signal good enough to support an interventional decomposition is an open empirical question, not a settled method. It is also unclear how to communicate an *interventional* (stochastic-intervention) pathway estimate to a brand team that wants "the percent through education" — the honest quantity is not the quantity they asked for, and the translation problem is real.

## 15. Summary

A message moves prescribing through three channels — education (M1, legally drivable), relationship-maintenance (M2), and reciprocity (M3, the heavily scrutinized $40B-ish pathway) — and the total effect from Chapter 4 lumps them together. The decision depends on the channel, so the total is a detection variable, not a decision variable. The counterfactual vocabulary (TE = NDE + NIE; CDE for "block a pathway") defines what a decomposition means. Baron–Kenny coefficient-shrinkage cannot deliver it: mediation needs four assumptions including the untestable cross-world one, and with three correlated, mutually-causal mediators the natural per-pathway split is generally not point-identified. The honest tools are the joint indirect effect, the direct effect, and interventional (in)direct effects (Vansteelandt & Daniel 2017), all under stated assumptions with sensitivity analysis — and the total effect remains more credible than any pathway share. M1, the legally central pathway, is also the hardest to measure, which sets up the rep-elicitation work to come.

## 16. Key terms

- **Mediator** — a variable on the causal path between treatment and outcome, through which part of the effect flows.
- **Educational / relationship-maintenance / reciprocity pathways (M1/M2/M3)** — the three channels from message to prescribing, with green/amber/red regulatory status respectively.
- **Total / Natural Direct / Natural Indirect / Controlled Direct Effect (TE/NDE/NIE/CDE)** — counterfactual contrasts decomposing a treatment effect; TE = NDE + NIE; CDE fixes the mediator by intervention.
- **Cross-world assumption** — the untestable no-exposure-induced-mediator-outcome-confounder assumption required for natural-effect identification; has no analogue in total-effect estimation.
- **Point identification (and its failure)** — whether the data pin down a unique value for an estimand; with correlated, mutually-causal mediators the per-pathway split generally is not point-identified.
- **Interventional (in)direct effects** — a decomposition (Vansteelandt & Daniel 2017) avoiding the cross-world assumption at the cost of a stochastic-intervention interpretation; gives an exact split across multiple mediators.
- **Baron–Kenny** — the classic product-of-coefficients mediation method; assumes no interaction and no mediator–outcome confounding, inappropriate for this problem.

## 17. Bridge

But some "features" you'd condition on are traps. The mediators in this chapter sit *downstream* of treatment, and for the pathway split you deliberately model them — yet exactly because they are downstream, conditioning on the wrong ones to estimate the *total* effect manufactures bias. Slide dwell time and reaction scores look like helpful engagement controls; they are post-treatment colliders, and physician consent is a selection collider on the sampling frame. Chapter 6 shows why "control for more stuff" is the bug, not the fix.

## 18. Further reading

- VanderWeele, T. (2016). "Mediation Analysis: A Practitioner's Guide." *Annual Review of Public Health* 37:17–32.
- VanderWeele, T. (2015). *Explanation in Causal Inference: Methods for Mediation and Interaction.* Oxford University Press.
- Robins, J. & Greenland, S. (1992). "Identifiability and Exchangeability for Direct and Indirect Effects." *Epidemiology* 3(2):143–155.
- Pearl, J. (2001). "Direct and Indirect Effects." *Proceedings of UAI 17*, 411–420.
- Vansteelandt, S. & Daniel, R. (2017). "Interventional Effects for Mediation Analysis with Multiple Mediators." *Epidemiology* 28(2):258–265.
- VanderWeele, T., Vansteelandt, S. & Robins, J. (2014). "Effect Decomposition in the Presence of an Exposure-Induced Mediator-Outcome Confounder." *Epidemiology* 25(2):300–306. [verify pages]
- Daniel, R. et al. (2015). "Causal Mediation Analysis with Multiple Mediators." *Biometrics* 71(1):1–14.
- Baron, R. & Kenny, D. (1986). "The Moderator–Mediator Variable Distinction in Social Psychological Research." *Journal of Personality and Social Psychology* 51(6):1173–1182.
- DeJong, C. et al. (2016). "Pharmaceutical Industry-Sponsored Meals and Physician Prescribing Patterns for Medicare Beneficiaries." *JAMA Internal Medicine* 176(8):1114–1122.
- Schwartz, L. & Woloshin, S. (2019). "Medical Marketing in the United States, 1997–2016." *JAMA* 321(1). [verify exact cite]
