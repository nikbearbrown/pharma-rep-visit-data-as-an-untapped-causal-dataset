# Chapter 5 Notes — The Causal Graph and the Three Pathways

*Research-gathering notes for Week 5 (Act II opener). Mediation: educational (M1, legally drivable)
vs relationship-maintenance (M2: samples/coupons) vs reciprocity (M3: meals/$40B); direct vs indirect
vs total effect; why conflating pathways misleads investment + ethics. Notes only — not chapter prose.*

Primary source: `pantry/rep-visit-as-causal-dataset_source.md` ("The data as a Living Model problem":
three-pathway mediation). TIKTOC Week 5 entry.

---

## A. Foundations

**The graph (from the source essay).** Training cohort (Z) → message variant (D) → and then three
mediating pathways to NRx (Y):
- **M1 — educational:** D → knowledge/attitude → Y. The physician learns something; prescribing
  changes because the clinical case changed. **The only legally drivable pathway.**
- **M2 — relationship-maintenance:** D → samples, co-pay/coupon cards, follow-up → Y.
- **M3 — reciprocity:** Open Payments meals/gifts → Y. The "$40B promotional" pathway — social
  obligation, not new information.

This is a **three-pathway (multiple-mediator) mediation** problem. Each pathway has a *different
ethical/regulatory status*, so the *total* effect (which IV in Ch 4 identified) is not enough — you
need the *decomposition*. Rung-1 NBA cannot separate the pathways; a Living Model on the parameterized
graph can estimate each pathway's contribution. The chapter's opening case: two visits with *identical*
NRx lift — one driven through education, one through a meal — and why that difference is everything.

**The vocabulary the chapter teaches (counterfactual mediation, not Baron-Kenny).**
- **Total Effect (TE):** Y(1, M(1)) − Y(0, M(0)). What the IV estimate targets.
- **Natural Direct Effect (NDE):** Y(1, M(0)) − Y(0, M(0)) — effect of the message holding the mediator
  at its *no-message* value.
- **Natural Indirect Effect (NIE):** Y(1, M(1)) − Y(1, M(0)) — effect of moving the mediator to its
  message-induced value. **TE = NDE + NIE** (clean additive split).
- **Controlled Direct Effect (CDE):** Y(1, m) − Y(0, m) — message effect when the mediator is *fixed by
  intervention* at m. Policy-relevant ("what if we blocked samples?"); does *not* generally sum to TE.

**Common misconception (load-bearing).**
> "Run a Baron–Kenny mediation: regress NRx on the message, add the mediator, see how much the message
> coefficient shrinks — that shrinkage is the pathway."
**Wrong / dangerous here.** Baron & Kenny (1986) is product-of-coefficients on regression coefficients:
it assumes no exposure-mediator interaction and no nonlinearity, **conflates statistical with causal
mediation**, and hides a "no unmeasured mediator–outcome confounding" assumption it never states. A
shrinking coefficient is a statistical event, not proof the effect flows through M. With a binary/count
outcome (NRx) and plausible interactions, the *a·b* split is biased. The counterfactual framework
(Robins–Greenland 1992; Pearl 2001; VanderWeele 2015/2016) tells you what NDE/NIE/CDE actually *mean*
and how demanding their assumptions are.

**Why mediation is harder than the total effect (the key caveat).** Identifying NDE/NIE requires
**four** no-unmeasured-confounding assumptions (all conditional on covariates): (1) exposure–outcome,
(2) mediator–outcome, (3) exposure–mediator, and (4) **no mediator–outcome confounder itself affected
by the exposure** — the **cross-world** assumption, which has *no analogue* in total-effect estimation
and is untestable even in an RCT. Randomizing the message secures (1) and (3) — enough for the *total*
effect — but does nothing for (2) and (4), because the mediator is never randomized. Robins & Greenland
(1992) proved direct/indirect effects are *not* separately identifiable from exposure randomization
alone. **Upshot for the book:** the IV/RCT total message effect is far more credible than any single
pathway share. Say so.

**Worked example (on this dataset — process incl. dead end → lesson → limit).**
1. Draw the graph: D (message) → M1 (educational attitude, proxied by rep-noted objection resolution /
   clinical questions) ; D → M2 (samples/coupons left, from rep-entered fields) ; M3 (Open Payments
   meals) → Y (NRx). Mark which arrows you actually have data for.
2. **Dead end to walk through:** estimate the "education pathway" by Baron–Kenny, controlling for
   samples and meals. Because the three mediators plausibly affect each other and share unobserved
   common causes (a physician's general openness drives engagement *and* meal acceptance *and*
   prescribing), one mediator is a *post-exposure confounder* for the others — the natural three-way
   split is **not point-identified**. **Lesson:** you can identify the *joint* indirect effect through
   {M1,M2,M3} and the direct effect not-through-any, but not a clean "share through education."
3. **The defensible tool:** **interventional (randomized-interventional) (in)direct effects**
   (VanderWeele, Vansteelandt & Robins 2014), generalized to multiple mediators with an *exact*
   decomposition even under unknown ordering / between-mediator confounding (Vansteelandt & Daniel
   2017). Trade-off: they don't require the cross-world assumption but carry a "stochastic intervention"
   interpretation and (for the 2014 version) don't sum exactly to TE.
4. **Limit:** even the interventional split is assumption-heavy; the chapter's honest claim is "we can
   bound/estimate the education vs non-education contribution under stated assumptions," which is enough
   to drive the investment-and-ethics argument. Bridge: "but some 'features' you'd condition on are
   traps" → Ch 6 (colliders).

---

## B. Examples + failure case

- **The meals-and-prescribing evidence (motivating example).** DeJong et al. (2016, *JAMA Internal
  Medicine* 176(8):1114–1122): linking Open Payments meals (Aug–Dec 2013) to Medicare Part D
  prescribing across ~279,669 physicians and four drug classes. A *single* sponsored meal (often <$20)
  was associated with higher promoted-brand prescribing — rosuvastatin OR ≈1.18; nebivolol OR ≈1.70 —
  rising with more/larger meals. **Crucial caveat the authors state:** association, *not* cause —
  confounding by indication/preference and reverse causation (reps target high-volume prescribers).
  This is exactly the gap the mediation framework addresses, and exactly why M3 (reciprocity) is
  "association-strong, causal-contested" (TIKTOC Part 11).
- **Failure case (the chapter's commercial/ethical sting).** A brand sees identical NRx lift from two
  tactics and, treating the *total* effect as the decision variable, doubles the budget on whichever is
  cheaper per contact — say, meals. If that lift ran through M3 (reciprocity) not M1 (education), the
  brand has (a) bought a legally/ethically exposed pathway and (b) mis-attributed it to "the message
  working." Conflating pathways misleads *both* investment and compliance. A Living Model that
  estimates each pathway's contribution makes the difference visible (TIKTOC Week 5 research finding).

---

## C. Connections

- **← Ch 4 (IV):** Ch 4 delivers the *total* message effect; this chapter is the decomposition of that
  same effect. Total → mediated split is the narrative hinge between Act I and Act II.
- **→ Ch 6 (colliders):** the mediators here (M1/M2) sit *downstream* of treatment — conditioning on a
  *post-treatment* variable to estimate the *total* effect is overadjustment. Ch 5 (you *want* the
  mediator, for the split) vs Ch 6 (you must *not* condition on dwell/reaction, for the total) is a
  deliberate, teachable contrast — same structural fact, opposite analytic goal.
- **→ Ch 11 (Project 2, meals causal forest):** M3 separation (reciprocity vs education) is operationalized
  as heterogeneous meal effects by specialty/baseline — the public-data-runnable project (Open Payments).
- **→ Ch 13 (regulatory line):** the educational-vs-reciprocity boundary is *the* regulatory boundary;
  M1 legally drivable, M3 the $40B exposure. Pathways = ethics, revisited at the handoff.
- **→ Living Model framework:** "estimate each pathway's direct contribution" is a parameterized-SCM
  capability (`living-models.md`), not a regression output.

---

## D. Current state / key refs / last-3-yr

**Classic baseline (teach then critique):**
- Baron & Kenny (1986), "The Moderator–Mediator Variable Distinction…," *J. Personality & Social
  Psych.* 51(6):1173–1182. DOI 10.1037/0022-3514.51.6.1173.

**Counterfactual mediation (the framework the chapter adopts):**
- Robins & Greenland (1992), "Identifiability and Exchangeability for Direct and Indirect Effects,"
  *Epidemiology* 3(2):143–155.
- Pearl (2001), "Direct and Indirect Effects," *Proc. UAI 17*, 411–420 (the Mediation Formula).
- VanderWeele (2015), *Explanation in Causal Inference: Methods for Mediation and Interaction*, Oxford UP.
- **VanderWeele (2016), "Mediation Analysis: A Practitioner's Guide," *Annual Review of Public Health*
  37:17–32** — best single citable overview (the four assumptions, interaction, binary outcomes,
  sensitivity analysis).

**Multiple mediators / interventional effects (the technically correct tool for three pathways):**
- VanderWeele, Vansteelandt & Robins (2014), "Effect Decomposition in the Presence of an
  Exposure-Induced Mediator-Outcome Confounder," *Epidemiology* 25(2):300–306 [verify pages]
  (introduces interventional/randomized-interventional effects).
- VanderWeele & Vansteelandt (2014), "Mediation Analysis with Multiple Mediators," *Epidemiologic
  Methods* 2(1):95–115 [verify vol/pages].
- **Vansteelandt & Daniel (2017), "Interventional Effects for Mediation Analysis with Multiple
  Mediators," *Epidemiology* 28(2):258–265** — exact decomposition under unknown ordering /
  between-mediator confounding. *The directly applicable result for M1/M2/M3.*
- Daniel et al. (2015), "Causal Mediation Analysis with Multiple Mediators," *Biometrics* 71(1):1–14.

**Empirical detailing/meals:**
- DeJong et al. (2016), *JAMA Internal Medicine* 176(8):1114–1122.
- Newham & Valente (2022), study on pharmaceutical detailing/promotion and prescribing,
  arXiv:2203.01778 [verify exact title/venue] — more recent econometric treatment of detailing effects;
  useful as a complement to DeJong for the meals/detailing evidence base.

**Promotional spend (for the "$40B" claim — flag):**
- Schwartz & Woloshin (2019), *JAMA* 321(1): U.S. medical marketing rose $17.7B (1997) → $29.9B (2016);
  ~$5.3B detailing + ~$16.4B free samples (≈$20B+ physician-directed). [verify exact cite]

---

## E. Teaching

- **Open on the two-identical-lifts case** (source essay) — it makes "total effect is not enough"
  visceral before any equation.
- **Teach Baron–Kenny, then break it** — same pedagogical pattern as Ch 4's F>10. Students arrive
  knowing Baron–Kenny; the lesson is *why it can't answer the pathway question here*.
- **Three-pathway figure:** D fanning into M1/M2/M3 then into Y, each arrow color-coded by regulatory
  status (M1 green/drivable, M2 amber, M3 red/$40B). Tie color to Ch 13.
- **The honesty beat:** state plainly that the *natural* three-way split is likely unidentified and that
  **interventional effects (Vansteelandt & Daniel 2017)** are the defensible alternative — at the cost
  of a stochastic-intervention interpretation. Modeling intellectual honesty is the point.
- **Evidence check (per chapter anatomy):** meals→Rx is association-strong/causal-contested — classify
  it as such; do not let the DeJong ORs read as causal.

---

## F. Library files (in `pantry/`)

- `_lib_ai-applied-causal-inference-powered-by-ml-and-ai.md` — **mediation chapter present** (also
  VanderWeele, do-calculus, DiD). Primary technical source for the decomposition.
- `_lib_math-causal-inference-mit-press-essential-knowledge-series.md` — mediation + direct/indirect
  effects at conceptual depth.
- `_lib_science-the-book-of-why-the-new-science-of-cause-and-effect.md` — Pearl's Mediation Formula and
  direct/indirect effects, narrative level (good for the opening intuition).
- `_lib_math-the-art-of-statistics-how-to-learn-from-data.md` — accessible mediation/confounding intuition.
- `_lib_economics-the-lean-startup...md` — only tangential (selection bias mention); not a primary ref
  here, listed for completeness from the scan.

---

## G. Gaps / flags

- **[verify]** Exact page numbers: VanderWeele, Vansteelandt & Robins (2014) and VanderWeele &
  Vansteelandt (2014) — research pass flagged these.
- **[verify]** The "$40B" promotional-spend figure. Primary sources cluster at ~$30B U.S. medical
  marketing (Schwartz & Woloshin 2019) / ~$27B+ (CBO, Pew, early 2010s). The bare "$40B" in the source
  essay needs a direct citation or should be re-sourced (possibly a global/total-promotion figure).
  Recommend the chapter cite Schwartz & Woloshin and footnote the $40B as approximate/[verify].
- **Identification honesty (the big one):** the source essay says "a Living Model on the parameterized
  graph estimates each pathway's *direct contribution*." Technically the *natural* per-pathway split is
  generally not point-identified with three correlated mediators. The chapter must not oversell this —
  use interventional effects + stated assumptions + sensitivity analysis. This is the most important
  flag in the chapter.
- **Data availability:** M1 (educational attitude) is the hardest mediator to *measure* — it lives in
  rep free-text notes, not a clean field. Flag that the elicitation/Interview-Bot work (Ch 8) is partly
  what supplies M1 signal.
