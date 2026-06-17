# Chapter 6 Notes — The Collider Trap (and the Consent Collider)

*Research-gathering notes for Week 6 (Act II). Post-treatment colliders (slide dwell `Duration_vod`,
reaction scores) — conditioning on them biases the message effect; Berkson's paradox; selection bias;
the **consent architecture** as a selection collider (`CLM_EXPLICIT_OPT_IN_vod` / opt-out
missingness); FCI vs PC/GES for latent-variable-aware discovery. Notes only — not chapter prose.*

Primary source: `pantry/rep-visit-as-causal-dataset_source.md` ("Collider warning" + "Consent
architecture caveat"). TIKTOC Week 6 entry.

---

## A. Foundations

**Collider — structural definition.** A *collider* on a path is a node with two incoming arrows
(A → C ← B). Unconditioned, a collider **blocks** the path: A and B are marginally independent through
it. **Conditioning on the collider — or on any descendant of it — opens the path**, inducing a
*spurious* (non-causal) association between its parents. This is d-separation's one inversion: for
chains and forks, conditioning on the middle node *removes* dependence; for a collider it *creates* it.
(Pearl 2009; Greenland, Pearl & Robins 1999; worked illustration Cole et al. 2010.)

**Post-treatment / overadjustment bias.** Adjusting for a variable *affected by treatment* — a mediator
or a post-treatment collider — biases the estimate of the **total** treatment effect, even in an RCT.
Randomization protects the treatment node, not anything measured downstream of it. (Rosenbaum 1984;
Schisterman, Cole & Platt 2009; Montgomery, Nyhan & Torres 2018 — the last explicitly: dropping or
*subsetting* on post-treatment criteria is as damaging as covariate control.)

**The pharma instance (the chapter's core claim).** `Duration_vod` (slide dwell), `Reaction_vod`
scores, and in-visit signals **look like confounders** an analyst would "control for engagement" with.
They are **post-treatment colliders**: common effects of the rep's call (treatment) *and* of
physician-level traits that also drive prescribing. Conditioning on them to estimate the total
message→NRx effect opens a spurious treatment–outcome path — a *structural* error, not a noise problem.
This is the chapter's misconception #2 from the TIKTOC learner profile.

**Common misconception (load-bearing).**
> "Slide dwell time / reaction score is a useful feature — control for engagement to clean up the
> estimate."
**Wrong, structurally.** Engagement is *downstream* of the message. Putting it in the model is textbook
overadjustment (Schisterman et al. 2009) and, because it's a collider, manufactures bias rather than
reducing it (Montgomery et al. 2018 is the on-point warning). The fix is *not* a better engagement
control — it's *not conditioning on post-treatment variables* when the target is the total effect.

**Berkson's paradox / selection as a collider.** Within a *restricted* (selected) sample, two variables
independent in the population can appear *negatively* (or spuriously) associated, because sample
membership is a collider — a common effect of both. (Berkson 1946, original hospital-data demonstration;
Griffith et al. 2020 *Nature Communications*, the COVID flagship: non-representative samples condition
on a sampling collider and distort risk-factor associations.) **Selection bias is not separate from
collider bias** — it is the same structure (Hernán, Hernández-Díaz & Robins 2004, "A structural
approach to selection bias"): selecting on S=1, where S is a common effect of (causes of) exposure and
(causes of) outcome, opens a spurious exposure–outcome path.

**The consent collider (the chapter's distinctive contribution).** Veeva `CLM_EXPLICIT_OPT_IN_vod`
governs logging; opt-out either suppresses clickstream or anonymizes (`CLM_OPT_OUT_BEHAVIOR_vod` 0/1,
strips NPI from `Multichannel_Activity_vod`). If consent depends on both treatment-related factors
(engaged reps, high-contact physicians) *and* outcome-related factors (prescribing propensity,
industry-skepticism, institutional limits), then **sample membership = consent is a selection
collider**. Analyzing only the consenting subsample = conditioning on that collider = selection bias by
Hernán et al.'s definition. Opt-out missingness is almost certainly correlated with physician
characteristics — a sampling-frame collider most analyses silently ignore (TIKTOC Week 6 research
finding; Adoption Risk #5).

**Worked example (on this dataset — process incl. dead end → lesson → limit).**
1. Goal: total effect of message variant on NRx.
2. **Dead end:** an analyst adds `Duration_vod` + `Reaction_vod` "to control for how engaged the
   physician was." The point estimate moves and tightens — *looks* better. It is *confidently wrong*:
   the collider path is now open. (Opening case in TIKTOC: "an analysis that 'controls for engagement'
   and gets a confidently wrong message effect.")
3. **Lesson:** identify variables by *structural position*, not by whether they "improve fit."
   Post-treatment → leave out for the total effect. (If you *want* the educational pathway, that's
   mediation — Ch 5 — and a different, assumption-heavy analysis, not a covariate.)
4. **The consent layer:** even with colliders handled, the *sample* is selected on consent. Put the
   consent mechanism *in the graph*; recognize you can only ever estimate effects on the consenting
   subsample and that this generalizes wrong (Ch 13 opening: "a model that works on the consenting
   subsample and silently generalizes wrong").
5. **Limit / tool:** because the data plausibly has *both* latent confounders (unobserved physician
   propensity) *and* a selection collider (consent), standard causal-discovery (PC, GES) is invalid —
   they assume *causal sufficiency*. Use **FCI** (latent-aware), which outputs a **PAG** that leaves
   honest uncertainty marks where direction can't be pinned down. Bridge: "even with colliders handled,
   the data can't orient every edge" → Ch 7.

---

## B. Examples + failure case

- **Collider illustrations to borrow** (Cole et al. 2010): flu/egg-salad → fever; genotype/environment
  → case status — two independent causes made spuriously associated by conditioning on the common
  effect. Clean templates to re-skin with dwell/reaction.
- **Berkson modern example:** Griffith et al. (2020) COVID — collider bias from hospitalized/tested/
  volunteer samples. A vivid, recent, non-pharma analogue for the consent-sample problem.
- **Failure case (consent-specific, the chapter's punch).** A targeting model is fit on consenting
  physicians only. Privacy-conscious / industry-skeptical physicians opt out *and* prescribe
  differently. The model's message→NRx estimate is biased (selection collider), and when deployed
  across the full prescriber population it generalizes wrong — silently, because the bias lives in the
  *sampling frame*, not in any visible covariate. Ignoring opt-out missingness = conditioning on a
  collider (TIKTOC Part 11: "Consent missingness is ignorable — False").

---

## C. Connections

- **← Ch 5 (mediation):** the deliberate contrast — Ch 5 *uses* the mediator (for the pathway split);
  Ch 6 says *do not condition* on post-treatment variables (for the total effect). Same nodes, opposite
  goal. Make Fellows hold both.
- **← Ch 4 (IV):** exclusion-restriction threats in Ch 4 were *confounding of the instrument*; colliders
  here are a *different* error (conditioning, not omission). Contrast the two so "control for it"
  instincts get structurally disciplined.
- **→ Ch 7 (Markov equivalence):** v-structures (colliders) are *exactly* what distinguishes equivalence
  classes — the collider is the one three-node structure that is identifiable. Ch 6's collider concept
  is the bridge into Ch 7's equivalence machinery.
- **→ Ch 11 (Project 3, DML on clickstream):** "specify the SCM before the DML residualization or it
  corrupts the estimate" — the collider discipline in action; reaction/dwell as post-treatment
  colliders must be excluded from the DML treatment/control set.
- **→ Ch 13 (consent + FCI):** the consent collider is *revisited* at the handoff; FCI + the
  educational-pathway regulatory line + drift monitoring on opt-out-rate shifts.
- **→ Living Model framework:** the drift monitor watches opt-out-rate shifts that change the bias
  structure over time (`living-models.md` drift material).

---

## D. Current state / key refs / last-3-yr

**Colliders / DAGs:**
- Pearl (2009), *Causality: Models, Reasoning, and Inference*, 2nd ed., Cambridge UP (d-separation;
  collider rule). [verify edition/pages]
- Greenland, Pearl & Robins (1999), "Causal Diagrams for Epidemiologic Research," *Epidemiology*
  10(1):37–48.
- Cole et al. (2010), "Illustrating bias due to conditioning on a collider," *Int. J. Epidemiology*
  39(2):417–420.

**Post-treatment / overadjustment:**
- Rosenbaum (1984), "Consequences of adjustment for a concomitant variable affected by the treatment,"
  *JRSS-A* 147(5):656–666.
- Schisterman, Cole & Platt (2009), "Overadjustment Bias and Unnecessary Adjustment…," *Epidemiology*
  20(4):488–495.
- **Montgomery, Nyhan & Torres (2018), "How Conditioning on Posttreatment Variables Can Ruin Your
  Experiment…," *AJPS* 62(3):760–775** — the directly on-point, recent warning.

**Berkson / selection:**
- Berkson (1946), "Limitations of the Application of Fourfold Table Analysis to Hospital Data,"
  *Biometrics Bulletin* 2(3):47–53.
- **Griffith et al. (2020), "Collider bias undermines our understanding of COVID-19…," *Nature
  Communications* 11:5749** — flagship recent example.
- Hernán, Hernández-Díaz & Robins (2004), "A Structural Approach to Selection Bias," *Epidemiology*
  15(5):615–625.
- Elwert & Winship (2014), "Endogenous Selection Bias: The Problem of Conditioning on a Collider
  Variable," *Annual Review of Sociology* 40:31–53 — the best single review unifying collider /
  selection / overadjustment bias; ideal core reading for this chapter. [verify pages]
- (Missing-data-as-selection extensions, if the chapter goes deeper: Saadati & Tian, arXiv:1907.01654;
  Nguyen et al. arXiv:1609.00606 — flagged from the prior stub, lower priority.)

**Causal discovery with latents (FCI vs PC/GES):**
- Spirtes, Glymour & Scheines (2000), *Causation, Prediction, and Search*, 2nd ed., MIT Press (PC under
  causal sufficiency; original FCI for latents/selection). [verify edition/pages]
- **Zhang (2008), "On the completeness of orientation rules… in the presence of latent confounders and
  selection bias," *Artificial Intelligence* 172(16–17):1873–1896** — the complete/augmented FCI.
- Chickering (2002), "Optimal Structure Identification With Greedy Search," *JMLR* 3:507–554 (GES;
  optimal *under causal sufficiency* — exactly the assumption that fails here).

---

## E. Teaching

- **This is the intuition-inverting chapter** (TIKTOC "hard chapters"): students' "control for more
  stuff" reflex is the bug. Lead with a case where adding a control makes the answer *worse*.
- **The diagnostic question to drill:** "Is this variable upstream or downstream of treatment?" Position,
  not predictive power, decides inclusion.
- **Two-figure pairing:** (1) the message→engagement←physician-trait collider (why conditioning on
  dwell opens a path); (2) the consent-selection DAG (S = consent as a common effect, analyzing S=1
  opens the path). Same shape, twice — drives the unification home.
- **FCI vs PC/GES as a "right tool" beat:** PC/GES assume away the very things (latent confounding,
  selection) that define this dataset. FCI's PAG makes the uncertainty *honest* (circle marks /
  bidirected edges where direction can't be determined). Pairs with Ch 7's "data identifies a class."
- **Consent/welfare check (per chapter anatomy):** opt-out missingness is the load-bearing ethics item —
  pervasive per Adoption Risk #5, not an appendix.

---

## F. Library files (in `pantry/`)

- `_lib_ai-ml-assisted-adjustment.md` — **newly copied, highly relevant.** A worked conversation on
  causal inference + ML-assisted adjustment + automated graph audit + estimability classification +
  sensitivity-analysis-as-default; touches collider/selection-aware adjustment and the expert-interview
  → graph-audit pipeline that mirrors this book's architecture. Good source for the "specify the SCM
  first / don't condition on post-treatment" discipline.
- `_lib_science-the-book-of-why-the-new-science-of-cause-and-effect.md` — colliders, Berkson's paradox,
  selection bias, causal discovery (Pearl/Mackenzie; the accessible canonical treatment of colliders).
- `_lib_science-calling-bullshit-the-art-of-skepticism-in-a-data-driven-world.md` — Berkson, selection
  bias, collider intuition at the popular level (good for the opening hook).
- `_lib_ai-applied-causal-inference-powered-by-ml-and-ai.md` — collider, selection bias, FCI/Spirtes,
  DML (Ch 11 link); the technical source for "specify the SCM before DML residualization."
- `_lib_math-causal-inference-mit-press-essential-knowledge-series.md` — selection bias, colliders,
  conceptual.
- `_lib_math-weapons-of-math-destruction...md` — selection bias in deployed models (ethics framing for
  the consent collider; not a method source).

---

## G. Gaps / flags

- **[verify]** Pearl (2009) and Spirtes-Glymour-Scheines (2000) exact editions/pages (book cites from
  bibliographic record, not re-checked this pass).
- **Veeva field names** (`CLM_EXPLICIT_OPT_IN_vod`, `CLM_OPT_OUT_BEHAVIOR_vod`,
  `Multichannel_Activity_vod`, `Duration_vod`, `Reaction_vod`): from the source essay; Adoption Risk #6
  flags Veeva-schema velocity — verify each field name/behavior against current Veeva CLM docs before
  print, treat as dated examples.
- **Empirical open question (TIKTOC Week 6 research finding):** that opt-out missingness *is* correlated
  with physician characteristics is stated as "almost certainly" — it is an empirical claim the chapter
  should present as highly plausible/testable, not proven. With synthetic data the book can *demonstrate*
  the bias mechanism with known ground truth.
- **FCI practical caveat:** FCI/PAGs are correct-but-coarse — they often leave many edges
  partially-oriented. Flag that FCI is the *honest* discovery tool here, not a magic edge-orienter; edge
  orientation still needs Ch 7–8's rep knowledge. (Connects the "right tool" beat to the equivalence-
  class limit.)
- The source essay says "use FCI not PC/GES." Make sure the chapter states *why* (causal-sufficiency
  failure = latent confounders + selection), not just *which* — that "why" is the teachable content.
