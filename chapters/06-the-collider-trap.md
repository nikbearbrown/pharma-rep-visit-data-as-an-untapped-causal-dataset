# Chapter 6: The Collider Trap (and the Consent Collider)

## 1. Overview

Everything you have learned so far rewards one instinct: be careful, control for confounders, do not let lurking variables fool you. This chapter is where that instinct turns into the bug. Some of the most natural-looking "controls" in rep-visit data do not clean up your estimate — they corrupt it, and they do so in a way that makes the result look *better*: tighter, more confident, more wrong.

The villain is the **collider**: a variable that two arrows point *into* rather than out of. Conditioning on a confounder closes a spurious path and helps you. Conditioning on a collider *opens* a spurious path and hurts you. It is the one place where d-separation inverts, and it is precisely where the "control for more stuff" reflex misfires. In this dataset the trap is baited with two of the most tempting features the telemetry offers — **slide dwell time** (`Duration_vod`) and **reaction scores** (`Reaction_vod`) — which look exactly like measures of "how engaged the physician was" that any sensible analyst would want to adjust for. They are post-treatment colliders, and adjusting for them manufactures bias in the message effect.

Then the chapter turns to a subtler and more consequential collider, one that does not live in any column you can see: **physician consent**. Veeva's opt-out mechanism governs whether a visit is logged at all. If consent depends on both treatment-side factors (engaged reps, high-contact physicians) and outcome-side factors (prescribing propensity, industry-skepticism), then the very act of analyzing the *consenting subsample* is conditioning on a collider — a Berkson-style selection structure baked into the sampling frame. The bias does not show up in any covariate; it hides in who is in the dataset at all, and a model fit on consenters generalizes wrong, silently.

Because the data plausibly has both latent confounders and a selection collider, the standard causal-discovery algorithms (PC, GES) are invalid here — they assume away exactly these features. The chapter closes by motivating **FCI**, the latent-variable-aware discovery algorithm whose output (a PAG) is honest about what cannot be oriented.

What you will be able to do: identify a collider by its structural position, predict the direction and danger of the bias from conditioning on dwell/reaction, recognize consent opt-out as a selection collider, and explain why FCI is the right discovery tool when causal sufficiency fails.

## 2. Learning objectives

By the end of this chapter you will be able to:

- **(Analyze)** Identify a collider by its structural position (two incoming arrows) and distinguish it from a confounder and a mediator.
- **(Evaluate)** Predict the bias from conditioning on slide dwell time and reaction scores when the target is the total message effect, and explain why a tighter, "better-fitting" estimate can be more wrong.
- **(Analyze)** Explain physician consent opt-out as a Berkson selection collider on the sampling frame, and why ignoring opt-out missingness is conditioning on a collider.
- **(Evaluate)** Justify choosing FCI over PC/GES when the dataset has latent confounders and selection, and interpret a PAG's uncertainty marks.

## 3. Opening case (failure-first)

An analyst is asked for the effect of a new message variant on NRx. She has the cohort instrument from Chapter 4 and a clean total-effect estimate in hand. But she worries: "physicians who barely glanced at the deck can't have been moved by it — I should control for how engaged they were, or I'm mixing real exposures with non-exposures." She has the perfect variables for it. The CLM telemetry gives her `Duration_vod` (seconds dwelt per slide) and `Reaction_vod` (the rep's positive/neutral/negative button per slide). She adds both to the model as engagement controls.

The estimate changes. It moves, and — this is the seductive part — it *tightens*. The standard error shrinks; the coefficient looks more decisive. By every diagnostic she habitually trusts (fit improved, precision up, "we accounted for engagement"), the new model is better. She reports it.

It is confidently wrong. Dwell time and reaction are not measures of a pre-existing trait she is adjusting away. They are **consequences of the message** — caused by the rep's call *and* by physician-level traits that also drive prescribing. They are colliders sitting downstream of treatment. By conditioning on them she opened a spurious path between the message and NRx that was closed before she touched it. The improved fit is the symptom, not the cure: she added variables correlated with the outcome, so of course fit improved and the SE shrank — but the correlation she exploited is the very bias she introduced. The honest model was the one *without* the engagement controls. The diagnostic that would have saved her is not a fit statistic; it is a single structural question she never asked: *is this variable upstream or downstream of the treatment?*

## 4. Core sections

### 4.1 The collider, structurally

A **collider** on a path is a node with **two arrows pointing into it**: A → C ← B. The name is the picture — two causal arrows "collide" at C. C is a *common effect* of A and B.

The behavior of a collider is the opposite of everything else in a DAG, and this is the one fact to burn in. For a **chain** (A → C → B) or a **fork** (A ← C → B), A and B are dependent, and *conditioning on the middle node C removes* that dependence — this is the ordinary "controlling for a confounder" move. For a **collider** (A → C ← B), A and B are marginally **independent** through C — the path is *blocked* when you leave C alone — and **conditioning on C (or on any descendant of C) opens the path**, inducing a *spurious, non-causal* association between A and B. This is the single inversion in d-separation (Pearl 2009, *Causality*, 2nd ed., Cambridge UP [verify edition/pages]; Greenland, Pearl & Robins 1999, *Epidemiology* 10(1):37–48; worked illustration in Cole et al. 2010, *International Journal of Epidemiology* 39(2):417–420).

The intuition that makes it stick: suppose two independent causes can each produce an effect. Flu and food poisoning both cause fever; they are unrelated in the population. Now look only at feverish people. Among them, if someone *doesn't* have flu, the fever has to be explained somehow, so food poisoning becomes more likely — flu and food poisoning are now negatively associated *within the feverish group*, though they were independent overall. Conditioning on the common effect (fever) manufactured a relationship between its causes. That manufactured relationship is collider bias.

### 4.2 Post-treatment / overadjustment bias

Now specialize to the case where the collider sits *downstream of treatment*. Adjusting for any variable **affected by the treatment** — a mediator or a post-treatment collider — biases the estimate of the **total** treatment effect, and this happens **even in a randomized experiment**. Randomization protects the treatment node from confounding; it does nothing to protect anything measured *after* treatment, because those variables are no longer randomized once the treatment has acted on them. This is **overadjustment bias** (Rosenbaum 1984, *JRSS-A* 147(5):656–666; Schisterman, Cole & Platt 2009, *Epidemiology* 20(4):488–495).

The most pointed recent statement is Montgomery, Nyhan & Torres (2018, *American Journal of Political Science* 62(3):760–775), whose title is the warning: conditioning on post-treatment variables can ruin your experiment. They show that the damage is not limited to putting the variable in a regression — **dropping or subsetting** your sample on a post-treatment criterion is just as destructive. (Hold that point; it is the bridge to consent in §4.4: restricting to consenters is subsetting on a post-treatment-ish variable.)

This is also the precise, teachable contrast with Chapter 5. There, you *wanted* the mediator — you modeled it deliberately to get the pathway split. Here, for the *total* effect, you must *not* condition on the same kind of downstream variable. Same nodes, opposite goal. If your target is "the share of the effect through education," dwell time might inform a mediation analysis (under heavy assumptions). If your target is "the total effect of the message," dwell time is poison. The variable did not change; the estimand did.

### 4.3 The pharma instance: dwell and reaction are colliders

Here is the chapter's core claim, made structural. `Duration_vod` (slide dwell), `Reaction_vod` (the per-slide reaction score), and other in-visit signals **look like confounders** — quantities an analyst would "control for engagement" with. They are not confounders. They are **common effects of the message (treatment) and of physician-level traits that also drive prescribing**:

```
        physician openness / interest
              │            │
              ▼            ▼
   message (D) ──▶  dwell / reaction  ◀── (same trait drives NRx)
              │         (collider)
              └────────────▶ NRx (Y)
```

The message drives how long the physician dwells and how she reacts; her underlying openness drives both her engagement *and* her prescribing. So dwell/reaction is a collider on a path between the message and NRx. Leave it alone and that path stays blocked. Condition on it — "control for engagement" — and you open a spurious message↔NRx path, biasing the total effect. The opening case is exactly this, and the tell (tighter, better-fitting estimate) is the trap: you added outcome-correlated variables, so fit *had* to improve, but the improvement is the bias.

The misconception, stated and refuted:

> "Slide dwell time / reaction score is a useful feature — control for engagement to clean up the estimate."

Wrong, structurally. Engagement is *downstream* of the message. Putting it in the model is textbook overadjustment (Schisterman et al. 2009), and because it is specifically a collider, it *manufactures* bias rather than reducing it (Montgomery et al. 2018 is the on-point warning). The fix is not a better engagement control. The fix is **not conditioning on post-treatment variables** when the target is the total effect.

### 4.4 Berkson's paradox and selection as a collider

The collider does not have to be a column in your data. It can be the **rule that decided who is in your data at all**. This is **Berkson's paradox** (Berkson 1946, *Biometrics Bulletin* 2(3):47–53): within a *selected* sample, two variables that are independent in the population can appear *negatively* (or otherwise spuriously) associated, because **sample membership is itself a collider** — a common effect of both variables (or of their causes). Berkson's original demonstration was hospital data: two unrelated diseases appear negatively correlated among hospitalized patients because having either raises the chance of admission, so among the admitted, lacking one makes the other more likely.

The modern flagship is Griffith et al. (2020, *Nature Communications* 11:5749): collider bias from non-representative COVID samples (the hospitalized, the tested, volunteers) distorts risk-factor associations, because sample membership conditions on a sampling collider. The structural unification you must internalize is Hernán, Hernández-Díaz & Robins (2004, *Epidemiology* 15(5):615–625), "A Structural Approach to Selection Bias": **selection bias is not a separate phenomenon from collider bias — it is the same structure.** Selecting on S = 1, where S is a common effect of (causes of) exposure and (causes of) outcome, opens a spurious exposure–outcome path. Elwert & Winship (2014, *Annual Review of Sociology* 40:31–53 [verify pages]) is the best single review unifying collider, selection, and overadjustment bias under one roof; read it if you read one thing here.

### 4.5 The consent collider — this dataset's distinctive trap

Now apply selection-as-collider to the feature that makes this dataset ethically and statistically distinctive: **physician consent**. Veeva's `CLM_EXPLICIT_OPT_IN_vod` governs whether CLM activity is logged; opt-out either suppresses the clickstream or anonymizes it (`CLM_OPT_OUT_BEHAVIOR_vod` set to 0/1 strips the NPI from `Multichannel_Activity_vod`). (These Veeva field names are dated mid-2026 examples; verify against current Veeva CLM documentation before print — Veeva-schema velocity is a known aging risk.)

Consider what consent depends on. Plausibly, on **treatment-side factors**: a physician called on by an engaged rep, or a high-contact physician, may be more likely to be enrolled and to stay opted in. And plausibly on **outcome-side factors**: prescribing propensity, industry-skepticism, privacy-consciousness, and institutional limits (some health systems forbid rep data-sharing). If consent (= sample membership) is a common effect of treatment-related and outcome-related factors, then:

```
   engaged rep / high contact ──▶  consent (S=1)  ◀── industry-skepticism /
        (treatment-side)                                prescribing propensity
                                                            (outcome-side)
```

**S = consent is a selection collider.** Analyzing only the consenting subsample is conditioning on S = 1, which — by Hernán et al.'s definition — opens a spurious message→NRx path. Opt-out missingness is almost certainly correlated with physician characteristics, making it a sampling-frame collider that most analyses silently ignore. This is the chapter's research finding and a load-bearing ethics item (Adoption Risk #5): consent missingness is *not* ignorable; treating it as ignorable is conditioning on a collider.

The failure case has teeth. A targeting model fit on consenting physicians estimates a biased message→NRx effect (selection collider), and when deployed across the *full* prescriber population it generalizes wrong — **silently**, because the bias lives in the sampling frame, not in any visible covariate. The privacy-conscious, industry-skeptical physicians who opted out also prescribe differently; the model never saw them and quietly assumes they behave like consenters. (This is the Chapter 13 opening: "a model that works on the consenting subsample and silently generalizes wrong.")

### 4.6 Why FCI, not PC or GES

If the data has *both* latent confounders (unobserved physician propensity behind the dwell/reaction collider) *and* selection (consent), then the standard causal-discovery algorithms are simply invalid, and you should know *why*, not just *which*. The PC algorithm and GES (Chickering 2002, *JMLR* 3:507–554) both assume **causal sufficiency** — that there are no unmeasured common causes and no selection — and they are correct *only* under that assumption (Spirtes, Glymour & Scheines 2000, *Causation, Prediction, and Search*, 2nd ed., MIT Press [verify edition/pages]). Causal sufficiency is exactly what fails here, by construction: latent physician openness is the unobserved common cause, and consent is the selection.

The right tool is **FCI** (Fast Causal Inference), the latent-variable- and selection-aware discovery algorithm (Spirtes, Glymour & Scheines 2000; completeness of its orientation rules established by Zhang 2008, *Artificial Intelligence* 172(16–17):1873–1896). FCI outputs a **PAG** (Partial Ancestral Graph) rather than a single DAG. The PAG is *honest about uncertainty*: where the data cannot determine an edge's direction — because a latent confounder or selection could explain it — FCI leaves a circle mark or a bidirected edge rather than committing to a direction it cannot justify.

But mark the practical caveat clearly: FCI is **correct but coarse**. It often leaves many edges only partially oriented, because honesty about latent confounding *means* leaving questions open. FCI is the *honest* discovery tool for this dataset, not a magic edge-orienter. The edges it leaves undetermined are exactly the ones that need the rep structural knowledge of Chapters 7–8 to orient. That is the bridge: even with colliders handled and the right discovery tool chosen, the data cannot orient every edge.

## 5. Worked example on this dataset

**Step 1 — State the goal precisely.** The target is the **total** effect of the message variant on NRx. (If it were a pathway share, this would be Chapter 5, and the analysis would differ — naming the estimand first is the discipline.)

**Step 2 — A dead end, walked through.** An analyst adds `Duration_vod` and `Reaction_vod` "to control for how engaged the physician was." The point estimate moves and the standard error tightens. By her usual diagnostics the model improved. **Lesson:** classify variables by *structural position*, not by whether they improve fit. Dwell and reaction are downstream of the message — post-treatment colliders. For the total effect they must be left out. The improved fit was the bias announcing itself.

**Step 3 — The diagnostic that fixes the reflex.** For every candidate control, ask one question: *is this variable upstream or downstream of treatment?* Upstream (specialty, baseline prescribing, territory, rep tenure) is fair game for confounding control. Downstream (dwell, reaction, in-visit signals, samples-left) is off-limits for the total effect. Position, not predictive power, decides inclusion. On the synthetic dataset, where the ground-truth structure is known, you can *demonstrate* the bias: estimate the total effect with and without the engagement controls and watch the controlled version diverge from the planted truth.

**Step 4 — The consent layer.** Even with colliders handled inside the model, the *sample* is selected on consent. Put the consent mechanism *in the graph* as a selection node S. Recognize that you can only ever estimate effects on the **consenting subsample**, and that this generalizes wrong to the opt-out population. On the synthetic data, simulate a consent rule that depends on both engagement and prescribing propensity, then show the consenting-subsample estimate diverging from the full-population truth — the bias that lives in the sampling frame.

**The limit / the tool.** Because the data plausibly has both latent confounders and a selection collider, PC and GES are invalid (causal sufficiency fails). Use **FCI**; read its PAG as an honest map of what the data can and cannot orient, expecting many partially-oriented edges. The undetermined edges are the agenda for the rep interviews. Bridge: even with colliders handled, the data can't orient every edge → Chapter 7.

## 6. Common misconceptions

- **"Controlling for more variables is safer."** Only for upstream confounders. Conditioning on a downstream collider *adds* bias, and the false reassurance of a tighter SE makes it worse.
- **"A tighter, better-fitting model is a better causal estimate."** Fit and precision say nothing about bias. Adding outcome-correlated post-treatment variables improves fit *while* corrupting the causal estimate.
- **"Dwell time / reaction score measures pre-existing engagement I should adjust for."** They are caused by the message; they are post-treatment colliders, not pre-treatment traits.
- **"Selection bias and collider bias are different problems."** They are the same structure (Hernán et al. 2004): selecting on a common effect opens a spurious path.
- **"Consent opt-out is missing data we can ignore if it's small."** Opt-out is almost certainly correlated with physician characteristics; analyzing consenters is conditioning on a selection collider, and the bias hides in the sampling frame, not in any covariate.
- **"PC or GES will discover the graph for us."** They assume causal sufficiency, which fails here. Use FCI, and expect a coarse PAG, not a finished DAG.

## 7. Evidence check and consent/compliance & patient-welfare check

**Evidence check — classify what you are leaning on.**

- *Independent / peer-reviewed (strong).* The collider rule and d-separation (Pearl 2009; Greenland, Pearl & Robins 1999; Cole et al. 2010), overadjustment/post-treatment bias (Rosenbaum 1984; Schisterman, Cole & Platt 2009; Montgomery, Nyhan & Torres 2018), Berkson/selection-as-collider (Berkson 1946; Griffith et al. 2020; Hernán, Hernández-Díaz & Robins 2004; Elwert & Winship 2014), and the discovery literature (Spirtes, Glymour & Scheines 2000; Zhang 2008; Chickering 2002) are all peer-reviewed and standard.
- *Hypothetical / author-constructed.* The opening engagement-control case, the consent-selection failure case, and the synthetic-data demonstrations are illustrative constructions showing the mechanism, not observed pharma studies.
- *Vendor.* No vendor claim is relied on; the "control for engagement" instinct is the foil.
- *Contested / [verify].* That opt-out missingness *is* correlated with physician characteristics is stated as "almost certainly" — a highly plausible, **testable** empirical claim, not a proven one; the synthetic dataset lets you demonstrate the bias under known ground truth, which is a demonstration of the *mechanism*, not evidence about any real population. The **Veeva field names** (`CLM_EXPLICIT_OPT_IN_vod`, `CLM_OPT_OUT_BEHAVIOR_vod`, `Multichannel_Activity_vod`, `Duration_vod`, `Reaction_vod`) are dated mid-2026 examples; verify against current Veeva CLM docs. Pearl (2009) and Spirtes-Glymour-Scheines (2000) exact editions/pages, and Elwert & Winship (2014) pages, are [verify].

**Consent / compliance & patient-welfare check.** This chapter *is* the consent-ethics chapter for the dataset, so the check is the content. The load-bearing point: **opt-out missingness is not ignorable**, and treating it as ignorable is conditioning on a selection collider — a statistical error with an ethical face. The compliance consequence: a model built on consenters and deployed on the full population is making decisions about physicians who never consented to be modeled, using a biased estimate that was never valid for them. The patient-welfare consequence runs through that bias: a silently mis-generalizing targeting model allocates detailing (and therefore changes prescribing, and therefore changes what patients receive) on the basis of an effect that does not hold for the opt-out population. The Living Model's **drift monitor** should therefore watch opt-out-rate shifts specifically, because a change in who opts out changes the bias structure over time. Public/IP firewall: the consent mechanism is taught and demonstrated on **synthetic data** with a planted consent rule; the partner replicates on real CRM internally; no proprietary consent records enter the public work.

## 8. Named Fellow artifact: a collider audit

Run a **collider audit** on a candidate feature set for the total-effect estimation in your thread's dataset. Produce a table with one row per candidate feature and these columns:

1. **Feature** (e.g., `Duration_vod`, baseline NRx, specialty, `Reaction_vod`, samples-left, territory, rep tenure).
2. **Structural position** — upstream of treatment / downstream of treatment / the treatment / the outcome.
3. **Role** — confounder / mediator / post-treatment collider / instrument / neither.
4. **Verdict for the total effect** — INCLUDE (upstream confounder) or EXCLUDE (downstream / collider), with one-line reasoning by position, *not* by fit.
5. **If excluded, why "controlling for it" tempts an analyst** — name the false intuition it triggers.

Then append a short **consent section**: a small DAG placing consent S as a selection node (common effect of a treatment-side and an outcome-side cause), one sentence stating that estimates hold only on the consenting subsample, and one sentence on the drift signal (opt-out-rate shift) you would monitor.

An audit that includes dwell or reaction "to control for engagement," or that justifies any inclusion by improved fit, is marked incomplete — the audit must reason by structural position.

## 9. Research finding for Fellows

Opt-out missingness is almost certainly correlated with physician characteristics — a sampling-frame collider that most analyses silently ignore. That is the finding, and it is distinctive: the dwell/reaction collider trap is at least *visible* (the variables are in the data, and a careful analyst can be talked out of using them), but the consent collider is invisible by construction — it lives in who is absent from the dataset. The first team to put the consent mechanism explicitly in the causal graph, demonstrate the resulting bias on synthetic data with a planted consent rule, and quantify how much a consenting-subsample model misgeneralizes to the opt-out population has a result that is both a methodological contribution and a direct compliance argument. The open empirical question is the *magnitude*: how badly does the consent collider bias real estimates, and how fast does drift in the opt-out rate change it? Those are measurable, and they are the agenda.

## 10. Exercises

1. **(Understand)** For each of: baseline prescribing, slide dwell time, specialty, and reaction score, state whether it is upstream or downstream of the message, and whether you would include it when estimating the *total* message effect.
2. **(Analyze)** Re-skin the flu/food-poisoning collider example (Cole et al. 2010) with `Duration_vod`: name the two independent causes, the collider, and the spurious association that appears when you condition on the collider.
3. **(Apply+)** On the synthetic dataset (known ground truth), estimate the total message effect twice — with and without `Duration_vod` + `Reaction_vod` as controls — and report how far the controlled version diverges from the planted truth and which version had the smaller standard error. Explain why the smaller SE is the trap.
4. **(Create — produces an artifact)** Build the §8 collider audit table for a candidate feature set in your thread's data, including the consent-selection DAG and the drift-monitoring line.

## 11. Five-part AI block

**When to use AI.** Use an LLM to render collider/selection DAGs, to recall the d-separation rules and the precise statements of overadjustment and selection-as-collider, and to scaffold a synthetic-data simulation that *demonstrates* the bias (planting a known message effect, a dwell/reaction collider, and a consent rule, then showing the biased vs honest estimates). It is good at generating the structural-position checklist for a feature set.

**When NOT to use AI.** Do not let an LLM pick your control set by predictive importance or fit — feature-importance reasoning is exactly the wrong criterion and will happily recommend including dwell and reaction. Do not accept a causal-discovery result from a model that ran PC or GES on this data; those algorithms assume causal sufficiency, which fails here, and an LLM may not flag the violation. And do not let it treat opt-out as ignorable missingness; the selection structure is a domain fact it will miss.

**LLM exercise (ready-to-paste prompt):**

> I am estimating the total effect of a sales message on prescribing (NRx). Here is my candidate feature set: [paste features, e.g., baseline NRx, specialty, territory, rep tenure, slide dwell time, reaction score, samples left]. For each feature, classify it as upstream or downstream of the message, label it as confounder / mediator / post-treatment collider / instrument / neither, and give an INCLUDE or EXCLUDE verdict for the total effect — reasoning strictly by structural position, never by predictive power or model fit. Then explain, structurally, why conditioning on a post-treatment collider opens a spurious treatment–outcome path. Do not recommend any variable on the grounds that it improves fit.

**CLI exercise (Claude Code / terminal).** In your repo, ask Claude Code to write a simulation that generates synthetic rep-visit data with (a) a known message→NRx effect, (b) a dwell/reaction collider driven by both the message and a latent physician trait, and (c) a consent rule that depends on both engagement and prescribing propensity. Have the script print four numbers: the true effect, the estimate with collider controls, the estimate without them, and the consenting-subsample-only estimate. The script should make the collider bias and the selection bias *visible* against ground truth.

**AI validation.** Validate against the synthetic ground truth: the honest estimate (no post-treatment controls, full sample) should land near the planted effect; the collider-controlled and consenting-only estimates should visibly diverge. If an AI-generated pipeline produces its *tightest* estimate from the collider-controlled spec and presents it as best, that is the trap reproducing itself — flag it.

## 12. AI Use Disclosure

This chapter was composed from human-prepared research notes with citations already gathered; an AI assistant helped turn notes into prose. The structural claims (dwell/reaction as post-treatment colliders; consent as a selection collider; FCI over PC/GES) are carried from the notes and the cited literature, not generated. Empirical claims are sourced or flagged [verify]; the Veeva field names are flagged as dated examples; the "opt-out is correlated with physician characteristics" claim is flagged as highly plausible and testable, not proven. No proprietary data was used.

## 13. What would change my mind

I would soften the dwell/reaction verdict if a credible causal model showed that, in a particular dataset, dwell time is *not* on any open back-door-opening path — for instance, if it were purely a noisy measurement of pre-visit interest assigned before the message, rather than a consequence of the message — because then it would be a pre-treatment proxy, not a post-treatment collider, and might be admissible. I would soften the consent-collider alarm if opt-out turned out to be **as-good-as-random** with respect to both engagement and prescribing (e.g., driven entirely by an institutional policy uncorrelated with physician behavior), in which case the consenting subsample would be a fair sample and the selection bias would vanish. Both are empirical questions; the chapter's position is that the burden is on the analyst to demonstrate these benign conditions, not to assume them.

## 14. Still puzzling

How much does the consent collider actually bias real estimates? The chapter argues the *structure* is there and demonstrates the mechanism on synthetic data, but the magnitude in real CRM data is unknown and, by the nature of the problem, hard to measure — you cannot observe the prescribing of physicians whose data was never logged. Bounding the bias without the missing data is genuinely unresolved. A related puzzle: FCI gives an honest PAG, but a *very* coarse one; it is unclear how much of the edge orientation the rep interviews can realistically supply versus how much will remain permanently undetermined, and therefore how much of the final model rests on elicited judgment rather than data.

## 15. Summary

A collider is a common effect (A → C ← B); conditioning on it opens a spurious path between its causes — the one inversion of d-separation. Slide dwell time and reaction scores are post-treatment colliders, common effects of the message and of physician traits that drive prescribing; "controlling for engagement" with them manufactures bias in the total message effect, and the tighter, better-fitting estimate that results is the symptom, not a cure. Selection bias is the same structure (Hernán et al. 2004): restricting to a sample selected on a common effect opens a spurious path. Physician consent opt-out is precisely such a selection collider — analyzing consenters conditions on it, biasing the estimate and silently misgeneralizing to the opt-out population, with the bias hidden in the sampling frame. Because the data has both latent confounders and selection, causal sufficiency fails, so PC and GES are invalid; FCI is the honest tool, outputting a PAG that marks what cannot be oriented. The diagnostic to drill: position, not predictive power, decides inclusion.

## 16. Key terms

- **Collider** — a node with two incoming arrows (A → C ← B); a common effect of its parents. Conditioning on it (or its descendant) opens a spurious path between the parents.
- **d-separation (and its inversion)** — the graphical criterion for independence; for chains and forks conditioning on the middle node removes dependence, for a collider it creates dependence.
- **Post-treatment / overadjustment bias** — bias in the total effect from adjusting for (or subsetting on) a variable affected by treatment; occurs even in an RCT.
- **Berkson's paradox** — spurious association between two variables within a sample selected on their common effect.
- **Selection bias as collider bias** — selecting on S = 1, a common effect of causes of exposure and outcome, opens a spurious exposure–outcome path (Hernán et al. 2004).
- **Consent collider** — physician opt-out as a selection node; analyzing the consenting subsample conditions on it, biasing estimates and misgeneralizing to opt-outs.
- **Causal sufficiency** — the assumption of no unmeasured common causes and no selection; required by PC and GES, violated here.
- **FCI / PAG** — the latent-variable- and selection-aware discovery algorithm and its output, a partial ancestral graph that leaves honest uncertainty marks where direction cannot be determined.

## 17. Bridge

Even with colliders handled and FCI chosen as the honest discovery tool, the data cannot orient every edge. FCI's PAG is coarse precisely because observational data identifies only an *equivalence class* of graphs, not a single one — and the v-structures (colliders) you learned to spot here are exactly what distinguishes one class from another. Chapter 7 makes this formal: Markov equivalence, why the data can only ever name a class, and why orienting the remaining edges requires structural knowledge the dataset does not contain — the knowledge that lives in the reps.

## 18. Further reading

- Elwert, F. & Winship, C. (2014). "Endogenous Selection Bias: The Problem of Conditioning on a Collider Variable." *Annual Review of Sociology* 40:31–53. [verify pages]
- Hernán, M., Hernández-Díaz, S. & Robins, J. (2004). "A Structural Approach to Selection Bias." *Epidemiology* 15(5):615–625.
- Montgomery, J., Nyhan, B. & Torres, M. (2018). "How Conditioning on Posttreatment Variables Can Ruin Your Experiment and What to Do About It." *American Journal of Political Science* 62(3):760–775.
- Schisterman, E., Cole, S. & Platt, R. (2009). "Overadjustment Bias and Unnecessary Adjustment in Epidemiologic Studies." *Epidemiology* 20(4):488–495.
- Cole, S. et al. (2010). "Illustrating Bias Due to Conditioning on a Collider." *International Journal of Epidemiology* 39(2):417–420.
- Griffith, G. et al. (2020). "Collider Bias Undermines Our Understanding of COVID-19 Disease Risk and Severity." *Nature Communications* 11:5749.
- Greenland, S., Pearl, J. & Robins, J. (1999). "Causal Diagrams for Epidemiologic Research." *Epidemiology* 10(1):37–48.
- Zhang, J. (2008). "On the Completeness of Orientation Rules for Causal Discovery in the Presence of Latent Confounders and Selection Bias." *Artificial Intelligence* 172(16–17):1873–1896.
- Spirtes, P., Glymour, C. & Scheines, R. (2000). *Causation, Prediction, and Search*, 2nd ed. MIT Press. [verify edition/pages]
- Berkson, J. (1946). "Limitations of the Application of Fourfold Table Analysis to Hospital Data." *Biometrics Bulletin* 2(3):47–53.
