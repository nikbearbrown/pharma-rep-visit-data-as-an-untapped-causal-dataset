# Chapter 4 — The Natural Experiment in Every Training Rollout
*The experiment has been running for fifteen years. Nobody noticed because nobody was looking for it.*

A Fellows team is handed a brand's message transition: in March the field began moving from an efficacy-first deck to a safety-first deck, and by June most reps had switched. The team wants the effect of the safety-first message on new prescriptions. They reason cleanly, or so they think. Physicians whose reps were early in the rollout got the safety message sooner; physicians whose reps switched later got it later. Compare 60-day prescribing between early-exposed and late-exposed physicians, control for specialty and territory, and read off the effect.

The estimate comes back large, positive, and tight. The safety-first message, they conclude, lifts new prescriptions by a meaningful margin. The brand is delighted. A senior reviewer asks one question: *who got trained first?*

It turns out the brand's best reps were trained first. Star performers go to the front of the rollout queue — they are trusted with the new message, they are managed more closely, and they were already assigned the higher-potential territories. So "trained early" is a proxy for "called on by a great rep in a great territory." The early-exposed physicians were always going to prescribe more, message or no message. The team did not measure the effect of the safety-first message. They measured the effect of being covered by a star rep, and pinned the label "message effect" on it.

Two things went wrong, and they are the two failure modes this chapter exists to prevent. **Independence failed**: the timing of exposure was correlated with the physicians' baseline prescribing, because rep quality drove both. **Exclusion failed**: even setting baseline aside, early-trained reps sell better in general, so cohort timing affected prescribing through a channel other than the message. The team would have caught the first failure with a single regression — cohort timing on pre-rollout prescribing — run *before* any effect estimate. They never ran it.

That regression is the midterm.

---

## What a natural experiment is, and why this is one

A randomized controlled trial assigns treatment by a coin flip the experimenter controls. The coin flip guarantees that treated and untreated units are identical in everything except treatment, so any difference in outcomes is caused by treatment. A **natural experiment** borrows the same logic from the world: some process outside the analyst's control and unrelated to the outcome assigns units to treatment-like conditions as if by a coin flip.

The canonical examples are worth holding in mind because the pharma case is structurally identical to both.

Angrist and Krueger (1991, *Quarterly Journal of Economics*) used **quarter of birth** as a lever on years of schooling. Compulsory-schooling laws tie school entry to the calendar, so children born in different quarters complete slightly different amounts of schooling for reasons that have nothing to do with their ability. The quarter of birth is unrelated to the outcome — earnings — except through its effect on schooling. Angrist (1990, *American Economic Review*) used the **Vietnam draft lottery**, where a literal random number determined who served, as a lever on veteran status to estimate the effect of military service on earnings. [verify exact pages for both citations] In each case the lever is something administrative and arbitrary — a birthday, a lottery number — that nudges people into treatment without itself caring about the outcome.

In rep-visit data the lever is **training-cohort timing**. A new clinical message rolls out in waves. Reps in the month-1 cohort begin delivering the safety-first deck; reps in the month-2 cohort are still delivering the old efficacy-first deck during that same window. A physician's exposure to the new message is therefore determined, in part, by when her rep happened to be scheduled for training — a scheduling decision driven by regional logistics, hiring calendars, and trainer availability. The physician did not choose her message. She got it because of administrative decisions upstream of her rep. That is the as-if-random nudge.

Whether the nudge is *actually* as-good-as-random — rather than correlated with physician prescribing propensity through a mechanism like star-rep assignment — is not an assumption to state and move past. It is a **testable claim**, and you must test it before reporting any estimate. The test is the opening case's missing step.

<!-- → [DIAGRAM: Two parallel timelines — "Training cohort A" and "Training cohort B" — with cohort A switching to the new deck in month 1 and cohort B in month 3. Arrows from each cohort to a sample physician, showing different message exposure for otherwise comparable physicians in the same month. Caption: "The natural experiment: administrative scheduling determined which physician heard which message when. That variation is the instrument."] -->

---

## The instrumental-variables framework, mapped to this dataset

An **instrumental variable** is a variable Z that affects the treatment D but affects the outcome Y *only through* D, and that is unrelated to whatever else confounds the D-to-Y relationship. If you have such a Z, you can recover the causal effect of D on Y even when D and Y share unmeasured confounders — which they badly do here. A physician's openness to a drug drives both how her rep details her and how she prescribes; those are tangled in any simple comparison.

The mapping for this design:

**Instrument Z** = training-cohort assignment — the rollout timing from HR and training records, specifically which cohort the rep was scheduled in and therefore when she could begin delivering the new deck.

**Treatment D** = message-variant exposure — which deck the physician's rep was actually delivering, recorded in the rep-entered key-message field in the CLM system.

**Outcome Y** = NPI-level new prescriptions at 30, 60, and 90 days after exposure, from the prescribing-data warehouse.

The estimation has two visible regressions. The **first stage** regresses treatment on the instrument: does cohort timing actually predict which deck got delivered? The **reduced form** regresses the outcome on the instrument: does cohort timing predict prescribing? The IV estimate is, roughly, the reduced form divided by the first stage — the part of the prescribing difference that travels through the message, scaled by how strongly the instrument moved the message.

A critical honesty point about *what* IV estimates. When treatment effects are heterogeneous — and they certainly are, since some physicians are persuadable and many are not — IV does not recover the average effect over everyone. It recovers the **Local Average Treatment Effect (LATE)**: the average effect among **compliers**, the physicians whose message exposure actually tracked their rep's cohort timing (Imbens & Angrist 1994, *Econometrica* 62(2):467–475; Angrist, Imbens & Rubin 1996, *JASA* 91(434):444–455). Physicians whose reps would have delivered the new deck regardless of training, or never, are not in the estimate. The LATE is real and useful, but it is a statement about a subpopulation you do not get to name in advance. Report it as such.

<!-- → [DIAGRAM: Three-node DAG — training cohort (Z) → message variant (D) → NRx (Y). A dashed bidirectional arrow between an unlabeled "unobserved confounder" node and both D and Y. The Z→Y path is visibly absent except through D. Caption: "The IV structure: Z shifts D; the confounders corrupt the direct D→Y comparison; Z's only path to Y is through D."] -->

---

## The three conditions, and a fourth

An instrument earns its keep only if it satisfies three conditions. Each one is falsifiable on this dataset, and none should be assumed.

**Relevance.** Z must actually shift D: the instrument must move the treatment. If cohort timing barely changed which deck got delivered — because reps switched on their own schedule regardless of training, or because old slides lingered in the field — the instrument is *weak* and everything downstream is unreliable. The check is the **first-stage F-statistic** on the instrument. We are about to argue that the threshold you probably have in your head is wrong.

**Independence.** Z must be as-good-as-randomly assigned with respect to the physicians' potential outcomes — cohort timing must be uncorrelated with how much these physicians would prescribe anyway. This is exactly what failed in the opening case. It is **testable**, and you are required to test it: regress cohort assignment on pre-rollout prescribing. If early-trained reps systematically cover higher-baseline physicians, the coefficient is significant and the design is dead. You do not patch this; you stop and report the failure.

**Exclusion.** Z must affect Y *only through* D — cohort timing must change prescribing only by changing the message, with no direct side channel. The obvious threat: early training may also improve a rep's general selling skill, or grant more selling days, or more manager attention, or earlier sample supply within the measurement window. Exclusion is **not directly testable** — it is a statement about a channel you cannot observe — but you defend it by controlling rep tenure and baseline performance, so that "early-trained" is not standing in for "more experienced and better at selling," and by checking that the event-study leads are flat before the rollout.

A **fourth** assumption rides along with the LATE interpretation: **monotonicity** — no defiers. A defier would be a rep who, trained later, reverts to the old message, or one who, trained earlier, refuses the new one. Reps reverting to a familiar old deck is plausible enough to name explicitly. State monotonicity as an assumption, not a given.

---

## The F > 10 rule is wrong — here is what replaces it

Here is a misconception you very likely arrived with: *first-stage F > 10 means the instrument is strong enough to trust the inference.*

This is wrong, and now formally so.

The F > 10 threshold traces to Staiger and Stock (1997, *Econometrica* 65(3):557–586) and was sharpened by Stock and Yogo (2005, in Andrews and Stock, eds., *Identification and Inference for Econometric Models*, Cambridge UP) as a **relative-bias criterion**: F > 10 roughly guarantees that the bias of the 2SLS estimator is under about 10 percent of the bias of OLS. Notice what that controls — **bias**, the location of the estimate. It says nothing about whether the **t-test you actually report** has correct size.

Lee, McCrary, Moreira, and Porter (2022, *American Economic Review* 112(10):3260–3290) worked out what is actually required for the conventional 5 percent t-test (|t| > 1.96) to have correct size in the just-identified single-instrument case — which is exactly the case here, one cohort instrument. The answer is a first-stage F of roughly **104.7**, not 10. Their **tF procedure** fixes this by inflating the standard error as a smooth function of the observed F: weaker first stages get larger standard errors automatically. When they re-examined 61 published *AER* papers, about a quarter of specifications needed standard errors at least 49 percent larger to be valid at the 5 percent level. A quarter of peer-reviewed instrumental-variables results in a top journal were overstating their precision.

The threshold that controls one quantity — bias — does not license a claim about a different quantity — inferential precision. "Everyone uses F > 10" is not a defense.

The teaching move for this dataset: report the first-stage F, but do not stop there. With a single cohort instrument, apply the tF standard-error adjustment, and additionally report **Anderson–Rubin** weak-instrument-robust confidence sets, which stay valid however weak the instrument is. Use the **effective F** of Montiel Olea and Pflueger (2013) [verify cite] because NRx data is clustered by rep and territory and the classical F assumes it is not. The review by Andrews, Stock, and Sun (2019, *Annual Review of Economics* 11:727–753) is the practitioner's summary and recommends exactly this combination.

<!-- → [CHART: Relationship between first-stage F-statistic and the effective size of a nominal 5% t-test — x-axis: F from 1 to 200; y-axis: actual test size. Horizontal line at 5%. The curve descends to 5% only around F ≈ 104.7. Vertical line at F = 10 showing the actual test size there is well above 5%. Caption: "F > 10 controls bias, not size. The tF correction is required for valid inference at conventional F values."] -->

---

## The staggered-rollout trap

Suppose independence and exclusion hold and you want to estimate the effect directly, treating "delivered the new deck" as a treatment that switches on at different times for different physicians. The natural move is a **two-way fixed effects (TWFE)** regression: outcome on a post-treatment dummy plus physician and time fixed effects. This is the workhorse difference-in-differences specification, and with staggered timing it is a trap.

Goodman-Bacon (2021, *Journal of Econometrics* 225(2):254–277) showed that staggered TWFE is a weighted average of all possible 2×2 difference-in-differences comparisons — and some of those comparisons are **forbidden**: they use already-treated early cohorts as the "control" group for later-treated cohorts. When treatment effects change over time, those forbidden comparisons enter with **negative weights**, so the TWFE coefficient can be a badly distorted, even sign-flipped, summary of the true effects. de Chaisemartin and D'Haultfœuille (2020, *AER* 110(9):2964–2996) independently demonstrated the negative-weighting problem. You can have every treatment effect positive and produce a negative TWFE coefficient.

The fix is a **heterogeneity-robust estimator** that never uses already-treated units as controls. Callaway and Sant'Anna (2021, *Journal of Econometrics* 225(2):200–230) estimate group-time average treatment effects using **not-yet-treated** cohorts as clean controls; Sun and Abraham (2021, *Journal of Econometrics* 225(2):175–199) give an interaction-weighted estimator that also exposes spurious pre-trends. Baker, Callaway, Cunningham, Goodman-Bacon, and Sant'Anna (2025, arXiv:2503.13323) is the current practitioner synthesis. The discipline: use a robust estimator with not-yet-trained cohorts as controls, then build the cohort instrument on top.

---

## Running the design, with the dead ends

Walk the design the way you should walk it, in order.

**Build the panel.** Link three layers. From the CLM rep-entered records, take the key-message field to identify which deck each physician's rep was delivering and when — treatment D. From HR and training records, take each rep's cohort timing — instrument Z. From the prescribing warehouse, take NPI-level new prescriptions at 30, 60, and 90 days — outcome Y — plus the physician's pre-rollout prescribing history. Add territory fixed effects, rep tenure, and a baseline rep-performance measure. The unit of analysis is physician by time.

**Run the falsification test first.** Before estimating anything, regress cohort assignment on pre-rollout prescribing, controlling for specialty and territory. This is the test the opening-case team skipped. If it comes back significant — early-trained reps systematically cover physicians whose pre-rollout prescribing was already higher — independence has failed. The honest action is to stop and report the failure. The design does not identify the message effect on this brand's rollout, and the deliverable is a memo saying so. This is not a setback to hide. It is the deliverable.

**Walk through the TWFE dead end.** Suppose falsification passes and you reach for the quick estimate: a TWFE regression of prescribing on a post-training dummy. Run the Goodman-Bacon decomposition on it before reporting the coefficient. You will likely find weight sitting on forbidden comparisons — late cohorts judged against already-trained early cohorts. Swap in Callaway–Sant'Anna using not-yet-trained cohorts as clean controls.

**Estimate, with honest inference.** Run the first stage and confirm relevance. Report the effective F and, if it is anywhere near the zone where the legacy rule fails, the tF-adjusted standard error and the Anderson–Rubin confidence set. Then read off the IV estimate of the message effect, controlling rep tenure and baseline performance to defend exclusion, and check that the event-study leads are flat before the rollout begins.

**Name the limit.** A clean estimate gives you a **population LATE for compliers** — the average effect of the safety-first message among physicians whose exposure actually tracked cohort timing. It does not tell the brand which message to deliver to a specific physician next week. That individual counterfactual question is a Rung-3 question and is handed to Chapter 9. And the LATE is a total effect — it lumps together every channel by which the message moved prescribing. Chapter 5 pulls that total apart into pathways, because the channels carry different ethical and regulatory weight.

---

## The named artifact: the IV plus falsification design

Produce a one-page identification design for a specific message transition in your thread's data. In order:

1. **The three-node DAG**, showing the instrument visibly upstream of treatment and the unmeasured confounder's paths to both treatment and outcome.

2. **The IV mapping**: one line each naming Z, D, and Y with the exact field or record each comes from.

3. **The three conditions with their checks**: relevance with the first-stage F plan and tF correction; independence with the falsification regression written out explicitly; exclusion with the control set and the flat-leads requirement. One additional line stating the monotonicity assumption.

4. **The kill criterion**: the explicit rule — if the falsification coefficient is significant at the 5 percent level, the design is abandoned and reported as failed.

The grading emphasis is order of operations. A design that shows an effect estimate before establishing that the falsification test passes is marked incomplete, by construction.

---

## What would change my mind

The chapter's central claim — that cohort timing is a usable instrument — is conditional, not universal. It holds only if independence survives the falsification test on a particular brand's rollout. If, run across multiple real rollouts, the test failed routinely because high-potential territories are systematically front-loaded, the instrument would be invalid in practice even though it is sound in principle. The chapter would have to retreat to "valid only in the rare rollout where logistics genuinely dominate scheduling."

A second revision would follow if early training so reliably improves a rep's general selling skill that the exclusion restriction cannot be defended by tenure and baseline controls. That would move cohort timing from "conditional instrument" to "not an instrument."

## Still puzzling

How large is the complier subpopulation in practice? If most reps switch decks on their own schedule regardless of training, the complier group could be small and unrepresentative, and the LATE would answer a question about a sliver of the market. There is no fully satisfying way to characterize compliers from the data alone.

It is also unresolved how to combine the cohort instrument with the staggered-DiD machinery in a formally clean way. The chapter teaches both and says "build the instrument on top of the robust estimator," but the joint estimand is a live methodological question.

---

**LLM exercise (copy-paste prompt):**

> "I have a staggered rollout of a new sales message across training cohorts, and I want to use cohort timing as an instrument for message exposure to estimate the effect on prescribing. Here is my setup: [paste your Z/D/Y mapping]. Do three things: (1) list every distinct way the independence assumption could fail for a training-cohort instrument, and for each, name the falsification check that would detect it; (2) list the threats to the exclusion restriction specific to sales-rep training and the control variables that would defend against each; (3) tell me explicitly which of these checks the data can test and which are assumptions I must argue. Do not estimate anything or tell me whether my instrument is valid — only enumerate the threats and checks."

**CLI exercise.** In your repo, ask Claude Code to scaffold a reproducible falsification-first pipeline: a script that (a) loads the synthetic panel, (b) runs and prints the falsification regression with its p-value and a hard `assert` or exit that stops the pipeline if significant at 5 percent, and (c) only then proceeds to the first stage and the Callaway–Sant'Anna estimate. The point is to encode the order-of-operations discipline in code so the estimate cannot run until the test passes.

**AI Validation exercise.** Whatever the AI scaffolds, validate it against the known ground truth in the synthetic dataset. The planted message effect is known, so check that your pipeline recovers it within its confidence set on the clean data and fails loudly on the version with the planted independence violation. If the AI-generated code produces a confident estimate on the violated version, the code is wrong, not the data.

**AI Use Disclosure**

*Write two sentences naming exactly what an AI tool did in your work for this chapter and what judgment you supplied that the AI could not. The judgment most specific to this chapter: whether the independence assumption survives on your particular brand's rollout — a domain judgment about who got trained first and why that depends on facts about this rollout's history that the model does not have and cannot verify.*

---

## Exercises

**Warm-up**

1. *(Recall — tests the natural-experiment structure)* For the draft-lottery and quarter-of-birth natural experiments, name Z, D, and Y in each, and state in one sentence why the instrument plausibly satisfies exclusion. Then do the same for the cohort-rollout design. *What this tests: whether you can read the IV structure from a narrative description rather than a formula.*

2. *(Recall — tests the F-statistic correction)* A colleague reports a first-stage F of 22 and declares the instrument is strong. Using Lee et al. (2022), explain what is wrong with that conclusion, and state what you would compute instead before believing any inference based on that F. *What this tests: whether you understand that the F > 10 rule controls bias, not inferential size, and can name the correction.*

3. *(Recall — tests the LATE)* Explain, in plain language, why IV does not recover the average treatment effect when treatment effects are heterogeneous, and name the subpopulation whose effect it does recover. Then describe one way that subpopulation might differ from the full physician population in a rep-visit setting. *What this tests: whether you can state the LATE definition and its practical limitation without jargon.*

**Application**

4. *(Apply — tests the falsification design)* Write the falsification regression for a training-cohort rollout explicitly: state the dependent variable, the key independent variable, the covariates to include, and the rule that determines whether the design proceeds or is abandoned. Then explain what a significant positive coefficient on the key independent variable would mean about the opening-case failure mode. *What this tests: whether you can specify the test before seeing any results.*

5. *(Apply — runs the pipeline)* On the synthetic rep-visit dataset, run the falsification regression twice: once on a version with a planted independence violation, once on a clean version. Report both coefficients, both p-values, and for each state whether you would proceed to estimation. *What this tests: whether you can run the test and apply the kill criterion without softening a failure.*

6. *(Apply — TWFE trap)* A colleague estimates a staggered TWFE regression of prescribing on a post-training dummy and finds a negative coefficient. They conclude the new deck hurts prescribing. Explain, in language a non-econometrician could follow, how a negative coefficient is consistent with every individual group-time treatment effect being positive. Name the fix. *What this tests: whether you understand the negative-weighting problem intuitively, not just technically.*

**Synthesis**

7. *(Synthesize — IV + staggered DiD)* Explain why, even after switching from TWFE to Callaway–Sant'Anna, you still need the cohort instrument rather than treating deck-delivered as the treatment directly. What unmeasured confounder does the instrument address that the robust DiD estimator alone cannot? *What this tests: whether you understand the distinct roles of the identification strategy (DiD) and the instrument (IV) in the combined design.*

8. *(Synthesize — all three conditions)* Return to the opening case and map both failures to the three IV conditions: which condition did "star reps trained first" violate, and which did "early training improves general selling skill" violate? For each failure, state whether it is testable or must be argued, and what evidence or control would address it. *What this tests: whether you can diagnose a failed design precisely rather than generically.*

**Challenge**

9. *(Challenge — produce the named artifact)* Build the full identification design of §8 for a message transition in your thread's data: the three-node DAG, the IV mapping with exact field names, all three conditions with their checks and the tF correction plan, the monotonicity statement, and the explicit pre-registered kill criterion. Submit it before running any estimate. *What this tests: whether you can execute the order-of-operations discipline — design before estimation — as a complete artifact rather than as a checklist.*
