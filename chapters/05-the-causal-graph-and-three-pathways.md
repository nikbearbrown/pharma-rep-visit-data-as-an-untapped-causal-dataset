# Chapter 5 — The Causal Graph and the Three Pathways
*Two identical lifts. Opposite ethical meaning. One number that cannot tell the difference.*

A brand runs two tactics in parallel and measures both against NRx. Tactic one is a redesigned clinical message — a tightened educational deck on the drug's outcomes data. Tactic two is a routine of sponsored lunches for the same physicians. At quarter's end the analytics team reports: both tactics produced the same NRx lift per physician reached. The lunches are cheaper per contact. The recommendation writes itself: shift budget toward lunches.

The recommendation is a disaster waiting to be discovered.

The two lifts are equal in magnitude and opposite in kind. The deck's lift, if it is real, ran through the educational pathway — the physician changed her prescribing because the clinical case changed in her mind. The lunch's lift ran through the reciprocity pathway — a sense of social obligation, not new information. By doubling down on lunches the brand has bought heavier exposure on the legally and ethically scrutinized channel, the one that draws Sunshine Act attention and reputational risk, while congratulating itself for "the message working" when the message had nothing to do with it.

The error is not a bad estimate. The total-effect number was fine. The error is asking a number that *cannot distinguish channels* to make a decision that *depends entirely on the channel.* The fix is not a better total-effect estimate. It is a decomposition — and, as we will see, an honest decomposition is far harder than the textbook mediation analysis the team would reach for.

---

Chapter 4 handed you a total effect: a defensible estimate of how much the message moved NRx, identified from the cohort-rollout instrument. That is a real achievement. It is also not enough, because the message can move prescribing through more than one channel, and the channels are not interchangeable.

A physician might prescribe more because she *learned* something — the clinical case for the drug changed in her assessment. Or because the rep left *samples and co-pay cards* that lowered the friction of trying it. Or because she accepted a sponsored *meal* and now feels a faint social obligation she would deny if you asked her. The total effect lumps all three into one number. But the three carry sharply different ethical and regulatory weight. The educational channel is the only one a brand can lawfully and openly try to drive. The reciprocity channel is the one that draws regulatory scrutiny and accounts for the bulk of physician-directed promotional spend. Two visits with identical NRx lift can be, ethically and legally, completely different events.

Start by drawing what we believe is happening.

```
                              ┌──▶ M1 educational ──────┐
                              │   (knowledge/attitude)  │
 cohort (Z) ──▶ message (D) ──┤                         ├──▶ NRx (Y)
                              ├──▶ M2 relationship ──────┤
                              │   (samples, co-pay,     │
                              │    follow-up)           │
                              │                         │
   Open Payments meals ───────┴──▶ M3 reciprocity ──────┘
```

A **mediator** is a variable that lies on the causal path between treatment and outcome — the treatment affects it, and it affects the outcome, so part of the treatment's effect flows *through* it. Here there are three.

**M1 — educational.** Message → knowledge and attitude → prescribing. The physician learns something; the clinical case for the drug changes in her assessment, and prescribing follows. This is the only legally drivable pathway. A brand may lawfully and openly try to inform prescribing decisions with accurate clinical data.

**M2 — relationship-maintenance.** Message → samples, co-pay cards, follow-up → prescribing. The rep lowers the friction of trying the drug. Amber, not red: regulated, sometimes legitimate — samples can help patients who cannot afford a trial — but not education.

**M3 — reciprocity.** Open Payments meals and gifts → prescribing. The mechanism is social obligation, not information. This is the channel behind the bulk of physician-directed promotional spend — the "$40B pathway" is an approximate figure drawn from U.S. medical marketing data, with the best independent estimate from Schwartz & Woloshin (*JAMA* 2019 `[verify exact cite]`) showing physician-directed spend rising from $17.7B in 1997 to $29.9B in 2016 — and it carries the heaviest regulatory and ethical exposure.

Before going further, mark your own graph with one piece of honesty: **which arrows do you actually have data for?** M2 is relatively measurable from rep-entered fields — samples and co-pay cards are recorded. M3 is measurable from public Open Payments data, meals by NPI and date. M1 is the hardest: "educational attitude" lives in rep free-text call notes, in the objection resolved and the clinical question answered, not in a clean structured field. The legally important pathway is also the hardest to measure. That is not a coincidence you can wish away.

<!-- → [DIAGRAM: Three-pathway mediation graph — nodes: Z (cohort timing, instrument), D (message variant, treatment), M1 (educational attitude), M2 (samples/co-pay), M3 (sponsored meals/reciprocity), Y (NRx outcome); arrows: Z→D, D→M1, D→M2, M3→Y (with meals entering from Open Payments), all M's→Y; color coding: M1 green (legally drivable), M2 amber (regulated), M3 red (scrutinized); each arrow annotated with data source and measurement confidence] -->

---

To talk precisely about "how much goes through education" you need counterfactual notation, because the question is inherently about worlds you do not observe.

Write $Y(d, m)$ for the prescribing a physician *would* show if the message were set to $d$ and the mediator fixed at $m$, and $M(d)$ for the mediator value that message $d$ would induce. Then four quantities matter:

The **Total Effect** is $Y(1, M(1)) - Y(0, M(0))$ — the message switched on versus off, with the mediator free to respond naturally. This is what the Chapter 4 IV estimate targets.

The **Natural Direct Effect** is $Y(1, M(0)) - Y(0, M(0))$ — the effect of the message while holding the mediator at the value it would have taken *with no message*. The part of the effect that does not flow through M.

The **Natural Indirect Effect** is $Y(1, M(1)) - Y(1, M(0))$ — the effect of moving the mediator from its no-message value to its message-induced value, with the message on. The part that flows *through* M. These two add up cleanly: **TE = NDE + NIE**.

The **Controlled Direct Effect** is $Y(1, m) - Y(0, m)$ — the message effect when the mediator is *fixed by intervention* at some chosen value $m$. This answers the policy question "what would the message do if we blocked the sample channel and set samples to zero?" — but it does not generally sum to TE.

The NDE/NIE split is the natural way to phrase "how much went through education." The CDE is the natural way to phrase "what if we turned a pathway off." Hold both; they answer different questions.

---

Almost everyone arrives with one mediation method in hand, and it is the wrong one here. It is worth teaching it carefully before breaking it, because the break is where the real understanding lives.

The Baron–Kenny approach, from their 1986 paper in the *Journal of Personality and Social Psychology* (51(6):1173–1182), is the product-of-coefficients method: regress the outcome on the treatment, then add the mediator, and watch how much the treatment coefficient shrinks. The shrinkage is the indirect effect — the pathway. It is foundational in the social sciences, and it is wrong, structurally rather than incidentally, for this problem.

Baron–Kenny assumes no exposure–mediator interaction: that the message's effect on prescribing is the same regardless of how much the mediator moved. But interaction is exactly what you would expect — a sample matters more to a physician the message has already half-persuaded. It assumes linearity, but NRx is a count and the relationships are plausibly nonlinear. It conflates a statistical event with a causal one: a coefficient shrinking when you add M is a fact about two regressions, not proof that the effect *flows through* M. And it silently assumes no unmeasured mediator–outcome confounding — an assumption it never states, and that is wildly implausible here, since a physician's general openness simultaneously drives engagement with the message, willingness to take a sample, and willingness to accept a meal.

The counterfactual framework — built through Robins & Greenland (1992, *Epidemiology* 3(2):143–155), Pearl (2001, Mediation Formula, *Proc. UAI 17*:411–420), and VanderWeele (2015, *Explanation in Causal Inference*, Oxford UP; 2016, *Annual Review of Public Health* 37:17–32) — replaces "watch the coefficient shrink" with "define NDE/NIE/CDE as counterfactual contrasts and state the assumptions under which the data identify them." It does not make the problem easy. It makes the difficulty *visible*, which is the point.

---

Here is why mediation is harder than the total effect, and why that hardness is structural rather than a matter of more data.

Identifying even the NDE and NIE for a *single* mediator requires four no-unmeasured-confounding assumptions, all conditional on measured covariates (VanderWeele 2016): no unmeasured exposure–outcome confounding; no unmeasured mediator–outcome confounding; no unmeasured exposure–mediator confounding; and — this is the one that has no analogue in total-effect estimation — **no mediator–outcome confounder that is itself affected by the exposure.**

That fourth assumption is called the **cross-world assumption**, and it is the hinge of the chapter.

Why "cross-world"? Because the NDE/NIE contrasts compare quantities like $Y(1, M(0))$ — the outcome in a world where the message is *on* but the mediator sits at its *message-off* value. That combination never occurs in any real or randomized world. You are reasoning across two counterfactual worlds simultaneously, and the assumption required to do so is **untestable even in a perfect randomized trial.**

Robins & Greenland (1992) proved the consequence precisely: direct and indirect effects are *not separately identifiable* from randomization of the exposure alone. Randomizing the message — which is what the cohort-rollout instrument approximated — secures assumptions one and three, which is enough for the total effect. It does nothing for assumptions two and four, because the *mediator* is never randomized. The total effect leaned on randomization-like variation we tested and defended. The pathway split leans on additional, untestable, cross-world assumptions about mediators we never manipulated. These are different epistemic creatures, and conflating their credibility is the subtler cousin of the opening case's error.

State this to a partner in plain language before showing any decomposition result: **the total message effect is far more credible than any pathway share.** Not because the decomposition is useless — the ethics and the economics demand it — but because it sits on a different and weaker evidential foundation.

<!-- → [DIAGRAM: Assumption hierarchy for causal identification — three stacked layers; bottom layer labeled "Total Effect (IV estimate)": requires assumptions 1 and 3, secured by quasi-randomization; middle layer labeled "Single-Mediator NDE/NIE": requires all 4 assumptions including cross-world; top layer labeled "Three-Mediator per-pathway split": additionally requires no between-mediator confounding; each layer annotated with what quasi-randomization buys and what it does not] -->

---

Now add the difficulty of three correlated mediators, because that is the actual problem.

M1, M2, and M3 are not independent. A physician's general openness simultaneously drives engagement with the educational content, willingness to accept samples, and willingness to accept a meal. Worse, the mediators plausibly affect each other: a persuasive educational exchange may make a physician more receptive to a sample; a meal may open the door to a longer detailing conversation. M2 may sit on the path between the message and M1's effect on prescribing — making M2 a post-exposure confounder of M1's pathway, not just a parallel channel.

When one mediator is a post-exposure confounder of another, the natural three-way decomposition — "X percent through M1, Y percent through M2, Z percent through M3" — is **generally not point-identified.** The data are consistent with many different splits. No regression recovers it; running the mediators in a different order produces a different answer. That instability is not a numerical artifact to be smoothed away. It is the identification problem made visible.

This is the single most important honesty point in the chapter. A peer who reads "the education pathway accounts for 60% of lift" should ask: in what order did you enter the mediators? How much does that share change if you enter them differently? The answer, in this setting, is: substantially, because the share is not identified.

What *is* identifiable, under stated assumptions, is coarser and still useful: the **joint** indirect effect through the set {M1, M2, M3} combined, and the **direct** effect not through any of them. That split — educational-plus-relationship-plus-reciprocity versus direct — is honest and is often enough to drive the investment-and-ethics argument.

To go further than the joint split without the cross-world assumption, the right tool is **interventional indirect effects** — also called randomized-interventional indirect effects — from VanderWeele, Vansteelandt & Robins (2014, *Epidemiology* 25(2):300–306 `[verify pages]`), extended to multiple mediators with an exact additive decomposition by **Vansteelandt & Daniel (2017, *Epidemiology* 28(2):258–265)**. The key insight: instead of asking "what would prescribing be if we held M1 at its natural no-message value," you ask "what would prescribing be if M1 were drawn randomly from its message-induced distribution." That stochastic intervention avoids the cross-world assumption entirely, because you never condition on a combination of worlds that cannot occur.

The trade-off you accept: interventional effects answer a slightly different question than natural effects, and the 2014 single-mediator version does not sum exactly to TE. You buy weaker assumptions at the cost of a more contingent interpretation — the share attributed to M1 is now "what would change if we intervened to randomize M1 from its message-induced distribution," not "the share that flows naturally through M1." That is an honest trade, and naming it clearly is the methodological point of the chapter.

---

Here is what the analysis actually looks like on this dataset, including the wrong turn.

**Drawing the graph and marking the data.** For a specific message transition, lay out D (message variant, from `Call2_Key_Message_vod`) → M1 (educational attitude, proxied by rep-noted objection resolution and clinical questions in free-text notes); D → M2 (samples and co-pay cards left, from rep-entered fields); and M3 (Open Payments meals, by NPI and date) → Y (NRx, lagged 30/60/90 days). Annotate each arrow with its data source and measurement confidence. M1 has the weakest data and is the legally most important. Write that down as a limit, not a footnote.

**The dead end.** You want "the education pathway," so you run a Baron–Kenny mediation: regress NRx on the message, add the educational proxy, control for samples and meals, read the coefficient shrinkage as the education share. Stop and examine the assumption you just made. You assumed the three mediators do not confound each other — but a physician's openness drives all three simultaneously, and an educational exchange can change sample uptake. M2 and M3 are post-exposure confounders for M1's effect on Y. The "education share" you just reported is an artifact of the order in which the mediators were entered. A colleague entering them in a different order would report a different education share. **Neither is the true share, because it is not identified.** The lesson: the dead end is not a computational mistake. It is a structural one, and the only fix is a different estimand.

**The defensible move.** Estimate the joint indirect effect through {M1, M2, M3} and the direct effect not through any of them — both identifiable under stated no-confounding assumptions. To go further, apply the Vansteelandt & Daniel (2017) interventional indirect effects, which give an exact additive decomposition across the three mediators without the cross-world assumption. Report the result *as* an interventional quantity, not as a natural pathway share. Run a sensitivity analysis: how strong would an unmeasured mediator–outcome confounder need to be to overturn the education-versus-non-education conclusion?

**The limit.** Even the interventional decomposition is assumption-heavy, and the M1 proxy is weak. The honest claim is bounded: under stated assumptions, an estimated fraction of the message's effect is attributable to non-educational channels, with a sensitivity range showing the conclusion survives confounders up to a stated strength. That is enough to drive the investment-and-ethics argument. It tells the brand whether it is buying education or obligation without pretending to a precision the data cannot deliver.

<!-- → [TABLE: Mediation estimand comparison — rows: Baron-Kenny, Natural Direct/Indirect Effects, Controlled Direct Effect, Interventional Indirect Effects; columns: what it answers, key identifying assumptions, whether cross-world assumption required, whether three-correlated-mediator decomposition is exact, appropriate use case; each row with a one-line verdict on suitability for this problem] -->

---

The regulatory stakes of this decomposition are not incidental. The pathway map *is* the regulatory map.

M1 is green: a brand may openly try to inform prescribing with accurate clinical data. The educational pathway is what the brand is legally doing when it details. M2 is amber: regulated and context-dependent, sometimes legitimate, not education. M3 is red: the channel the Sunshine Act was designed to make visible and the one anti-kickback scrutiny attaches to. The Open Payments database exists because Congress believed the M3 pathway warranted public disclosure.

A brand that cannot tell whether its lift runs through M1 or M3 cannot tell whether it is doing medicine or buying obligation. The DeJong et al. result (*JAMA Internal Medicine* 2016, 176(8):1114–1122) — that a single sponsored meal under $20 was *associated with* higher promoted-brand prescribing across 279,669 physicians, with the rate rising with more and larger meals — is association evidence, not causal proof. The authors say so explicitly. A physician's general prescribing propensity may drive both meal acceptance and branded prescribing; reverse causation is plausible (reps target high-volume prescribers). Treat M3 as real-association, contested-causation, and carry that caveat into any partner presentation.

The patient-welfare dimension follows from the regulatory one. Education-driven prescribing is defensible to the extent the education is accurate and the drug is clinically warranted. Reciprocity-driven prescribing moves what patients receive without a clinical reason — the physician prescribes more not because it helps the patient but because the rep bought lunch. That is the welfare harm the regulation exists to prevent. Estimating M3 honestly is therefore a patient-welfare act, not only a compliance exercise.

One practical note for the annotated graph: the public/IP firewall applies here as always. Meals are public Open Payments data and live on the Fellow's side of the wall. The M1 free-text proxy and M2 fields are built and tested on synthetic data; the partner replicates on real CRM data internally.

---

**Five-Part AI Exercise Block**

**When to use AI here.** Use an LLM to render and label the DAG, to recall the precise definitions and identifying assumptions of NDE/NIE/CDE and interventional effects, and to scaffold mediation code — the `mediation` package family, or a Vansteelandt–Daniel interventional-effects implementation. It is also useful for drafting sensitivity-analysis setups once you have stated your estimand.

**When NOT to use AI here.** Do not let an LLM tell you that your three-pathway split is identified, or hand you a Baron–Kenny education share as if it were a causal quantity — both are the exact errors this chapter exists to prevent, and a model trained on a literature full of naive mediation will reproduce them confidently. Do not outsource the regulatory classification of a pathway; M1-versus-M3 status is a legal and domain judgment routed to counsel, not a model output.

**LLM exercise (copy-paste prompt):**
> "I am decomposing the effect of a sales message on prescribing into three correlated, mutually-causal mediators: educational attitude (M1), samples and co-pay cards (M2), and sponsored meals (M3). A colleague wants to use Baron–Kenny product-of-coefficients to report the share through education. Do three things: (1) explain structurally why the natural per-pathway split is generally not point-identified when mediators are correlated and affect each other; (2) name the cross-world assumption and explain why randomizing the message does not rescue the decomposition; (3) describe the interventional indirect effects approach (Vansteelandt & Daniel 2017) as the defensible alternative, stating exactly what interpretation I trade away. Do not produce a numeric education share."

**CLI exercise.** In Claude Code, write a script that estimates the education share two ways on the synthetic data — naive Baron–Kenny with the mediators entered in two different orders, and a joint-indirect-versus-direct decomposition — and prints all results side by side with the known ground-truth pathway split. The purpose is to demonstrate non-identification: the two Baron–Kenny orderings should disagree, and both should miss ground truth, while the joint/direct split should be honest about what it does and does not pin down. If the AI-generated pipeline reports a stable, confident education share across orderings, treat that stability as a bug.

**AI validation.** Validate any decomposition code against the synthetic dataset's known pathway structure. The correct behavior is *not* that the method recovers per-pathway shares — it should not, because they are not identified — but that the joint indirect effect is recovered and the naive per-pathway numbers visibly fail to match ground truth and disagree across orderings. This is the test of whether your implementation is honest about what the data can and cannot say.

**AI Use Disclosure**

*Write two sentences naming what an AI tool did in your work for this chapter and the one judgment it could not make. For example: "I used an LLM to scaffold the interventional-effects code and to draft the DAG; I determined myself that the natural three-pathway split is not point-identified in my specific dataset, because the model reproduced the Baron–Kenny calculation confidently without flagging the between-mediator confounding that makes it invalid here."*

---

**What Would Change My Mind**

I would soften the non-identification verdict if the three mediators turned out to be, in a specific dataset, effectively uncorrelated and non-interacting — if a physician's openness did *not* jointly drive education, samples, and meals, and the mediators did not affect each other — because then the per-pathway split would be much closer to identified and a careful natural-effects analysis would be defensible. I would harden the caution if sensitivity analyses on real data routinely showed the education-versus-reciprocity conclusion flipping under modest unmeasured confounding, which would mean even the joint/interventional claims are too fragile for partner use. And I would revise the M3 framing from "contested causation" toward "established" if a credible quasi-experimental study — not an association — established a clean causal meal effect with a defensible counterfactual.

**Still Puzzling**

- The M1 measurement problem is genuinely unresolved. "The physician learned something" is the legally central quantity and the one with the worst data — a free-text proxy that conflates a thoughtful clinical exchange with a rep's optimistic note. Whether the Chapter 8 elicitation can produce an M1 signal good enough to support an interventional decomposition is an open empirical question, not a settled method.
- How do you communicate an *interventional* pathway estimate — what would change if M1 were drawn randomly from its message-induced distribution — to a brand team that wants "the percent through education"? The honest quantity is not the quantity they asked for, and the translation problem is real and unsolved.
- The DeJong et al. association between meals and prescribing is strong and dose-responsive, which strengthens the causal interpretation. But the plausible confounders — physician openness, rep targeting of high-volume prescribers — are also strong. How much of the M3 association would survive a credible instrumental-variable study? No such study exists yet.

---

## Exercises

**Warm-up**

1. *(Factual recall — the estimand vocabulary)* Define NDE, NIE, and CDE in one sentence each in plain language, without notation. Then state which one answers the question "what would happen to the message's effect if we banned sponsored meals for this brand and set them to zero?"
   *What this tests: whether you hold the three counterfactual contrasts distinct and can map them to policy questions.*

2. *(Factual recall — why Baron–Kenny fails)* Name three structural reasons Baron–Kenny coefficient-shrinkage is inappropriate for this three-mediator problem. For each, write one sentence on what it assumes and why that assumption is violated here.
   *What this tests: the specific failure modes of the standard method, not generic criticism.*

3. *(Factual recall — the cross-world assumption)* State the cross-world assumption in plain language. Explain in two sentences why randomizing the message — which the cohort-rollout instrument approximated — secures the total effect but does nothing for this assumption.
   *What this tests: the logical gap between securing the total effect and securing the pathway split.*

**Application**

4. *(Apply — the opening case revisited)* The analytics team reports equal NRx lift from the educational deck and the sponsored lunches. After a decomposition shows the lunch lift runs through M3 and the deck's lift through M1, write the recommendation the brand should make — and contrast it, precisely, with the recommendation the total effect alone produced. Name the ethical and regulatory difference between the two actions.
   *What this tests: translating a decomposition result into a decision, with the regulatory stakes named.*

5. *(Apply — graph annotation)* Draw the three-pathway mediation graph for a specific message transition in your thread's dataset. Annotate each arrow with its data source, measurement confidence, and regulatory color (green/amber/red). Mark which arrows you have data for and which you are proxying or cannot measure. Flag the M1 arrow explicitly with the measurement-confidence problem.
   *What this tests: building the graph as a concrete data-documentation exercise, not as an abstract diagram.*

6. *(Apply — non-identification demonstration)* On the synthetic dataset, run a Baron–Kenny mediation for the education share with the mediators entered in one order. Then re-run with the mediators in a different order. Report both education shares and compare each to the known ground-truth pathway split. Write one paragraph explaining why the two estimates disagree and what the disagreement demonstrates about identification.
   *What this tests: demonstrating non-identification empirically rather than accepting it as a theoretical claim.*

**Synthesis**

7. *(Synthesize — the defensible estimand)* For your thread's dataset, write the honest estimand statement: the counterfactual quantity you will actually report (joint indirect effect, direct effect, and if attempted the interventional decomposition), the identifying assumptions each requires, the cross-world/interventional trade-off you are accepting, and the M1 caveat. This is the estimand statement required in the named Fellow artifact.
   *What this tests: writing a pre-registered-style commitment to what you will claim and what you will not — the core scientific discipline of the chapter.*

8. *(Synthesize — sensitivity analysis)* Sketch the sensitivity analysis for your decomposition: what unmeasured mediator–outcome confounder would have to be present, and how strong would it have to be, to overturn the education-versus-non-education conclusion? Name one plausible real-world confounder (e.g., physician openness) and estimate, qualitatively, whether it is above or below the sensitivity threshold.
   *What this tests: converting the identification concern into a quantified robustness check rather than a blanket disclaimer.*

**Challenge**

9. *(Open-ended — the M1 measurement problem)* The educational pathway is the legally central quantity and the hardest to measure. The current proxy is rep free-text notes — the objection resolved, the clinical question answered. Propose a measurement strategy that would produce a stronger M1 signal: what data would you need, how would you extract it, what would validate it, and what assumption about the relationship between the proxy and the true educational attitude does your strategy require? Name the way this strategy could fail and the evidence that would reveal the failure.
   *What this tests: attacking the chapter's standing unresolved problem rather than accepting it as a fixed limitation.*
