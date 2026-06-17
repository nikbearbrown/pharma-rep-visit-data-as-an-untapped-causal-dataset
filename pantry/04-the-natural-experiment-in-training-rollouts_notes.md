# Chapter 4 Notes — The Natural Experiment in Every Training Rollout

*Research-gathering notes for Week 4 (Act I, MIDTERM chapter). The staggered training-cohort
rollout as an **instrument**; the IV framework; the three IV conditions and the falsification test;
when each condition fails. Notes only — not chapter prose.*

Primary source: `pantry/rep-visit-as-causal-dataset_source.md` ("The natural experiment in every
training rollout" + "The data as a Living Model problem"). TIKTOC Week 4 entry.

---

## A. Foundations

**The core idea (from the source essay).** A new clinical message (e.g., efficacy-first → safety-first)
rolls out through **training cohorts** scheduled by region/geography/hiring calendar — timing set by
admin logistics, not by physician characteristics. Group A (trained month 1) delivers the new deck;
comparable Group B (trained month 2) delivers the old deck in that same window. Physicians did not
choose their message; they got it because of *when their rep trained*. That timing is the source of
quasi-random variation.

**The IV mapping (the chapter's spine).**
- **Treatment** D = message-variant exposure (which deck the physician's rep was delivering).
- **Instrument** Z = training-cohort assignment (rollout timing).
- **Outcome** Y = NPI-level NRx at 30 / 60 / 90 days.

**The three IV conditions** (canonical potential-outcomes / LATE framing — Imbens & Angrist 1994;
Angrist, Imbens & Rubin 1996). With heterogeneous effects, IV identifies a **Local Average Treatment
Effect (LATE)** — the effect for *compliers* (reps/physicians whose message actually tracks cohort
timing) — under:
1. **Relevance / first stage:** Z shifts D, Cov(Z,D)≠0. Test: first-stage F. The source's stated bar
   is **F > 10**. (See misconception below — F>10 is the *legacy* rule.)
2. **Independence / exogeneity:** cohort timing is as-good-as-randomly assigned, uncorrelated with
   physician potential outcomes. **Falsification test:** regress cohort assignment on *pre-rollout*
   prescribing — a significant coefficient kills the design.
3. **Exclusion:** cohort timing affects NRx *only through* the message — no direct channel. Threatened
   if early training also improves general selling skill (or grants more selling days, manager
   attention, sample supply before the measurement window). **Defense:** control rep tenure + baseline
   performance.
- A fourth LATE assumption usually travels with these three: **monotonicity** (no "defiers" — reps
  who, trained later, *revert* to the old message). Worth naming so the LATE interpretation is honest.

**Common misconception (load-bearing — fix it explicitly).**
> "First-stage F > 10 means the instrument is strong enough to trust the t-test."
**Wrong, and now formally so.** F>10 traces to Staiger & Stock (1997) and was made precise by Stock &
Yogo (2005) as a *relative-bias* criterion (2SLS bias < ~10% of OLS bias). It controls **bias**, not
the **size of the t-test you actually report**. Lee, McCrary, Moreira & Porter (2022, *AER*) show that
for the conventional 5% t-test (|t|>1.96) to have correct size in the just-identified single-IV case,
you need a first-stage **F of roughly 104.7**, not 10. Their **tF procedure** adjusts the standard
error as a smooth function of F. In a re-examination of 61 AER papers, ~a quarter of specifications
needed SEs ≥49% larger (5% level). *Teaching move:* report the F, but with one cohort instrument use
the **tF** SE adjustment and/or **Anderson–Rubin** weak-IV-robust confidence sets, and the **effective
F** of Montiel Olea & Pflueger (2013) because NRx data is clustered by rep/territory.

**Worked example (on this dataset — process incl. dead end → lesson → limit).**
1. Build the panel: link `Call2_Key_Message_vod__c` (message delivered) to rep training-cohort timing
   (HR records) to NPI NRx (IQVIA Xponent / Symphony) at 30/60/90d, with territory fixed effects.
2. **Falsification FIRST** (the midterm deliverable): regress cohort on historical pre-rollout NRx. If
   early-trained reps systematically cover higher-baseline physicians, independence fails — stop.
3. **Dead end the chapter should walk through:** running a naive TWFE regression of NRx on a
   "post-training" dummy. With reps trained in waves this mixes "forbidden comparisons" (late cohorts
   judged against already-trained earlier cohorts), so the coefficient can be a negatively-weighted,
   even sign-flipped, summary (Goodman-Bacon 2021; de Chaisemartin & D'Haultfœuille 2020). **Lesson:**
   use a heterogeneity-robust estimator (Callaway–Sant'Anna group-time ATT, or Sun–Abraham IW) with
   *not-yet-trained* cohorts as clean controls, then build the cohort instrument on top.
4. **Limit:** even a clean instrument yields a *population* LATE for compliers — not the per-physician
   "which message next week?" the brand needs. That Rung-3 question is handed to Ch 9 (counterfactual
   engine). The bridge: "the experiment identifies an *average* effect — but the effect runs through
   several pathways" → Ch 5.

---

## B. Examples + failure case

- **Canonical natural-experiment instruments** (the lineage to cite): Angrist & Krueger 1991 *QJE*
  (quarter-of-birth → schooling) and Angrist 1990 *AER* (Vietnam draft lottery → veteran status). The
  "credibility revolution" frames IV as *the* tool for exploiting natural experiments (Angrist &
  Pischke 2010, *JEP*). [verify exact A&K/A pages before print.]
- **The pharma instance:** cohort timing → message → NRx is the field's overlooked natural experiment
  (source essay; TIKTOC Week 4 "Research finding"). It is the basis for Project 1 (Ch 10, staggered
  DiD).
- **Failure case (the chapter's opening / midterm trap).** A team estimates the message effect by
  comparing early-trained vs late-trained physicians *without* the falsification test. It turns out
  star reps were trained first (career-stage selection), so cohort is correlated with rep quality and
  territory potential → **independence violated**, and (because early reps also sell better) **exclusion
  violated**. The "effect" is selection, not message. The falsification regression (cohort on
  pre-rollout NRx) would have caught it; the tenure/baseline controls partly repair exclusion.

---

## C. Connections

- **→ Ch 3 (Rung 1):** IV is the move that lets the data answer the Rung-2 do-question the NBA engines
  cannot — P(NRx | do(message)).
- **→ Ch 5 (three pathways):** the IV gives a *total* message effect; Ch 5 decomposes it into education
  / relationship / reciprocity. IV total → mediation split.
- **→ Ch 6 (colliders):** the exclusion-restriction threats here (skill, selling days) are *confounding*
  of the instrument; Ch 6's dwell/reaction are *post-treatment colliders* — different structural error,
  worth contrasting so Fellows don't conflate "control for it" instincts.
- **→ Ch 10 (Project 1):** this chapter's design is executed as staggered DiD; the TWFE-bias trap and
  Callaway–Sant'Anna belong to both, taught here, run there.
- **→ Living Model framework** (`living-models.md` Pearl's-ladder + Rung-2/3 material): IV is the
  identification layer feeding the parameterized SCM.

---

## D. Current state / key refs / last-3-yr

**IV assumptions & LATE (foundational):**
- Imbens & Angrist (1994), "Identification and Estimation of LATE," *Econometrica* 62(2):467–475.
- Angrist, Imbens & Rubin (1996), "Identification of Causal Effects Using IV," *JASA* 91(434):444–455.

**Weak instruments (the F-threshold story — get this current):**
- Staiger & Stock (1997), *Econometrica* 65(3):557–586 (weak-IV asymptotics; origin of F≈10).
- Stock & Yogo (2005), "Testing for Weak Instruments in Linear IV Regression" (in Andrews & Stock,
  eds., Cambridge UP) — formal critical values; relative-bias criterion.
- **Lee, McCrary, Moreira & Porter (2022), "Valid t-Ratio Inference for IV," *AER* 112(10):3260–3290**
  — F>10 is not enough; need F≈104.7 for correct t-test size; the **tF** procedure. *The current SOTA;
  cite prominently.*
- Andrews, Stock & Sun (2019), "Weak Instruments in IV Regression: Theory and Practice," *Annual
  Review of Economics* 11:727–753 (review; recommends effective-F and Anderson–Rubin).
- Montiel Olea & Pflueger (2013) — effective F under heteroskedasticity/clustering. [verify cite]

**Staggered adoption / DiD revolution (last ~5 yr, all relevant since cohort rollout = staggered):**
- Goodman-Bacon (2021), *J. Econometrics* 225(2):254–277 (TWFE = weighted 2×2 DiDs; forbidden
  comparisons; negative weights; Bacon decomposition).
- de Chaisemartin & D'Haultfœuille (2020), *AER* 110(9):2964–2996 (negative weights; DID_M).
- Callaway & Sant'Anna (2021), *J. Econometrics* 225(2):200–230 (group-time ATT(g,t); `did` package).
- Sun & Abraham (2021), *J. Econometrics* 225(2):175–199 (interaction-weighted estimator; spurious
  pretrends).
- Baker, Callaway, Cunningham, Goodman-Bacon & Sant'Anna (2025), "Difference-in-Differences Designs:
  A Practitioner's Guide," arXiv:2503.13323 — recent applied synthesis of the heterogeneity-robust
  estimators; useful as the "how to actually run it" reference for Project 1 (Ch 10).

**Credibility revolution / falsification:**
- Angrist & Pischke (2010), "The Credibility Revolution in Empirical Economics," *JEP* 24(2):3–30.

---

## E. Teaching

- **Midterm = falsification design** (per TIKTOC assessment): "identify the graph + design the
  falsification test." The deliverable is the *regression of cohort on pre-rollout prescribing*,
  stated as the thing that must pass before any effect is believed.
- **Order of operations is pedagogy:** falsification *before* estimation. The chapter should refuse to
  show an effect estimate until the test passes — model the discipline.
- **One figure that must land:** the 3-step DAG, cohort → message variant → physician → Rx (source
  essay's "production note"). Make the instrument visibly *upstream* of treatment.
- **Anchor the F-threshold correction with the AER re-analysis stat** (a quarter of specs needed SEs
  ≥49% larger) — concrete, memorable, current.
- **Tie each condition to a falsifiable check:** relevance→F (+tF/effective-F); independence→cohort-on-
  pre-NRx + covariate balance; exclusion→tenure/baseline controls + flat event-study leads.

---

## F. Library files (in `pantry/`)

- `_lib_ai-applied-causal-inference-powered-by-ml-and-ai.md` — **richest source.** Covers IV / weak-IV
  / LATE (Ch 13 of that book: weak-instrument simulation, weak-identification-robust inference,
  Anderson–Rubin), difference-in-differences, do-calculus, Spirtes/VanderWeele. Primary go-to for this
  chapter's methods.
- `_lib_math-causal-inference.md` and `_lib_math-causal-inference-mit-press-essential-knowledge-series.md`
  — instrumental variables, natural experiment, selection bias, do-calculus, mediation (conceptual,
  accessible).
- `_lib_science-the-book-of-why-the-new-science-of-cause-and-effect.md` — instrumental variables,
  causal-discovery framing, the do-operator (Pearl/Mackenzie, narrative level).
- `_lib_math-naked-statistics-stripping-the-dread-from-the-data.md` and
  `_lib_math-the-art-of-statistics-how-to-learn-from-data.md` — natural experiment + selection bias at
  the intuition level (good for the opening, not the method).
- (Note: the legacy MD library does not contain Stock-Yogo / Lee-McCrary-Moreira-Porter / the staggered-
  DiD papers — those are web-sourced above and must be cited directly.)

---

## G. Gaps / flags

- **[verify]** Exact pages for Angrist & Krueger (1991, *QJE*) and Angrist (1990, *AER*) — cited from
  standard knowledge by the research pass, not re-checked against originals.
- **[verify]** Montiel Olea & Pflueger (2013) exact citation (effective F-test).
- **Tension to flag for the author:** the source essay states "first-stage F>10" as the bar. The
  current literature (Lee et al. 2022) says that is insufficient for valid t-inference. The chapter
  should *teach the F>10 rule and then correct it* — this is a live pedagogical opportunity, not an
  error to bury. Aging risk: low (the correction is now standard).
- **Open empirical question (from TIKTOC Part 11):** cohort timing is a *conditional* instrument —
  valid only if independence holds. Present as testable, never assumed. This is Adoption Risk #4
  ("Identification assumptions oversold — IV independence").
- **Monotonicity** is under-discussed in the source essay; flag whether to include defier-reps in the
  LATE caveat (recommend: one sentence, since reps reverting to old decks is plausible).
