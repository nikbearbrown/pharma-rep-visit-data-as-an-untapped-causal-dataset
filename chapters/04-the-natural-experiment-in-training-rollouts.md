# Chapter 4: The Natural Experiment in Every Training Rollout

## 1. Overview

By the end of Chapter 3 you could classify any pharma analytic practice by its rung on Pearl's ladder, and you could see that every deployed Next-Best-Action engine answers a Rung-1 question (what correlates with engagement) when the budget decision is a Rung-2 question (what message *causes* prescribing). That left an obvious objection: if the do-question is so important, why doesn't anyone just run a randomized trial? The answer is that randomizing clinical messages to physicians is slow, expensive, and ethically fraught — and, more importantly, that you do not need to. Pharma has been running a usable experiment for fifteen years without noticing.

This chapter teaches you to find that experiment and to estimate from it. The experiment is the **staggered rollout of new sales decks through training cohorts**. When a brand changes its clinical message, reps do not all switch on the same day; they switch when their training cohort is scheduled, and that schedule is set by hiring calendars and regional logistics, not by the prescribing habits of the physicians they call on. That timing is a source of quasi-random variation in which message a physician received — and quasi-random variation is exactly what an **instrumental variable (IV)** is.

You will learn the IV framework on this dataset: the three conditions an instrument must satisfy, the falsification test that must pass *before* you believe any estimate, the modern weak-instrument correction (the F > 10 rule you may have heard is wrong, and we will fix it precisely), and the staggered-difference-in-differences trap that sinks the naive version of this analysis. The deliverable, and the course midterm, is a falsification design: the regression you run to try to *kill* your own instrument before you trust it.

What you will be able to do: take a message transition in rep-visit data, write down the instrument, state and test all three IV conditions, and report a defensible average message effect with its assumptions on the table — or, just as valuably, conclude that the design fails and say why.

## 2. Learning objectives

By the end of this chapter you will be able to:

- **(Apply)** Set up the instrumental-variables design for a message transition — name the treatment, instrument, and outcome from the rep-visit panel, and write the first-stage and reduced-form regressions.
- **(Analyze)** Test the three IV conditions — relevance (first-stage strength), independence (cohort timing as-good-as-random), and exclusion (cohort affects prescribing only through the message) — and identify which one each plausible threat attacks.
- **(Evaluate)** Judge whether a reported first-stage F actually licenses the inference being drawn, using the modern tF correction rather than the legacy F > 10 rule of thumb.
- **(Create)** Design the falsification test — the regression of cohort timing on pre-rollout prescribing — that must pass before any effect estimate is believed.

## 3. Opening case (failure-first)

A Fellows team is handed a brand's message transition: in March the field began moving from an efficacy-first deck to a safety-first deck, and by June most reps had switched. The team wants the effect of the safety-first message on new prescriptions (NRx). They reason cleanly, or so they think: physicians whose reps were *early* in the rollout got the safety message sooner; physicians whose reps switched *later* got it later. Compare 60-day NRx between early-exposed and late-exposed physicians, control for specialty and territory, and read off the effect.

The estimate comes back large, positive, and tight. The safety-first message, they conclude, lifts NRx by a meaningful margin. The brand is delighted. A senior reviewer asks one question: *who got trained first?*

It turns out the brand's best reps were trained first. Star performers go to the front of the rollout queue — they are trusted with the new message, they are managed more closely, and they were already assigned the higher-potential territories. So "trained early" is a proxy for "called on by a great rep in a great territory." The early-exposed physicians were always going to prescribe more, message or no message. The team did not measure the effect of the safety-first message. They measured the effect of *being covered by a star rep*, and pinned the label "message effect" on it.

Two things went wrong, and they are the two failure modes this chapter exists to prevent. First, **independence failed**: the timing of exposure was correlated with the physicians' baseline prescribing, because rep quality drove both. Second, **exclusion failed**: even setting baseline aside, early-trained reps sell better in general, so cohort timing affected NRx through a channel *other than* the message. The team would have caught the first failure with a single regression — cohort timing on pre-rollout prescribing — run *before* any effect estimate. They never ran it. That regression is the midterm.

## 4. Core sections

### 4.1 What a natural experiment is, and why this is one

A **randomized controlled trial (RCT)** assigns treatment by a coin flip the experimenter controls. The coin flip guarantees that, on average, treated and untreated units are identical in everything except treatment, so any difference in outcomes is caused by treatment. A **natural experiment** is the same logic borrowed from the world: some process *outside the analyst's control and unrelated to the outcome* assigns units to treatment-like conditions as if by a coin flip. The economist's "credibility revolution" is, in large part, the discovery that such as-if-random assignments are scattered through observational data, waiting to be exploited (Angrist & Pischke 2010, *Journal of Economic Perspectives* 24(2):3–30).

The canonical examples are worth holding in mind because the pharma case is structurally identical. Angrist & Krueger (1991, *Quarterly Journal of Economics*) used **quarter of birth** as a lever on years of schooling: compulsory-schooling laws tie school entry to the calendar, so children born in different quarters complete slightly different amounts of schooling for reasons that have nothing to do with their ability. Angrist (1990, *American Economic Review*) used the **Vietnam draft lottery**, where a literal random number determined who served, as a lever on veteran status to estimate the effect of military service on earnings. (Exact pages for both [verify] before print.) In each case the lever is something administrative and arbitrary — a birthday, a lottery number — that nudges people into or out of treatment without itself caring about the outcome.

In rep-visit data the lever is **training-cohort timing**. A new clinical message rolls out in waves. Reps in the month-1 cohort begin delivering the safety-first deck; comparable reps in the month-2 cohort are still delivering the old efficacy-first deck during that same window. A physician's exposure to the new message is therefore determined, in part, by *when her rep happened to be scheduled for training* — a scheduling decision driven by region, hiring calendar, and trainer availability. The physician did not choose her message. She got it because of administrative logistics upstream of her rep. That is the as-if-random nudge.

### 4.2 The instrumental-variables framework, on this dataset

An **instrumental variable** is a variable Z that affects the treatment D but affects the outcome Y *only through* D, and that is unrelated to whatever else confounds the D→Y relationship. If you have such a Z, you can recover the causal effect of D on Y even when D and Y share unmeasured confounders — which they badly do here (a physician's openness to a drug drives both how her rep details her and how she prescribes).

The mapping for this chapter's design:

- **Treatment** D = message-variant exposure — which deck the physician's rep was actually delivering (new safety-first vs old efficacy-first), recorded in the rep-entered field `Call2_Key_Message_vod__c`.
- **Instrument** Z = training-cohort assignment — the rollout timing, taken from HR / training records (which cohort the rep trained in, hence when she could begin delivering the new deck).
- **Outcome** Y = NPI-level NRx at 30 / 60 / 90 days after exposure, from the warehouse layer (IQVIA Xponent or Symphony).

The estimation has two visible regressions. The **first stage** regresses treatment on the instrument: does cohort timing actually predict which deck got delivered? The **reduced form** regresses the outcome on the instrument: does cohort timing predict NRx? The IV estimate is, loosely, the reduced form divided by the first stage — the part of the NRx difference that travels through the message, scaled up by how strongly the instrument moved the message.

A critical honesty point about *what* IV estimates. When treatment effects are heterogeneous — and they certainly are; some physicians are persuadable and many are not — IV does not recover the average effect over everyone. It recovers the **Local Average Treatment Effect (LATE)**: the average effect among **compliers**, the physicians whose message exposure actually tracked their rep's cohort timing (Imbens & Angrist 1994, *Econometrica* 62(2):467–475; Angrist, Imbens & Rubin 1996, *JASA* 91(434):444–455). Physicians whose reps would have delivered the new deck regardless, or never, are not in the estimate. The LATE is real and useful, but it is a statement about a subpopulation you do not get to name in advance, and you should report it as such rather than as "the message effect."

### 4.3 The three conditions (and a fourth)

An instrument earns its keep only if it satisfies three conditions. Each maps to a falsifiable check on this dataset.

**1. Relevance (the first stage).** Z must actually shift D: Cov(Z, D) ≠ 0. If cohort timing barely moved which deck got delivered — because reps switched on their own schedule regardless of training, or because old slides lingered in the field — the instrument is *weak* and everything downstream is unreliable. The check is the **first-stage F-statistic** on the instrument. The source essay states the bar as F > 10. That bar is wrong as usually understood, and §4.4 fixes it.

**2. Independence (exogeneity).** Z must be as-good-as-randomly assigned with respect to the physicians' potential outcomes — cohort timing must be uncorrelated with how much these physicians would prescribe anyway. This is exactly what failed in the opening case (star reps trained first). It is **testable**, and you are required to test it: regress cohort assignment on **pre-rollout** prescribing. If early-trained reps systematically cover higher-baseline physicians, the coefficient is significant and the design is dead. You do not patch this; you stop. This falsification regression is the load-bearing move of the chapter (§4.6, and the midterm).

**3. Exclusion (the exclusion restriction).** Z must affect Y *only through* D — cohort timing must change prescribing only by changing the message, with no direct side channel. The obvious threat: if early training also improves a rep's general selling skill, or grants more selling days, or more manager attention, or earlier sample supply within the measurement window, then early-trained reps lift NRx for reasons that have nothing to do with the deck. Exclusion is **not directly testable** (it is a statement about a channel you cannot observe), but you defend it by **controlling rep tenure and baseline performance**, so that "early-trained" is not standing in for "more experienced and better at selling," and by checking that the event-study leads are flat (no effect before exposure).

A **fourth** assumption rides along with the LATE interpretation: **monotonicity** — no "defiers." A defier here would be a rep who, *trained later*, reverts to the *old* message, or one who, trained earlier, refuses the new one. If the instrument pushes some reps toward treatment and others away, the LATE loses its clean interpretation. Reps reverting to a familiar old deck is plausible enough to name explicitly; you should state monotonicity as an assumption, not assume it silently.

### 4.4 The F > 10 rule is a legacy rule — fix it

Here is a misconception you very likely arrived with, stated as the source essay states it:

> "First-stage F > 10 means the instrument is strong enough to trust the t-test."

This is wrong, and now formally so. The F > 10 number traces to Staiger & Stock (1997, *Econometrica* 65(3):557–586) and was made precise by Stock & Yogo (2005, in Andrews & Stock, eds., *Identification and Inference for Econometric Models*, Cambridge UP) as a **relative-bias** criterion: F > 10 roughly guarantees that the bias of the 2SLS estimator is under ~10% of the bias of OLS. Notice what that controls — **bias**, the location of the estimate. It says nothing about whether the **t-test you actually report** has correct size.

Lee, McCrary, Moreira & Porter (2022, *American Economic Review* 112(10):3260–3290) worked out what is actually required for the conventional 5% t-test (|t| > 1.96) to have correct size in the just-identified single-instrument case — which is exactly our case, one cohort instrument. The answer is a first-stage F of roughly **104.7**, not 10. Their **tF procedure** fixes this by inflating the standard error as a smooth function of the observed F: weaker first stages get larger SEs, automatically. When they re-examined 61 published AER papers, about **a quarter of specifications needed standard errors at least 49% larger** to be valid at the 5% level. A quarter of published, peer-reviewed instrumental-variables results were overstating their precision.

The teaching move for this dataset is therefore: report the first-stage F, but do not stop there. With a single cohort instrument, apply the **tF** standard-error adjustment, and/or report **Anderson–Rubin** weak-instrument-robust confidence sets (which stay valid however weak the instrument is), and use the **effective F** of Montiel Olea & Pflueger (2013) [verify cite] because NRx data is clustered by rep and territory and the classical F assumes it is not. The review by Andrews, Stock & Sun (2019, *Annual Review of Economics* 11:727–753) is the practitioner's summary and recommends exactly this combination. The lesson generalizes past IV: a threshold that controls one quantity (bias) does not license a claim about a different quantity (inferential precision), and "everyone uses F > 10" is not a defense.

### 4.5 The staggered-rollout trap (a dead end to walk through)

Suppose independence and exclusion hold and you want to estimate the effect directly, treating "delivered the new deck" as a treatment that switches on at different times for different physicians. The natural move is a **two-way fixed effects (TWFE)** regression: regress NRx on a "post-treatment" dummy plus physician and time fixed effects. This is the workhorse difference-in-differences specification, and with staggered timing it is a trap.

The reason was made precise only recently. Goodman-Bacon (2021, *Journal of Econometrics* 225(2):254–277) showed that staggered TWFE is a weighted average of all possible 2×2 difference-in-differences comparisons — and some of those comparisons are **forbidden**: they use *already-treated* early cohorts as the "control" group for *later*-treated cohorts. When treatment effects change over time, those forbidden comparisons enter with **negative weights**, so the TWFE coefficient can be a badly distorted, even **sign-flipped**, summary of the true effects. de Chaisemartin & D'Haultfœuille (2020, *AER* 110(9):2964–2996) independently demonstrated the negative-weighting problem and proposed estimators that avoid it. You can have every treatment effect positive and a negative TWFE coefficient.

The fix is a **heterogeneity-robust estimator** that never uses already-treated units as controls: Callaway & Sant'Anna (2021, *Journal of Econometrics* 225(2):200–230) estimate group-time average treatment effects ATT(g, t) using **not-yet-treated** cohorts as clean controls (their `did` package); Sun & Abraham (2021, *Journal of Econometrics* 225(2):175–199) give an interaction-weighted estimator that also exposes spurious pretrends. The recent practitioner synthesis is Baker, Callaway, Cunningham, Goodman-Bacon & Sant'Anna (2025, "Difference-in-Differences Designs: A Practitioner's Guide," arXiv:2503.13323). The discipline: use a robust estimator with not-yet-trained cohorts as controls, *then* build the cohort instrument on top. This trap and its fix are taught here and executed in Chapter 10 (Project 1).

## 5. Worked example on this dataset

Let us run the design the way you should run it, dead ends included.

**Step 1 — Build the panel.** Link three layers. From the CLM rep-entered plane, take `Call2_Key_Message_vod__c` to identify which deck each physician's rep was delivering and when (treatment D). From HR / training records, take each rep's training-cohort timing (instrument Z). From the warehouse, take NPI-level NRx at 30/60/90 days (outcome Y) and the physician's **pre-rollout** NRx history. Add territory fixed effects, rep tenure, and a baseline rep-performance measure. The unit of analysis is physician-by-time.

**Step 2 — Falsification FIRST.** Before estimating anything, regress cohort assignment on pre-rollout NRx (plus specialty and territory). This is the test that the opening-case team skipped. Suppose it comes back significant: early-trained reps systematically cover physicians whose *pre-rollout* prescribing was already higher. Independence has failed. The honest action is to stop and report the failure — the design does not identify the message effect on this brand's rollout. This is not a setback to hide; it is the deliverable. (In a teaching run on the synthetic dataset, where the ground-truth structure is known, you can show the test correctly flagging a planted violation and correctly passing when there is none.)

**Step 3 — A dead end, walked through.** Suppose falsification passes and you reach for the quick estimate: a TWFE regression of NRx on a post-training dummy. Resist the urge to report the coefficient. Run the Goodman-Bacon decomposition on it and you will likely find a chunk of weight sitting on forbidden comparisons — late cohorts judged against already-trained early cohorts. The coefficient is a contaminated summary. **Lesson:** with staggered timing, swap in Callaway–Sant'Anna using not-yet-trained cohorts as controls before you trust any number.

**Step 4 — Estimate, with the instrument and honest inference.** With clean controls in place, run the first stage (cohort → deck delivered) and confirm relevance. Do **not** declare victory at F > 10; report the effective F and, if it is anywhere near the danger zone, the tF-adjusted SE and Anderson–Rubin confidence set. Then read off the IV estimate of the message effect on NRx, controlling rep tenure and baseline performance to defend exclusion, and check that the event-study leads are flat.

**The limit.** Even a clean, well-instrumented estimate gives you a **population LATE for compliers** — the average effect of the safety-first message among physicians whose exposure tracked cohort timing. It does not tell the brand "which message should this physician get *next week*?" That individual, Rung-3 question is a counterfactual on a parameterized model, and it is handed to Chapter 9. And the LATE is a *total* effect — it lumps together every channel by which the message moved prescribing. The next chapter pulls that total apart into pathways, because the channels carry different ethical and regulatory weight.

## 6. Common misconceptions

- **"F > 10 means the instrument is strong."** It means 2SLS bias is probably small relative to OLS bias. It does *not* mean your reported t-test has correct size; for that, in the single-instrument case, you need F near 104.7 or the tF correction (Lee et al. 2022).
- **"IV gives the average treatment effect."** With heterogeneous effects, IV gives the LATE — the effect for compliers only, a subpopulation you cannot name. Report it as such.
- **"Independence is an assumption you state and move on."** Independence is *testable* here. You must regress cohort timing on pre-rollout prescribing and let the data try to kill your design.
- **"A natural experiment means I can skip the controls."** Exclusion is the soft spot. Without rep-tenure and baseline-performance controls, 'early-trained' silently encodes 'better rep,' and you are back to the opening-case failure.
- **"Just run difference-in-differences on the rollout."** Naive staggered TWFE can negatively weight forbidden comparisons and even flip the sign of the true effect. Use a heterogeneity-robust estimator.

## 7. Evidence check and consent/compliance & patient-welfare check

**Evidence check — classify what you are leaning on.**

- *Independent / peer-reviewed (strong).* The IV and LATE framework (Imbens & Angrist 1994; Angrist, Imbens & Rubin 1996), the weak-instrument literature (Staiger & Stock 1997; Stock & Yogo 2005; Lee et al. 2022; Andrews, Stock & Sun 2019), and the staggered-DiD results (Goodman-Bacon 2021; de Chaisemartin & D'Haultfœuille 2020; Callaway & Sant'Anna 2021; Sun & Abraham 2021) are all peer-reviewed and, for the last group, the current standard.
- *Hypothetical / author-constructed.* The specific brand rollout, the star-reps-trained-first opening case, and the synthetic-data demonstration are illustrative constructions, not observed studies. They show how the method behaves; they are not evidence that a real rollout satisfies the conditions.
- *Vendor.* No vendor claim is relied on. NBA-vendor material is the foil from Chapter 3, not a source.
- *Contested / [verify].* That cohort timing is a *valid* instrument for any given brand is **conditional, not established** — it holds only if independence survives the falsification test (Adoption Risk #4: identification assumptions oversold). Exact Angrist & Krueger (1991) and Angrist (1990) pages are [verify]; the Montiel Olea & Pflueger (2013) effective-F citation is [verify].

**Consent / compliance & patient-welfare check.** This chapter estimates a *total* message effect and does not yet decompose it into pathways, so it does not by itself tell you whether the effect you are estimating runs through a legally drivable channel (education) or a regulated one (reciprocity) — that is Chapter 5's job, and it matters before anything is acted on. Two cautions even here: first, the warehouse linkage uses NPI-level prescribing data; the public/IP firewall means you teach and test this on **synthetic rep-visit data and public sources (Open Payments, Part D)**, while the partner replicates on real CRM internally — no proprietary clickstream enters the public work. Second, the *patient-welfare* frame: a "message effect on NRx" is a change in prescribing, which is a change in what patients are given. Estimating it accurately is a precondition for asking whether the change is clinically warranted; it is not a license to maximize it.

## 8. Named Fellow artifact: the IV + falsification design

Produce a one-page **identification design** for a specific message transition in your thread's dataset (synthetic or public). It must contain, in order:

1. **The three-step DAG**, drawn in ASCII, with the instrument visibly upstream of treatment:

   ```
   training cohort (Z) ──▶ message variant (D) ──▶ NRx (Y)
                                  ▲
            unobserved physician │  (confounds D and Y;
            openness / potential ┘   the reason we need Z)
   ```

2. **The IV mapping**: one line each naming Z, D, Y with the exact field or record they come from.
3. **The three conditions, each with its check**: relevance → first-stage F + effective-F/tF plan; independence → the falsification regression (written out: `cohort ~ pre_rollout_NRx + specialty + territory`); exclusion → the control set (rep tenure + baseline performance) and the flat-leads check. Add one line stating the monotonicity (no-defiers) assumption.
4. **The kill criterion**: the explicit rule "if the falsification coefficient is significant at the 5% level, the design is abandoned and reported as failed." 

This artifact is the course midterm ("identify the graph + design the falsification test"). The grading emphasis is order of operations: a design that shows an effect estimate *before* establishing that the falsification test passes is marked incomplete, by construction.

## 9. Research finding for Fellows

The cohort-rollout instrument is the field's overlooked natural experiment. Pharma has run staggered message rollouts for over a decade, generating exactly the quasi-random variation econometricians spend careers searching for — and the variation sits unexploited because the data is read as a CRM log, not as an experiment. The first team to estimate a defensible, falsification-tested message effect from cohort timing has a result that is both publishable (it is a clean application of credibility-revolution methods to a new dataset) and directly productizable (it becomes Project 1, the staggered-DiD study in Chapter 10). The open empirical question is not whether the instrument *can* work — the structure is sound — but whether independence *survives* on any particular brand's rollout. That is an invitation to run the falsification test on real rollouts and report which ones pass.

## 10. Exercises

1. **(Understand)** For the draft-lottery and quarter-of-birth examples, name Z, D, and Y in each, and state in one sentence why the instrument plausibly satisfies exclusion. Then do the same for the cohort-rollout design.
2. **(Analyze)** A colleague reports a first-stage F of 22 and a 5% t-test that just clears 1.96. They conclude the message effect is "significant." Using Lee et al. (2022), explain what is wrong, and state what you would compute instead before believing the result.
3. **(Apply+)** On the synthetic rep-visit dataset, run the falsification regression (cohort on pre-rollout NRx) twice: once on a version with a planted independence violation, once on a clean version. Report both coefficients and state, for each, whether you would proceed.
4. **(Create — produces an artifact)** Build the full Named Fellow artifact of §8 for a message transition in your thread's data: the DAG, the IV mapping, the three conditions with checks, and the explicit kill criterion. This is your midterm submission.

## 11. Five-part AI block

**When to use AI.** Use an LLM to scaffold the econometrics you are about to run — to draft the `did` (Callaway–Sant'Anna) or `linearmodels`/`statsmodels` IV-2SLS code, to remind you of the syntax for an Anderson–Rubin confidence set, or to translate a method paper's equations into a runnable estimation. It is also good at generating the *checklist* of threats to independence and exclusion you should consider for a specific rollout.

**When NOT to use AI.** Do not let an LLM decide whether your instrument is valid. Independence and exclusion are domain judgments about *this* rollout — who trained first and why, whether early reps got more selling days — that depend on facts the model does not have and will confabulate if asked. Do not accept an F > 10 "it's fine" from a model; the model is repeating the legacy rule. And never let it skip the falsification test to get you to an estimate faster.

**LLM exercise (ready-to-paste prompt):**

> I have a staggered rollout of a new sales message across training cohorts, and I want to use cohort timing as an instrument for message exposure to estimate the effect on prescribing (NRx). Here is my setup: [paste your Z/D/Y mapping]. Do three things: (1) list every distinct way the *independence* assumption could fail for a training-cohort instrument, and for each, name the falsification check that would detect it; (2) list the threats to the *exclusion restriction* specific to sales-rep training and the control variables that would defend against each; (3) tell me explicitly which of these checks the data can test and which are assumptions I must argue. Do not estimate anything or tell me whether my instrument is valid — only enumerate the threats and checks.

**CLI exercise (Claude Code / terminal).** In your repo, ask Claude Code to scaffold a reproducible falsification-first pipeline: a script that (a) loads the synthetic panel, (b) runs and prints the falsification regression with its p-value and a hard `assert`/exit that stops the pipeline if it is significant at 5%, and (c) only then proceeds to the first stage and the Callaway–Sant'Anna estimate. The point is to encode the order-of-operations discipline in code, so the estimate *cannot* run until the test passes.

**AI validation.** Whatever the AI scaffolds, validate it against the known ground truth in the synthetic dataset: the planted message effect is known, so check that your pipeline recovers it (within its confidence set) on the clean data and *fails loudly* on the version with the planted independence violation. If the AI-generated code happily produces a confident estimate on the violated version, the code is wrong, not the data.

## 12. AI Use Disclosure

The methods, citations, and structure of this chapter were drafted from human-prepared research notes with citations already gathered; an AI assistant helped compose prose from those notes. Every empirical and method claim is sourced to the named primary literature or flagged [verify]. The IV-validity judgment for any real rollout — whether independence and exclusion actually hold — is a human/domain call no model can make, and the chapter says so repeatedly. No proprietary data was used.

## 13. What would change my mind

I would revise the central claim of this chapter — that cohort timing is a usable instrument — if, run on real rollouts, the falsification test failed routinely: if cohort assignment turned out to be correlated with pre-rollout prescribing across most brands (because high-potential territories are systematically front-loaded), the instrument would be invalid in practice even though it is sound in principle, and the chapter would have to retreat to "valid only in the rare rollout where logistics genuinely dominate." I would also revise if a credible reanalysis showed that early training so reliably improves selling skill that the exclusion restriction cannot be defended by tenure/baseline controls — that would move cohort timing from "conditional instrument" to "not an instrument."

## 14. Still puzzling

How large is the complier subpopulation, really? The LATE is the effect for physicians whose message exposure tracked cohort timing — but if most reps switch decks on their own schedule regardless of training, the complier group could be small and unrepresentative, and the LATE would answer a question about a sliver of the market. There is no fully satisfying way to characterize compliers from the data alone. It is also unresolved how to combine the cohort instrument with the staggered-DiD machinery formally — the chapter teaches both and says "build the instrument on top of the robust estimator," but the clean joint estimand is a live methodological question.

## 15. Summary

Pharma's staggered training rollouts generate quasi-random variation in which message a physician received, and that variation is an instrument. Mapped to this dataset: instrument = cohort timing, treatment = the deck delivered, outcome = NRx. An instrument must satisfy relevance (first stage), independence (testable — falsify by regressing cohort on pre-rollout prescribing), and exclusion (defended by rep-tenure and baseline controls), plus monotonicity for a clean LATE. The F > 10 rule controls bias, not inferential size; modern practice needs a far larger effective F or the tF correction. Naive staggered TWFE can sign-flip the effect through forbidden comparisons; use Callaway–Sant'Anna. Above all: run the falsification test *first*, and refuse to report an estimate until it passes. What you get is a population LATE — a *total* effect for compliers — which the next chapter decomposes into pathways.

## 16. Key terms

- **Natural experiment** — an as-if-random assignment occurring in the world, outside the analyst's control and unrelated to the outcome, exploitable to identify a causal effect.
- **Instrumental variable (IV)** — a variable Z that shifts treatment D and affects outcome Y only through D, used to identify D→Y despite unmeasured confounding.
- **First stage / reduced form** — the regression of treatment on the instrument; the regression of the outcome on the instrument.
- **Relevance, independence, exclusion** — the three IV conditions: Z moves D; Z is as-good-as-random w.r.t. potential outcomes; Z affects Y only through D.
- **Monotonicity / defiers / compliers** — no units move against the instrument (defiers); compliers are units whose treatment tracks the instrument; IV identifies the effect for compliers.
- **LATE (Local Average Treatment Effect)** — the average treatment effect among compliers, which is what IV recovers under heterogeneity.
- **Weak instrument / tF correction** — an instrument with a small first stage; the tF procedure inflates the standard error as a function of the observed F so the t-test stays valid (Lee et al. 2022).
- **TWFE / forbidden comparison / Callaway–Sant'Anna** — two-way fixed effects DiD; the bias from using already-treated units as controls under staggered timing; the heterogeneity-robust estimator that avoids it.
- **Falsification test** — a check designed to *kill* a design if it is invalid; here, regressing cohort timing on pre-rollout prescribing.

## 17. Bridge

The experiment identifies an *average* effect — but the effect runs through several pathways. A message can move prescribing because the physician learned something, because she got samples and follow-up, or because a meal created a sense of obligation. Those channels carry very different ethical and regulatory weight, and the total effect lumps them together. Chapter 5 draws the message→prescribing relationship as a three-pathway mediation graph and asks what it takes to pull the channels apart.

## 18. Further reading

- Imbens, G. & Angrist, J. (1994). "Identification and Estimation of Local Average Treatment Effects." *Econometrica* 62(2):467–475.
- Angrist, J., Imbens, G. & Rubin, D. (1996). "Identification of Causal Effects Using Instrumental Variables." *Journal of the American Statistical Association* 91(434):444–455.
- Lee, D., McCrary, J., Moreira, M. & Porter, J. (2022). "Valid t-Ratio Inference for IV." *American Economic Review* 112(10):3260–3290.
- Andrews, I., Stock, J. & Sun, L. (2019). "Weak Instruments in Instrumental Variables Regression: Theory and Practice." *Annual Review of Economics* 11:727–753.
- Stock, J. & Yogo, M. (2005). "Testing for Weak Instruments in Linear IV Regression." In Andrews & Stock (eds.), Cambridge University Press.
- Goodman-Bacon, A. (2021). "Difference-in-Differences with Variation in Treatment Timing." *Journal of Econometrics* 225(2):254–277.
- Callaway, B. & Sant'Anna, P. (2021). "Difference-in-Differences with Multiple Time Periods." *Journal of Econometrics* 225(2):200–230.
- de Chaisemartin, C. & D'Haultfœuille, X. (2020). "Two-Way Fixed Effects Estimators with Heterogeneous Treatment Effects." *American Economic Review* 110(9):2964–2996.
- Baker, A., Callaway, B., Cunningham, S., Goodman-Bacon, A. & Sant'Anna, P. (2025). "Difference-in-Differences Designs: A Practitioner's Guide." arXiv:2503.13323.
- Angrist, J. & Pischke, J.-S. (2010). "The Credibility Revolution in Empirical Economics." *Journal of Economic Perspectives* 24(2):3–30.
