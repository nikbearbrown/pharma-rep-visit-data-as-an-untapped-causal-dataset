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

![Three side-by-side three-node causal graphs. Left, a fork (A from C to B): conditioning on C removes a spurious association and helps. Middle, a chain (A to C to B): conditioning on C blocks the path, appropriate only if C is a mediator you mean to block. Right, a collider (A to C from B): A and B are independent through C, so conditioning on C opens a spurious path and hurts. A dashed box marks the conditioned node in each panel.](images/06-the-collider-trap-fig-01.png)

*Figure 6.1 — The one inversion: controlling for a collider makes things worse, not better*

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

![Two identical four-node graphs. Message and NRx sit left and right; latent physician openness sits above center with a dashed outline; dwell/reaction sits below center as a common effect of the message and of openness, making it a collider. Left graph: dwell/reaction untouched and the path through openness stays blocked. Right graph: a conditioning box around dwell/reaction opens a dashed spurious path linking message to NRx through openness.](images/06-the-collider-trap-fig-02.png)

*Figure 6.2 — Dwell and reaction are post-treatment colliders: conditioning on them creates the bias, it does not remove it*

<!-- → [DIAGRAM: The dwell/reaction collider in the pharma DAG. Nodes: "Message (D)" on left, "NRx (Y)" on right, "Physician Openness (latent)" above, "Dwell / Reaction" in the center. Arrows: Message → NRx (direct effect), Message → Dwell/Reaction, Physician Openness → Dwell/Reaction, Physician Openness → NRx. Bold annotation: "Dwell/Reaction is a collider. Conditioning on it opens the Message–NRx path through Physician Openness." Second version of the diagram shows the same graph with a conditioning box around Dwell/Reaction, with a dashed spurious path highlighted between Message and NRx. Caption: "Without conditioning: the collider blocks the spurious path. With conditioning: the path opens and the total effect estimate is corrupted."] -->

---

Now comes the subtler trap, the one that does not live in any column of the data.

A collider does not have to be an observed variable. It can be the rule that determined who is in the dataset at all. This is Berkson's paradox, named for the statistician who showed in 1946 that two unrelated diseases appear negatively correlated among hospitalized patients — because having either disease raises the probability of admission, so among admitted patients, lacking one makes the other more likely. Sample membership is itself a collider. Conditioning on it — which is what you do simply by analyzing the sample — opens a spurious association between its causes.

Hernán, Hernández-Díaz, and Robins (2004) unified this under one roof: selection bias is not a separate phenomenon from collider bias. It is the same structure. Selecting on $S = 1$ where $S$ is a common effect of causes of exposure and causes of outcome opens a spurious exposure-outcome path, exactly as conditioning on a collider does in the interior of a graph. Griffith and colleagues demonstrated this concretely in 2020, showing that COVID risk-factor associations were distorted in hospitalized and tested samples because sample membership was a collider conditioned on by the act of sampling.

Now apply this to the one feature that makes this dataset ethically and statistically distinctive.

Veeva's consent mechanism governs whether a physician's CLM activity is logged at all. The `CLM_EXPLICIT_OPT_IN_vod` setting and the account-level `CLM_Opt_Type_vod` field record whether a physician has consented to have their visit interaction captured; `CLM_OPT_OUT_BEHAVIOR_vod` then governs the opt-out case — 0 suppresses the clickstream entirely, 1 writes the activity anonymously to the Multichannel Activity objects without the NPI-bearing association. (Field names verified against Veeva CRM Help as of mid-2026; schema naming is a known aging risk through the CRM-to-Vault migration, so verify against `crmhelp.veeva.com` / `vaultcrmhelp.veeva.com` before relying on them.) The consenting subsample is the only sample available for analysis.

What determines whether a physician consents? Consider the plausible causes. On the treatment side: a physician who has been called on frequently by an engaged rep, or who is embedded in a high-contact territory, may be more likely to be enrolled and to remain opted in. On the outcome side: prescribing propensity, industry-skepticism, privacy consciousness, and institutional policies at health systems that prohibit data-sharing with pharmaceutical companies all plausibly govern opt-out decisions.

If consent is a common effect of treatment-related factors and outcome-related factors, then:

$$\text{engaged rep / high contact} \rightarrow S = \text{consent} \leftarrow \text{industry-skepticism / prescribing propensity}$$

Consent is a selection collider. Analyzing the consenting subsample is conditioning on $S = 1$. By the structure Hernán and colleagues formalized, this opens a spurious message-to-prescribing path. Opt-out missingness is almost certainly correlated with physician characteristics — it is a sampling-frame collider that most analyses silently ignore. [This is a highly plausible, testable claim; the synthetic dataset lets you demonstrate the bias under known ground truth, which shows the mechanism, not the magnitude in any real population.]

![Consent as a selection collider. A treatment-side cause (rep engagement / visit frequency) and an outcome-side cause (industry-skepticism / prescribing propensity) both point into a central consent node, S = 1. Below the node, a label notes the observed sample is only S = 1 physicians. Analyzing this subsample conditions on S, opening a spurious message-to-NRx path exactly as conditioning on any collider does. The bias lives in who is absent, so no diagnostic on the observed data reveals it.](images/06-the-collider-trap-fig-03.png)

*Figure 6.3 — The consent collider: a Berkson/selection collider invisible in the data, demanding FCI rather than PC or GES*

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

---

## Prompts

### Figure 6.1 — The one inversion: fork vs chain vs collider

Produce a single self-contained HTML file (inline CSS, D3 7.9.0 from the cdnjs CDN) rendering a three-panel structural comparison of causal graphs. Chart type: three side-by-side three-node DAGs separated by hairline dividers. Data shape: Panel 1 FORK — a top node C with two arrows down to A and B, labeled "conditioning HELPS, removes a spurious A-B association"; Panel 2 CHAIN — three nodes in a row A→C→B, labeled "conditioning BLOCKS, fine if C is a mediator you mean to block"; Panel 3 COLLIDER — A and B both arrow up into a top node C, labeled "conditioning HURTS, opens a spurious path between A and B". Marks: rectangles for nodes, directed arrows, and a dashed box marking the conditioned node in each panel (red dashed in the collider panel to flag harm). Channels: arrow direction encodes causal role; the red dashed box marks the harmful conditioning. No quantitative axes. Annotations: per-panel verdict labels and a caption noting that in this book the collider is a post-treatment variable (dwell, reaction), so structural position downstream of treatment decides the harm, not predictive power. Brutalist palette, EB Garamond title / Inter body / JetBrains Mono labels. Deliverable: one HTML file.

### Figure 6.2 — Dwell/reaction collider: path closed vs path opened

Produce a single self-contained HTML file (inline CSS, D3 7.9.0 from the cdnjs CDN) rendering a two-state DAG comparison. Chart type: two identical four-node graphs side by side, divided by a hairline. Data shape: each graph has message (left), NRx (right), latent physician openness (top center, dashed-outline node = unobserved), and dwell/reaction (bottom center, a collider — common effect of message and openness). True edges in both: message→NRx, message→dwell/reaction, openness→dwell/reaction, openness→NRx. Left graph: dwell/reaction untouched, path through openness stays blocked. Right graph: a red dashed conditioning box around dwell/reaction plus a red dashed spurious path arcing from message through openness to NRx. Marks: rectangles for observed nodes, dashed ellipse for the latent node, solid arrows for true edges, red dashed for the induced spurious path. Channels: line style distinguishes observed/latent and true/induced; red marks the bias. No quantitative axes. Annotations: a legend (dashed node = unobserved; red dashed = spurious path induced by conditioning) and a caption that dwell and reaction are post-treatment colliders, so adding them creates bias rather than removing it; Veeva field names dated mid-2026, kept out of the figure. Brutalist palette and font chains as above. Deliverable: one HTML file.

### Figure 6.3 — The consent collider

Produce a single self-contained HTML file (inline CSS, D3 7.9.0 from the cdnjs CDN) rendering a selection-collider DAG. Chart type: three-node node-link graph with an induced-association overlay. Data shape: a treatment-side cause (rep engagement / visit frequency, left) and an outcome-side cause (industry-skepticism / prescribing propensity, right) both point into a central consent node S = 1; a label below S states the observed sample is only the consenting physicians. Marks: rectangles for the two causes and the consent node, a red dashed box around the consent node to mark it as conditioned-on by the act of sampling, two solid arrows into S, and a red dashed spurious arc directly between the two causes to show the association induced by selection. Channels: arrow direction encodes causal role; the red dashed arc and box mark the selection-induced bias. No quantitative axes. Annotations: a caption that analyzing the consenting subsample conditions on S — a Berkson/selection collider invisible in the observed data — so causal discovery must use FCI (latent + selection), not PC or GES, and a note that the relationship is inferred and Veeva field names are dated mid-2026. Brutalist palette and font chains as above. Deliverable: one HTML file.

---

## Chapter 6 Exercises: The Collider Trap

**Project:** The Causal Interview Bot
**This chapter adds:** Bot questions that flag collider-prone in-visit signals — dwell time, reaction score — that reps over-weight as "engagement," so the bot tags them DOWNSTREAM-OF-MESSAGE and keeps them out of the adjustment set.

### Exercise 1 — When to Use AI

Reps will tell your bot, unprompted, that they "could feel her leaning in — she stayed on the safety slide forever." That is a dwell-time signal dressed as causal knowledge, and the bot has to catch it. Two tasks where AI earns its keep here:

- **Render the three-panel structural comparison** (fork vs chain vs collider) and the dwell/reaction collider graph from this chapter, so you can eyeball whether a candidate edge sits upstream or downstream of the message. *Why AI works here:* (rendering / recall of a fixed formalism) — d-separation rules are settled mathematics and the output is a picture you can check against Figure 6.1.
- **Generate a structural-position checklist** that, given a candidate feature list, classifies each as upstream/downstream and confounder/mediator/post-treatment collider. *Why AI works here:* (classification against an explicit rule) — the rule is "position decides inclusion," and you can verify each verdict by asking whether the variable is measured before or after message delivery.

**The tell:** you can independently evaluate the output — every classification is checkable against the temporal order of measurement, and every rendered edge against the chapter's graphs. You are not trusting the model's judgment; you are using it as a fast clerk for a rule you already hold.

### Exercise 2 — When NOT to Use AI

- **Do not let the bot — or any LLM — decide whether dwell time is a confounder by looking at how much it improves fit.** *Why AI fails here:* (causal-ID + LLM-suggestibility) — feature-importance reasoning confidently recommends including dwell and reaction, because they correlate with the outcome; that correlation is the bias, and the model has no access to the temporal-order fact that settles it.
- **Do not let the model rule the consent collider "ignorable missingness."** *Why AI fails here:* (ground truth + values) — whether opt-out is as-good-as-random is a domain fact about who declines to be logged, and treating it as random is a values-laden choice about which physicians the model is allowed to silently assume away.

**The tell:** AI as reason vs tool — if the model's fit statistic is the *reason* an edge goes into the adjustment set, you have outsourced the one judgment the chapter forbids outsourcing. AI is a tool for rendering and bookkeeping; the structural-position verdict is yours.

**Series connection:** tier **T5 (Causal)** — classifying a variable's structural position relative to treatment is irreducible causal-identification work that no amount of data or model fit can supply, plus **T7 (Compliance/Values)** for the consent-ignorability call, which decides whose data the model is permitted to extrapolate over.

### Exercise 3 — LLM Exercise

**What you're building:** The collider-screening layer of the bot — the rule that intercepts in-visit signals a rep volunteers as "engagement" and tags them downstream-of-message before they can poison an adjustment set.

**Tool:** Claude, run as a **Claude Project** so the bot spec (evidence gate, downstream-tagging rule, the firewall that fit never decides inclusion) lives as persistent context across every transcript you feed it — you do not want to re-paste the discipline each time, because a fresh chat will happily reach for fit.

**The Prompt:**

```
You are the collider-screening layer of an expert-elicitation bot for
pharmaceutical rep-visit data. A rep has just said this in an interview:

"Dr. Patel is great — every time I bring the safety data she stays on that
slide for a good minute, you can tell she's really chewing on it, and her
reaction score is always positive. That's how I know the message is landing,
and honestly that's why her scripts are up this quarter."

Your target estimand is the TOTAL effect of the message variant on
prescribing (NRx).

Do the following, and nothing else:
1. List every variable the rep mentioned.
2. For each, state whether it is measured BEFORE or AFTER the message is
   delivered, and therefore whether it is upstream or downstream of the
   message.
3. Classify each as confounder / mediator / post-treatment collider /
   neither, reasoning ONLY from structural position — never from how well it
   would predict NRx or improve model fit.
4. Give an INCLUDE or EXCLUDE verdict for the total-effect adjustment set.
5. For every EXCLUDE on an in-visit signal, write the exact one-sentence
   note the bot should attach to that edge, beginning:
   "DOWNSTREAM-OF-MESSAGE — do not adjust:"
6. Explain, in three sentences, why conditioning on dwell time / reaction
   score OPENS a spurious message-to-NRx path through latent physician
   openness, rather than closing one.

Do NOT recommend any variable on the grounds that it improves fit or
predicts the outcome. If you are tempted to, write "FIT IS NOT A REASON"
instead.
```

**What this produces:** A screened edge list in which `Duration_vod`-style dwell and `Reaction_vod`-style reaction are tagged DOWNSTREAM-OF-MESSAGE and excluded, with the structural reason attached — the exact annotation the bot will carry into the prior DAG.

**How to adapt:** swap the rep quote for a transcript line from your own synthetic dataset; on **ChatGPT or Gemini**, paste the bot-spec discipline at the top of the prompt since there is no persistent Project context; in a **Claude Project**, store the discipline once as project instructions and feed only the quote.

**Connection to previous chapters:** This operationalizes Chapter 6's "position decides inclusion" and reuses the causal-role vocabulary (confounder/mediator/collider) from the graph chapters.

**Preview of next chapter:** The signals you just kept *out* of the adjustment set are not useless — some of them sit on edges the data cannot orient. Chapter 7 shows why even a correctly screened graph leaves arrows undecided, and the bot's job grows from screening to orienting.

### Exercise 4 — CLI Exercise

**What you're building:** A synthetic-data harness that makes the collider trap visible against ground truth, so the bot's downstream-tagging rule has a numeric justification.

**Tool:** Claude Code (or Cowork) — because this is a multi-file scripting task (data generator + estimator + report) that benefits from iterating on a real repo rather than copy-pasting snippets. **Skill level:** intermediate (comfortable running Python and reading a regression coefficient).

**Setup:**
- [ ] A scratch repo with Python 3.11+, `numpy`, `pandas`, `statsmodels`.
- [ ] A `data/` folder for synthetic output only — no real CRM data anywhere near this.
- [ ] A `CLAUDE.md` stub stating "synthetic-only; never read or write real prescriber data."

**The Task:**

```
Work only inside this repo's data/ and src/ folders. Do not touch anything
else. Generate SYNTHETIC rep-visit data only — no real data exists here.

Write src/collider_sim.py that:
1. Generates N=5000 synthetic physicians with a LATENT openness trait.
2. Plants a KNOWN total message-to-NRx effect (set TRUE_EFFECT = 4.0).
3. Builds dwell time and reaction score as a COLLIDER: each is a function
   of BOTH the message AND latent openness (openness also drives NRx).
4. Builds a consent flag that depends on both rep engagement and prescribing
   propensity, then exposes a consenting-only subsample.
5. Estimates the total message effect FOUR ways and prints exactly four
   labeled numbers: (a) true effect, (b) estimate with dwell+reaction as
   controls, (c) estimate without them (full sample), (d) consenting-only
   estimate.

Stopping condition: the script runs end to end and prints the four numbers.
Verification step: assert that (c) lands within 0.3 of TRUE_EFFECT and that
(b) is BOTH further from the truth AND has a smaller standard error than (c);
print "COLLIDER TRAP REPRODUCED" if so.
```

**Expected output:** Four numbers where the honest, no-controls, full-sample estimate (c) sits near 4.0, while the collider-controlled estimate (b) is biased *and* tighter — and the consenting-only estimate (d) drifts on top of that.

**What to inspect:** Confirm (b) has the smaller standard error. That "more wrong, more confident" combination is the trap; if (b) were merely noisier you would not have reproduced it.

**If it goes wrong:** If (b) lands near the truth, the collider is too weak — increase openness's loading on dwell/reaction and on NRx, regenerate, and re-check. (Recovery: the planted effect is known, so you can always dial the latent path until the bias appears.)

**CLAUDE.md note:** Add "Synthetic-only repo. Estimator inclusion is decided by structural position, never by fit or standard error. Dwell/reaction are post-treatment colliders — never controls for the total effect."

### Exercise 5 — AI Validation Exercise

**What you're validating:** The output of Exercise 3 (the bot's screened edge list) or Exercise 4 (the four-number simulation) — specifically, whether the collider discipline actually held.

**Validation type:** Structural-correctness audit against synthetic ground truth. **Risk level:** **High** — a collider silently conditioned-on produces a confident, well-fit, wrong estimate that downstream targeting will act on, and the bias is invisible in the observed data.

**Setup:** Use your Ex3 edge list or Ex4 output. If you want a pre-flawed artifact to practice on, take the Ex3 prompt and feed it a version of the rep quote, then deliberately keep an edge the model labeled "engagement → NRx, INCLUDE (improves fit)" — that is the hallucinated collider edge you are hunting.

**The Validation Task:**

```
Validation Checklist — Chapter 6 (The Collider Trap)

For the artifact below, mark each item PASS / FAIL / CANNOT-DETERMINE
with a one-line reason:

1. Correctness — Is the total-effect estimand stated, and are all verdicts
   reasoned from structural position rather than fit?
2. Completeness — Is every in-visit signal (dwell, reaction) accounted for
   and explicitly tagged downstream-of-message?
3. Scope — Does the artifact stay on model construction / screening, and
   avoid producing a deployable susceptibility or targeting score?
4. Chapter-specific: Consent layer — Is the consenting subsample flagged as
   conditioning on a selection collider, with the generalization assumption
   named?
5. Chapter-specific: Discovery tool — If discovery is mentioned, is FCI
   chosen over PC/GES, with the causal-sufficiency violation named?
6. Failure-mode check — Scan for the fluent-but-wrong tell: any variable
   admitted to the adjustment set because it "improves fit," "is predictive,"
   or "tightens the estimate." A collider conditioned-on for fit reasons is
   an automatic FAIL. Also flag any claim of correctness that lacks a
   ground-truth comparison.

Artifact:
<paste Ex3 edge list or Ex4 output here>
```

**What to do with findings:** All pass — the screening held; proceed to Chapter 7. One fail — fix that edge and re-run the checklist. Multiple fails — the discipline did not hold; rebuild the screening prompt with the firewall language and treat the artifact as untrustworthy.

**AI Use Disclosure prompt (mandatory):** *Write two sentences naming what an AI tool did in this exercise and the one judgment it could not make. For example: "I used Claude to classify candidate features and render the collider graph; I decided myself that dwell time is a post-treatment collider rather than a pre-visit engagement proxy, because that turns on the temporal order of CLM measurement relative to message delivery — a domain fact the model does not have."*

**Series connection:** The signature failure mode is a **collider conditioned-on for fit reasons** — fluent, well-fit, and wrong with no ground-truth check. This is a **T5 (Causal)** validation task: only structural knowledge, not data or model fit, can catch it.
