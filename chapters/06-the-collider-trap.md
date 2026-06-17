# Chapter 6 — The Collider Trap (and the Consent Collider)
*Why the most natural-looking control in rep-visit data manufactures the bias it claims to remove.*

An analyst is asked for the effect of a new message variant on prescribing. She has a clean cohort instrument and a total-effect estimate in hand. But something bothers her. Physicians who barely glanced at the deck cannot have been moved by it — she is mixing real exposures with non-exposures. She has the perfect variables to fix this. The CLM telemetry gives her seconds-per-slide and a per-slide reaction score that the rep taps positive, neutral, or negative after each exchange. She adds both to the model as engagement controls.

The estimate changes. It tightens. The standard error shrinks; the coefficient looks more decisive. By every diagnostic she habitually trusts — fit improved, precision up, the model now accounts for engagement — the new model is better.

It is confidently wrong.

Dwell time and reaction score are not measures of a pre-existing physician trait she is adjusting away. They are consequences of the message. They were produced by the visit, shaped by both the content of the rep's call and by something about the physician — her underlying curiosity, her receptivity, her professional disposition — that also drives her prescribing. Adding them to the model did not clean up the estimate. It opened a spurious path between the message and prescribing that was closed before she touched them. The tighter standard error is the symptom, not the cure: she added variables correlated with the outcome, so fit had to improve and the SE had to shrink. But the correlation she exploited is the bias she introduced.

The honest model was the one without the engagement controls. The diagnostic that would have saved her is not a fit statistic. It is a single structural question she never asked: *is this variable upstream or downstream of the treatment?*

---

The thing she conditioned on has a name and a picture. A **collider** is a node in a causal graph with two arrows pointing into it — two causes meeting at a common effect. The name is the picture: two causal arrows collide at a single node.

$$A \rightarrow C \leftarrow B$$

Here C is a collider on the path between A and B. C is their common effect.

The behavior of a collider is the one inversion in the logic of causal graphs, and it is worth stating with some care because everything else in the graph behaves differently. In a chain ($A \rightarrow C \rightarrow B$) or a fork ($A \leftarrow C \rightarrow B$), A and B are associated through C, and conditioning on C removes that association — this is the ordinary "control for a confounder" move. It works. But in a collider structure, A and B are marginally *independent* through C — the path between them is blocked when you leave C alone. Conditioning on C, or on any descendant of C, *opens* the path and induces a spurious association between A and B that was not there before.

This is the one place where the control instinct inverts. Controlling for a confounder closes a bad path. Controlling for a collider opens one.

The intuition that makes this stick: suppose two independent causes can each produce the same effect. Flu and food poisoning both cause fever. In the general population they are unrelated. Now restrict to feverish patients. Among them, if someone does not have flu, the fever must be explained somehow — food poisoning becomes more likely. Within the feverish group, flu and food poisoning are negatively associated, though they were independent overall. Conditioning on the common effect manufactured a relationship between its causes.

That manufactured relationship is collider bias. And it is exactly what the analyst produced by conditioning on dwell time.

<!-- → [DIAGRAM: Three-panel structural comparison. Panel 1 (confounder/fork): A ← C → B, with label "Conditioning on C removes the A–B association. This helps." Panel 2 (chain): A → C → B, with label "Conditioning on C blocks the A–B path. Appropriate if C is a mediator you want to block." Panel 3 (collider): A → C ← B, with label "A and B are independent through C. Conditioning on C opens a spurious A–B path. This hurts." Caption: "The collider is the one inversion: controlling for C makes things worse, not better."] -->

---

Now specialize the collider to the case where it sits *downstream of treatment*. This is the overadjustment problem, and it has a feature that makes it particularly dangerous: it can corrupt the estimate even in a randomized experiment.

Randomization protects the treatment assignment from confounding. It does nothing to protect anything measured *after* the treatment acts, because those post-treatment variables are no longer randomized once the treatment has touched them. A variable caused by the treatment is a **mediator** if it lies on the path from treatment to outcome, or a **post-treatment collider** if it is a common effect of the treatment and of something else that also affects the outcome. Adjusting for either one, when your goal is the total effect of treatment, introduces bias — not because the variable is unimportant but because it is downstream.

Montgomery, Nyhan, and Torres (2018) put this plainly: conditioning on post-treatment variables can ruin your experiment. Their analysis covers not only putting the variable in a regression but subsetting or dropping observations on a post-treatment criterion. The damage is structural, not a matter of scale.

This is the precise contrast with Chapter 5. There, the goal was a pathway split — what share of the message effect runs through the educational channel — and modeling the mediator was the point of the exercise. Here, the goal is the total effect, and the same kind of downstream variable becomes poison. The variable did not change; the estimand did. The question you are trying to answer is what determines whether conditioning on something helps or hurts.

---

Here is the chapter's core claim, made structural for the dwell and reaction case.

Slide dwell time (`Duration_vod`) and the per-slide reaction score (`Reaction_vod`) look like measures of pre-existing physician engagement — the kind of thing a careful analyst would naturally want to adjust for. They are not. They are common effects of two things: the message itself, which shapes how long a physician lingers and how the rep records the exchange, and the physician's underlying openness or receptivity, which drives both engagement during the visit and prescribing behavior after it.

```
       physician openness / receptivity
              │                    │
              ▼                    ▼
  message ─────────▶  dwell / reaction  ◀────── (same trait drives NRx)
              │            (collider)
              └────────────────────▶ NRx
```

Leave dwell and reaction alone and the path through them is blocked — the collider sits closed. Condition on them and the path opens, inducing a spurious association between the message and prescribing that runs through the manufactured correlation between the message and physician receptivity. The total effect estimate is now contaminated by that spurious path.

The tell is the tighter standard error. The analyst added variables correlated with the outcome, so fit improved and precision increased — exactly as they would if she had added a genuine confounder. But she did not remove a confounder; she opened a collider. The improved fit is the bias announcing itself in the only language her diagnostics know.

The fix is not a better engagement control. The fix is not conditioning on post-treatment variables when the target is the total effect. The rule is: position decides inclusion. Upstream of treatment — specialty, baseline prescribing, territory, rep tenure — is appropriate for confounding control. Downstream of treatment — dwell, reaction, in-visit signals, samples left behind — is off-limits for the total effect. Not because these variables are uninformative. Because they are downstream, and conditioning on downstream variables when estimating a total effect is structurally guaranteed to introduce bias.

<!-- → [DIAGRAM: The dwell/reaction collider in the pharma DAG. Nodes: "Message (D)" on left, "NRx (Y)" on right, "Physician Openness (latent)" above, "Dwell / Reaction" in the center. Arrows: Message → NRx (direct effect), Message → Dwell/Reaction, Physician Openness → Dwell/Reaction, Physician Openness → NRx. Bold annotation: "Dwell/Reaction is a collider. Conditioning on it opens the Message–NRx path through Physician Openness." Second version of the diagram shows the same graph with a conditioning box around Dwell/Reaction, with a dashed spurious path highlighted between Message and NRx. Caption: "Without conditioning: the collider blocks the spurious path. With conditioning: the path opens and the total effect estimate is corrupted."] -->

---

Now comes the subtler trap, the one that does not live in any column of the data.

A collider does not have to be an observed variable. It can be the rule that determined who is in the dataset at all. This is Berkson's paradox, named for the statistician who showed in 1946 that two unrelated diseases appear negatively correlated among hospitalized patients — because having either disease raises the probability of admission, so among admitted patients, lacking one makes the other more likely. Sample membership is itself a collider. Conditioning on it — which is what you do simply by analyzing the sample — opens a spurious association between its causes.

Hernán, Hernández-Díaz, and Robins (2004) unified this under one roof: selection bias is not a separate phenomenon from collider bias. It is the same structure. Selecting on $S = 1$ where $S$ is a common effect of causes of exposure and causes of outcome opens a spurious exposure-outcome path, exactly as conditioning on a collider does in the interior of a graph. Griffith and colleagues demonstrated this concretely in 2020, showing that COVID risk-factor associations were distorted in hospitalized and tested samples because sample membership was a collider conditioned on by the act of sampling.

Now apply this to the one feature that makes this dataset ethically and statistically distinctive.

Veeva's consent mechanism governs whether a physician's CLM activity is logged at all. The field `CLM_EXPLICIT_OPT_IN_vod` records whether a physician has consented to have their visit interaction captured; opt-out either suppresses the clickstream entirely or anonymizes it, stripping the NPI from the activity record. [Verify these Veeva field names against current CLM documentation — schema naming is a known aging risk.] The consenting subsample is the only sample available for analysis.

What determines whether a physician consents? Consider the plausible causes. On the treatment side: a physician who has been called on frequently by an engaged rep, or who is embedded in a high-contact territory, may be more likely to be enrolled and to remain opted in. On the outcome side: prescribing propensity, industry-skepticism, privacy consciousness, and institutional policies at health systems that prohibit data-sharing with pharmaceutical companies all plausibly govern opt-out decisions.

If consent is a common effect of treatment-related factors and outcome-related factors, then:

$$\text{engaged rep / high contact} \rightarrow S = \text{consent} \leftarrow \text{industry-skepticism / prescribing propensity}$$

Consent is a selection collider. Analyzing the consenting subsample is conditioning on $S = 1$. By the structure Hernán and colleagues formalized, this opens a spurious message-to-prescribing path. Opt-out missingness is almost certainly correlated with physician characteristics — it is a sampling-frame collider that most analyses silently ignore. [This is a highly plausible, testable claim; the synthetic dataset lets you demonstrate the bias under known ground truth, which shows the mechanism, not the magnitude in any real population.]

<!-- → [DIAGRAM: Consent as a selection collider. Nodes: "Rep engagement / visit frequency" (treatment-side cause), "Consent (S = 1)" (center, selection node), "Industry-skepticism / prescribing propensity" (outcome-side cause). Arrows into S from both sides. Below S: "Observed sample = only S = 1 physicians." Annotation: "Analyzing this subsample conditions on S. This opens a spurious message→NRx path, just as conditioning on any collider does." Second panel shows the full population DAG with opt-outs included, and the causal graph free of the selection path. Caption: "The selection collider is invisible in the data — it lives in who is absent."] -->

The failure case has real consequences. A targeting model fit on consenting physicians estimates a biased message-to-prescribing effect. When deployed across the full prescriber population, it generalizes to physicians who never consented to be modeled, using an effect estimate that was never valid for them. The privacy-conscious, industry-skeptical physicians who opted out also prescribe differently. The model never saw them and quietly assumes they behave like consenters. The bias is silent because it lives in the sampling frame, not in any visible covariate — no diagnostic on the observed data can reveal it.

---

Because this dataset has both latent confounders — physician openness is unobserved; it drives both engagement and prescribing but appears in no column — and a selection collider in the consent mechanism, the standard causal discovery algorithms are simply the wrong tool. This is not a matter of preference or computational convenience. It is a matter of validity.

The PC algorithm and GES both assume **causal sufficiency**: no unmeasured common causes, no selection on a collider. Under that assumption they are correct; outside it, they are not. Causal sufficiency fails here by construction. Latent physician openness is the unobserved common cause behind the dwell and reaction collider, and consent is the selection. Any graph the PC algorithm or GES returns is a graph that assumed away the features most likely to matter.

The right tool is **FCI** — Fast Causal Inference — the discovery algorithm designed for data with latent confounders and selection. FCI outputs a **PAG**, a Partial Ancestral Graph, rather than a single DAG. The PAG is honest about what the data cannot determine: where a latent confounder or selection mechanism could explain an edge, FCI leaves a circle mark or a bidirected edge rather than committing to a direction that the data cannot justify. Zhang (2008) established the completeness of FCI's orientation rules under these conditions.

The practical caveat is important. FCI is correct but coarse. It often leaves many edges only partially oriented, because honesty about latent confounding means leaving questions open. A PAG that is half uncertain is not a failure — it is an accurate representation of what the data can and cannot tell you. The undetermined edges are the agenda for the next chapters: the structural knowledge that lives in experienced reps and that no observational algorithm can extract from the telemetry alone.

---

Run the analysis the way it should be done, step by step.

First, state the estimand precisely. The target is the **total** effect of the message variant on prescribing. That choice determines everything that follows, because it is the total effect that forecloses conditioning on dwell and reaction — they are downstream. If the estimand were a pathway share, the analysis would look different.

Second, classify every candidate control by structural position, not by predictive power. Upstream of treatment — specialty, baseline prescribing volume, territory, rep tenure — is fair game. Downstream — dwell time, reaction score, in-visit signals, samples left — is off-limits for the total effect. Position decides inclusion. Fit does not.

Third, on the synthetic dataset where the planted truth is known, demonstrate the bias directly. Estimate the total message effect with and without the engagement controls. The controlled version diverges from the planted truth. The controlled version also has the smaller standard error. That combination — more wrong, more confident — is the collider trap made visible against ground truth.

Fourth, add the consent layer to the graph explicitly. Place the consent mechanism as a selection node — a common effect of treatment-side factors and outcome-side factors. Recognize that estimates from this dataset are estimates on the consenting subsample, and that their validity for the opt-out population depends on an assumption — that opt-out is as-good-as-random with respect to both engagement and prescribing — that is almost certainly false.

Fifth, recognize that causal sufficiency fails, and choose FCI. Read the resulting PAG as a map of what the data can and cannot orient. The undetermined edges are not failures of the algorithm; they are the honest answers to questions the data cannot resolve.

The limit is real and worth stating plainly. Even after handling colliders correctly and choosing the right discovery algorithm, the PAG will be coarse. Observational data identifies an equivalence class of graphs, not a single graph. Orienting the remaining edges requires structural knowledge — the knowledge that lives in the people who have been running these calls for years — and that is what the next chapters supply.

**Five-part AI exercise block**

**When to use AI here.** Use an LLM to render collider and selection DAGs, to recall the d-separation rules and the precise statements of overadjustment bias and selection-as-collider, and to scaffold a synthetic-data simulation that demonstrates the bias — planting a known message effect, a dwell/reaction collider, and a consent rule, then showing the biased versus honest estimates against ground truth. It is also useful for generating the structural-position checklist for a feature set.

**When NOT to use AI here.** Do not let an LLM pick your control set by predictive importance or model fit. Feature-importance reasoning is exactly the wrong criterion and will recommend including dwell and reaction confidently. Do not accept a discovery result from a model that ran PC or GES on this data without flagging the causal-sufficiency violation. And do not let it treat opt-out as ignorable missingness — the selection structure is a domain fact the model will miss.

**LLM exercise (copy-paste prompt):**
> "I am estimating the total effect of a sales message on prescribing (NRx). Here is my candidate feature set: [paste features — e.g., baseline NRx, specialty, territory, rep tenure, slide dwell time, reaction score, samples left]. For each feature, classify it as upstream or downstream of the message, label it as confounder / mediator / post-treatment collider / instrument / neither, and give an INCLUDE or EXCLUDE verdict for the total effect — reasoning strictly by structural position, never by predictive power or model fit. Then explain, structurally, why conditioning on a post-treatment collider opens a spurious treatment–outcome path. Do not recommend any variable on the grounds that it improves fit."

**CLI exercise.** In your repo, ask Claude Code to write a simulation that generates synthetic rep-visit data with: (a) a known message-to-NRx effect, (b) a dwell/reaction collider driven by both the message and a latent physician trait, and (c) a consent rule that depends on both engagement and prescribing propensity. Have the script print four numbers: the true effect, the estimate with collider controls, the estimate without them, and the consenting-subsample-only estimate. The simulation makes the collider bias and the selection bias visible against ground truth.

**AI validation.** Validate against the synthetic ground truth: the honest estimate — no post-treatment controls, full sample — should land near the planted effect; the collider-controlled and consenting-only estimates should visibly diverge. If an AI-generated pipeline produces its tightest estimate from the collider-controlled specification and presents it as the best model, the trap has reproduced itself. Flag it.

## AI Use Disclosure

*Write two sentences naming what an AI tool did in your work for this chapter and the one judgment it could not make. For example: "I used an LLM to render the DAGs and scaffold the synthetic simulation; I decided myself that dwell time is a post-treatment collider rather than a pre-treatment engagement proxy, because that classification requires knowing the temporal order of measurement relative to message delivery — a domain fact about how CLM telemetry is recorded that the model does not have access to."*

## What Would Change My Mind

I would soften the dwell/reaction verdict if a credible causal model showed that, in a specific dataset, dwell time is not on any open back-door-opening path — for instance, if it were purely a noisy measurement of pre-visit interest assigned before the message, rather than a consequence of the message, making it a pre-treatment proxy rather than a post-treatment collider. I would soften the consent-collider alarm if opt-out turned out to be as-good-as-random with respect to both engagement and prescribing — driven entirely by an institutional policy uncorrelated with physician behavior — in which case the consenting subsample would be a fair sample and the selection bias would vanish. Both are empirical questions; the chapter's position is that the burden is on the analyst to demonstrate these benign conditions, not to assume them.

## Still Puzzling

How much does the consent collider actually bias real estimates? The chapter argues the structure is present and demonstrates the mechanism on synthetic data, but the magnitude in real CRM data is unknown and, by the nature of the problem, hard to measure directly — you cannot observe the prescribing of physicians whose data was never logged. Bounding the bias without the missing data is genuinely unresolved. A related puzzle: FCI gives an honest PAG, but a coarse one. How much of the edge orientation can the rep interviews realistically supply versus how much will remain permanently undetermined — and therefore how much of the final model rests on elicited judgment rather than data? The answer is not yet known, and it is the question that makes the next two chapters necessary.

---

## Exercises

**Warm-up**

1. *(Recall — low difficulty) What this tests: whether you can identify a collider by its structural position.* For each of the following: baseline prescribing volume, slide dwell time, physician specialty, and per-slide reaction score — state whether it is upstream or downstream of the message, and whether you would include it when estimating the *total* message effect on prescribing. Justify each decision in one sentence using structural position, not predictive power.

2. *(Recall — low difficulty) What this tests: whether you can state the one inversion in d-separation.* Explain in your own words why conditioning on a confounder helps (closes a spurious path) while conditioning on a collider hurts (opens a spurious path). Use the flu-and-food-poisoning example to make the collider case concrete, then name the analogous structure in the dwell/reaction case.

3. *(Recall — low difficulty) What this tests: whether you understand Berkson's paradox and selection as a collider.* Explain Berkson's original observation in one sentence. Then state, in one sentence each: (a) the general claim from Hernán et al. (2004) that unifies selection bias and collider bias, and (b) why analyzing the consenting physician subsample is an instance of that structure.

**Application**

4. *(Apply — medium difficulty) What this tests: demonstrating the collider trap on synthetic data.* On the synthetic dataset with planted ground truth, estimate the total message effect twice — once without engagement controls and once with dwell time and reaction score added. Report: the planted true effect, the estimate without controls and its standard error, the estimate with controls and its standard error. Explain why the controlled version has the smaller standard error and why that makes it the trap rather than the improvement.

5. *(Apply — medium difficulty) What this tests: building the collider audit.* Run a collider audit on a candidate feature set for your thread's dataset. Produce a table with one row per candidate feature and these columns: feature name, structural position (upstream / downstream / the treatment / the outcome), role (confounder / mediator / post-treatment collider / instrument / neither), verdict for the total effect (INCLUDE or EXCLUDE with one-line reasoning by position), and — for excluded features — a sentence naming the false intuition that would tempt an analyst to include it. An audit that justifies any inclusion by improved fit is incomplete.

6. *(Apply — medium difficulty) What this tests: placing consent in the causal graph.* Draw a small DAG placing physician consent as a selection node S. Label at least one treatment-side cause of S and at least one outcome-side cause of S with plausible variable names from the rep-visit context. Then: (a) state in one sentence why estimating the message effect on the consenting subsample is conditioning on S, (b) state what assumption would need to hold for the consenting-subsample estimate to validly generalize to the full prescriber population, and (c) state why that assumption is unlikely to hold in practice.

**Synthesis**

7. *(Synthesis — high difficulty) What this tests: justifying FCI over PC and GES.* Explain why PC and GES are invalid for this dataset — name the assumption they require and state why it fails here. Then explain what FCI produces instead (a PAG), what the uncertainty marks in a PAG mean, and why a PAG with many undetermined edges is the correct, honest output rather than a failure. Finally, name the kind of information that would be needed to orient the undetermined edges, and explain where that information comes from in this research program.

8. *(Synthesis — high difficulty) What this tests: connecting the collider trap to the ethics of generalization.* A targeting model is trained on consenting physicians using a collider-controlled specification (dwell and reaction included as engagement controls). The model is deployed across the full prescriber population. Write a two-paragraph analysis of the dual failure: (a) what the collider-controlled specification did to the effect estimate for consenting physicians, and (b) what the selection collider does to the validity of that estimate when the model generalizes to opt-out physicians. Conclude with one sentence on the patient-welfare implication if detailing allocations are made using this model.

**Challenge**

9. *(Challenge — open-ended) What this tests: bounding the selection bias without the missing data.* The consent collider introduces a bias whose magnitude cannot be directly measured — you cannot observe the prescribing of physicians who never consented. Design a study using observable data that would allow you to *bound* the bias rather than measure it directly. Specify: what observable quantities you would use, what assumptions are required, what the bound tells you (and what it does not), and what observable signal — a shift in the opt-out rate, a policy change at a health system — would tell you that the bias structure has changed and the model needs to be re-estimated.
