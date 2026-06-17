# Chapter 10: Project 1 — Staggered Difference-in-Differences on Message-Variant Transitions

## 1. Overview

This is the flagship. Of the three research projects this book builds toward, Project 1 is the most feasible and the highest value, and it is the one a partner can productize fastest. The reason is structural: the natural experiment you learned to see in Chapter 4 — physicians receiving a new clinical message because of *when their rep happened to train*, not because of anything about the physician — is exactly the variation that a modern difference-in-differences (DiD) design is built to exploit.

By the end of this chapter you will be able to take a physician-by-time panel (synthetic here, proprietary at the partner), set up a staggered-adoption DiD, run a falsification test *before* you believe any effect, estimate the treatment-on-the-treated (ToT) lift of a message variant on prescribing, and report that number with its load-bearing assumption and a sensitivity bound stated out loud. You will also learn — by deliberately breaking it — why the obvious move (two-way fixed effects) is biased here, and how to show that bias rather than just assert it.

The chapter is organized around one discipline: **falsification first, effect second.** A DiD estimate is only as good as the parallel-trends assumption underneath it, and the cohort-rollout design is *conditional* — valid only if the rollout timing is uncorrelated with how physicians were already trending. You test that. You do not assume it.

---

## 2. Learning objectives

By the end of this chapter you will be able to:

- **(Create)** Implement a staggered-adoption DiD / event-study design on a physician-by-time panel using the Callaway–Sant'Anna group-time ATT framework.
- **(Analyze)** Run the falsification test — regress cohort assignment on pre-rollout prescribing, and inspect pre-treatment event-study leads — *before* interpreting any treatment effect.
- **(Analyze)** Diagnose why naive two-way fixed effects (TWFE) is biased under staggered timing with heterogeneous effects, and quantify the contamination with a Goodman-Bacon decomposition.
- **(Evaluate)** Report the treatment-on-the-treated effect with its parallel-trends assumption stated and an Honest-DiD sensitivity band attached.
- **(Evaluate)** Identify the collider and selection threats (in-visit engagement; consent opt-out) that would corrupt the estimate if mishandled, and route them correctly.

---

## 3. Opening case: the deck that "hurt" prescribing

A brand team rolls out a new clinical deck — a shift from an efficacy-first to a safety-first message. The rollout is staggered: the West region's reps train in January, the Midwest in February, the Southeast in March, simply because that is how the training logistics fell. Six months later, an analyst is asked the obvious question: did the new deck help?

She does the obvious thing. She builds a panel of physician-month prescribing, codes a treatment dummy that flips on once a physician's rep is delivering the new deck, throws in physician fixed effects and month fixed effects, and reads the coefficient. It comes back **negative and significant.** The new deck, the regression says, *reduced* prescribing.

The brand team is alarmed. There is talk of pulling the deck. Someone drafts a memo recommending a rollback.

Here is the problem: the deck worked. Every cohort that received it prescribed *more* afterward, not less. The negative coefficient is an artifact — a known, named, mathematically guaranteed artifact of running two-way fixed effects on staggered data with effects that grow over time. The regression quietly used the *already-treated* January cohort as a "control" for the later-treated March cohort, and because the January effect was still growing during March's comparison window, that comparison entered with the wrong sign and a *negative weight*. The estimator averaged real positive effects into a fake negative total.

The analyst did nothing careless. She ran the textbook regression. The textbook was wrong. This chapter is about not being that analyst — and, more usefully, about being the analyst who can *show* the brand team exactly where the negative number came from and hand them the right one.

---

## 4. Core sections

### 4.1 The estimand: treatment-on-the-treated (ToT / ATT)

Before any code, name what you are estimating. DiD does *not* give you a population average treatment effect (ATE) — the effect a message variant would have on a randomly chosen physician. It gives you the **average treatment effect on the treated** (ATT, also called ToT): the average effect of the new message *on the physicians whose reps actually delivered it.*

That is the right estimand for this problem. The brand does not detail a random physician; it details the physicians its reps call on. The decision the budget funds is "what does our new deck do to the physicians we reach," and that is the ToT.

Formally, with a treated group and a control group and an outcome measured before and after, the canonical 2×2 DiD identifies:

```
ATT = E[ΔY | treated] − E[ΔY | control]
```

where ΔY is the change in outcome from before to after. Here ΔY is the change in NPI-level new prescriptions (NRx), measured in 30-, 60-, and 90-day windows after the message transition. The treated are physicians of cohort reps already delivering the new deck; the controls are physicians of reps not yet trained.

### 4.2 Parallel trends: the load-bearing assumption

DiD buys you one thing for free and charges you for another. For free, it allows the treated and control groups to sit at completely different *levels* — your treated physicians can be higher prescribers to begin with, and the design differences that out. What it does **not** allow is differing *trends*: the assumption is that, absent the new message, the treated physicians' NRx *would have evolved the same way* as the control physicians'. That is **parallel trends**, and it is the entire identifying assumption.

Two riders from the library DiD chapter (`_lib_did-applied-causal-inference_excerpt.md`) matter operationally:

1. **Parallel trends differences out levels, not trends.** A pre-existing gap is fine. A pre-existing *divergence* is fatal — it will be misread as a treatment effect.
2. **Parallel trends is functional-form dependent.** If trends are parallel in NRx, they need *not* be parallel in log(NRx) (Roth & Sant'Anna 2023, *Econometrica*). The scale is a modeling choice you must make deliberately and defend, not a default. For prescribing counts that grow multiplicatively, this is not academic — the linear and log specifications can disagree.

### 4.3 The TWFE bias trap — the chapter's central inversion

The intuitive design is **two-way fixed effects**: regress NRx on a treatment dummy plus unit (physician/territory) fixed effects plus time fixed effects, and read the treatment coefficient. For a *single* treatment time, this reduces to the clean 2×2 DiD and is fine. Under **staggered adoption with heterogeneous or dynamic effects, it is biased** — and the opening case is what that bias looks like in the wild.

The mechanism is now thoroughly established in the econometrics literature:

- **Goodman-Bacon (2021)** proved that the TWFE DiD coefficient is a *weighted average of every possible 2×2 DiD comparison* between groups and time periods. Some of those comparisons are legitimate (newly treated vs. not-yet-treated). But some use **already-treated units as controls for later-treated units** — the "forbidden comparisons." When treatment effects change over time, those forbidden comparisons get the dynamic effect *backwards*, and the weights attached to them can be **negative.** The result: TWFE can return a negative coefficient even when *every single unit's* effect is positive. (Goodman-Bacon, A. 2021, "Difference-in-Differences with Variation in Treatment Timing," *J. Econometrics* 225(2):254–277; NBER w25018.)
- **de Chaisemartin & D'Haultfœuille (2020)** prove the same pathology in general: TWFE estimates a weighted sum of ATEs whose weights may be negative, so the headline coefficient can be negative while all the underlying ATEs are positive. (AER 110(9):2964–2996; arXiv:1803.08807.)
- **Sun & Abraham (2021)** show the event-study version of the bug: in a TWFE regression with leads and lags, the coefficient on a given relative-time lag is *contaminated by effects from other periods*, and — this is the dangerous part — **apparent pre-trends can be a pure artifact of treatment-effect heterogeneity**, not evidence of a real parallel-trends violation. (*J. Econometrics* 225(2):175–199; arXiv:1804.05785.)

That last point is a double trap. Not only can TWFE flip the sign of your effect; it can also manufacture a fake pre-trend that makes you *abandon a valid design.*

### 4.4 The modern estimators (what to use instead)

The fix is to stop letting a single regression silently choose its comparisons, and instead build the comparisons by hand from clean 2×2 building blocks.

**Callaway & Sant'Anna (2021) — the workhorse.** Define **group-time average treatment effects**, `ATT(g, t)`: the effect for the cohort first treated at time *g*, evaluated at time *t*. Each `ATT(g, t)` is estimated by a clean 2×2 DiD against a *valid* control group — either **never-treated** units or **not-yet-treated** units — using an outcome-regression, inverse-probability-weighting, or (best) **doubly-robust** estimand, allowing parallel trends to hold only *conditional on covariates*. There are no forbidden comparisons and no negative weights by construction. You then **aggregate** the `ATT(g, t)` into whatever summary you need: an overall ATT, a dynamic event-study path by exposure length, an effect by calendar time, or an effect by cohort. (*J. Econometrics* 225(2):200–230; arXiv:1803.09015; `did` R package, bcallaway11.github.io/did.)

**Sun & Abraham (2021) — the interaction-weighted (IW) estimator.** Compute cohort-specific relative-time effects, then aggregate with sample-share weights to recover *uncontaminated* event-study coefficients. Reach for this when the dynamic path is the deliverable and you have a clean never-treated (or last-treated) control. Implemented as `fixest::sunab`.

**de Chaisemartin & D'Haultfœuille — `did_multiplegt` family.** Robust to heterogeneity and, crucially, handles treatments that turn **on *and* off.** This matters here: message decks get rolled back and re-versioned. If your treatment is *non-absorbing* (a physician can revert to the old deck), Callaway–Sant'Anna's absorbing-treatment design no longer fits and you should prefer this family.

**Goodman-Bacon (2021) — the diagnostic.** Primarily, run the Bacon decomposition to *show* how much of a naive TWFE estimate comes from forbidden comparisons. It is your teaching exhibit: it makes the bug visible before you switch estimators.

> **Term box.**
> *Staggered adoption:* different units begin treatment at different times.
> *Absorbing treatment:* once treated, always treated (no reversal). Non-absorbing = can turn off.
> *Group-time ATT, `ATT(g,t)`:* effect for the cohort first treated at *g*, measured at *t*.
> *Forbidden comparison:* a 2×2 DiD that uses an already-treated unit as the control — the source of negative weights.
> *Event study:* the sequence of `ATT(g,t)` plotted against relative time (periods since treatment), with pre-periods as a falsification check.

### 4.5 Falsification first — the chapter's spine

The cohort-rollout design is valid *only if* timing is uncorrelated with physician prescribing dynamics. You test this three ways, in order, and you do it *before* you look at any treatment effect.

**Test 1 — Independence / no selection on trends.** Regress cohort assignment on *pre-rollout* prescribing (and pre-rollout prescribing growth). A significant coefficient means cohort timing is correlated with physician characteristics — the rollout was not "as good as random" — and the experiment is dead. In Chapter 4 you met this as the IV-independence falsification; in DiD it is the parallel-trends pre-test.

**Test 2 — Pre-trends / event-study leads.** Estimate `ATT(g, t)` for *pre-treatment* periods (relative time < 0). They should be approximately zero. Two cautions ride along: heed Sun–Abraham (use the *clean* estimator so an apparent pre-trend is not just heterogeneity contamination), and heed **Roth (2022)** — pre-testing for parallel trends and then conditioning your analysis on having *passed* the test distorts inference, because the test is underpowered and selection-biased. (*AER: Insights* 4(3):305–322.) So you report the leads, but you do not treat "leads look flat" as proof.

**Test 3 — Honest sensitivity.** Rather than assuming parallel trends holds *exactly*, use **Rambachan & Roth (2023), "Honest DiD"**: bound how large a post-period violation could plausibly be, relative to the violations you actually observe in the pre-period, and report which conclusions survive. (*Review of Economic Studies*.) This converts "trust me, trends are parallel" into "here is the effect, and here is how much trend-violation it can absorb before the conclusion breaks." Implemented as `HonestDiD`.

And one more, carried from the IV framing in Chapter 4: **exclusion.** Cohort timing should affect NRx *only* through the message. The threat is that early-trained reps also got better at selling generally. You defend by controlling **rep tenure and baseline rep performance** so the cohort effect is not just an early-rep skill effect.

The single best "current state" cite for this whole discipline is **Roth, Sant'Anna, Bilinski & Poet (2023), "What's Trending in Difference-in-Differences?"** (*J. Econometrics* 235(2):2218–2244; arXiv:2201.01194) — the practitioner survey and recommendation set.

### 4.6 The required panel

From the source essay's Project 1 specification, you need:

- `Call2_Key_Message_vod__c` — the delivered message variant (defines who got the new deck). *(Veeva field name, mid-2026.)*
- **Training-cohort timing** — from HR / the rollout calendar; this is the staggered treatment time *g*.
- **NPI-level NRx panel** — IQVIA Xponent or Symphony, the outcome Y at 30/60/90 days.
- **Territory fixed effects.**
- **Rep tenure and baseline rep performance** — the exclusion controls.

The firewall (Part 10 of the book plan) is explicit here: **Project 1 needs proprietary or synthetic data.** Cohort timing and `Call2_Key_Message_vod__c` are partner-owned and never enter the public book or repo. Project 2 (meals, next chapter) is the only fully public-data-runnable project. So for this chapter you run on the **synthetic rep-visit dataset, which ships with known ground-truth structure** — which is a feature, not a consolation prize: it lets you *verify your method recovers the truth*, and verify that TWFE *fails* to.

---

## 5. Worked example on this dataset (full run, including dead ends)

We run Project 1 on the synthetic panel. The synthetic generator plants a known ground truth: three cohorts (`g ∈ {Jan, Feb, Mar}`) adopt the safety-first deck; reps not trained in the window are the never-treated control; the true ToT is a *positive, dynamically growing* lift in NRx (small at 30 days, larger at 60 and 90). The dynamics are deliberate — that is exactly the condition under which TWFE breaks, so we can watch it break.

We'll use Python (`econml`, `pandas`, `statsmodels`) where possible and note where the cleanest tooling is in R (`did`, `bacondecomp`, `fixest`, `HonestDiD`). The point is the workflow, not the language.

### Step 0 — Load and sanity-check the panel

```python
import pandas as pd
import numpy as np

panel = pd.read_parquet("synthetic_rep_visit_panel.parquet")
# columns: npi, month, nrx, cohort_g (treatment month; 0 = never),
#          territory, rep_tenure, baseline_nrx
# 'treated' turns on at month >= cohort_g for cohorts with g > 0

panel["rel_time"] = np.where(
    panel["cohort_g"] > 0,
    panel["month"] - panel["cohort_g"],
    np.nan,  # never-treated have no relative time
)
print(panel.groupby("cohort_g")["npi"].nunique())
```

Confirm you have a clean never-treated group (`cohort_g == 0`) — it is your safest control, and Honest-DiD wants it.

### Step 1 (the dead end we run on purpose) — naive TWFE

```python
import statsmodels.formula.api as smf

panel["post"] = (panel["month"] >= panel["cohort_g"]) & (panel["cohort_g"] > 0)
twfe = smf.ols("nrx ~ post + C(npi) + C(month)", data=panel).fit(
    cov_type="cluster", cov_kwds={"groups": panel["territory"]}
)
print(twfe.params["post[T.True]"])   # the naive treatment coefficient
```

On the synthetic data — where the *true* ToT is positive — this prints a coefficient that is **attenuated, near zero, or outright negative.** That is the opening-case disaster reproduced on data whose truth we know. Do not stop here. Do not believe it.

### Step 2 — Show *why*: the Bacon decomposition

The honest move is not just to switch estimators but to *exhibit the bug.* The Goodman-Bacon decomposition splits the TWFE estimate into its 2×2 components and the weight on each. (This is cleanest in R's `bacondecomp`; the logic is what matters.)

```r
# R
library(bacondecomp)
bd <- bacon(nrx ~ post, data = panel, id_var = "npi", time_var = "month")
# bd reports each comparison type and its weight:
#   "Treated vs Untreated"            (good)
#   "Earlier vs Later Treated"        (good)
#   "Later vs Earlier Treated"        (FORBIDDEN — already-treated as control)
aggregate(weight ~ type, data = bd, FUN = sum)
```

You will see a non-trivial share of total weight sitting on **"Later vs Earlier Treated"** — the forbidden comparisons — and, because the synthetic effects grow over time, those comparisons carry the negative sign that dragged the TWFE coefficient down. *Now* you can tell the brand team precisely where their negative number came from: "X% of your estimate was built from comparisons that used already-treated physicians as a control, and those comparisons run backwards when the effect is still growing."

### Step 3 — Falsify *before* you estimate

**Test 1 — regress cohort on pre-rollout prescribing.**

```python
pre = panel[panel["month"] < panel["cohort_g"].replace(0, np.inf)]
pre_phys = pre.groupby("npi").agg(
    cohort_g=("cohort_g", "first"),
    pre_nrx=("nrx", "mean"),
    pre_slope=("nrx", lambda s: np.polyfit(range(len(s)), s, 1)[0] if len(s) > 1 else 0.0),
).reset_index()
pre_phys = pre_phys[pre_phys["cohort_g"] > 0]

falsif = smf.ols("cohort_g ~ pre_nrx + pre_slope", data=pre_phys).fit()
print(falsif.pvalues[["pre_nrx", "pre_slope"]])
```

If `pre_slope` is significant, *stop.* Cohort timing tracks pre-existing prescribing trends; parallel trends is implausible and the design is dead. On the synthetic data (built clean), these should be non-significant — but you must check, every time, on every dataset, before you let yourself believe an effect.

**Test 2 — pre-treatment event-study leads** (estimated with the clean estimator in Step 4, then inspected for relative time < 0).

### Step 4 — Estimate it right: Callaway–Sant'Anna group-time ATT

```r
# R — the canonical implementation
library(did)
att <- att_gt(
  yname   = "nrx",
  tname   = "month",
  idname  = "npi",
  gname   = "cohort_g",          # 0 = never-treated control
  xformla = ~ rep_tenure + baseline_nrx + territory,  # conditional parallel trends + exclusion controls
  data    = panel,
  control_group = "notyettreated",  # or "nevertreated"
  est_method    = "dr"              # doubly robust
)

# Falsification (pre-trends) lives in the same object: ATT(g,t) for t < g should be ~0
es <- aggte(att, type = "dynamic")   # event-study aggregation
summary(es)                          # inspect leads (rel time < 0) and lags (>= 0)

overall <- aggte(att, type = "group")  # overall ToT, weighted across cohorts
summary(overall)
```

A Python-only path: `econml`'s DiD utilities or the `differences` package can estimate group-time ATTs; the residual-on-residual doubly-robust logic is the same one DML uses (Chapter 11). The key arguments are the same in any language: **never/not-yet-treated control**, **doubly-robust estimation**, **conditioning covariates chosen for conditional parallel trends and exclusion (tenure, baseline) — never in-visit engagement.**

On the synthetic data, the event study now shows **flat pre-treatment leads (≈ 0)** and **positive, growing lags** — recovering the planted ground truth that TWFE destroyed.

### Step 5 — Attach an honest sensitivity band

```r
library(HonestDiD)
# Feed the event-study estimates + vcov into HonestDiD's relative-magnitudes bound:
# "how large could a post-period trend violation be, relative to observed pre-period
#  deviations, before the ToT loses significance?"
sensitivity <- honest_did(es, type = "relative_magnitude", Mbarvec = c(0.5, 1, 1.5, 2))
plot_sensitivity(sensitivity)
```

The output is the deliverable's spine: *the ToT remains positive and significant as long as any post-period parallel-trends violation is no more than M× the largest violation we observed before treatment.* That is a number a partner's biostatistician can argue with — which is the point.

### Step 6 — Read it out

The ToT readout, stated honestly:

> Under conditional parallel trends (conditioning on rep tenure, baseline NRx, and territory), and using never-treated reps as controls, the safety-first deck raised treated physicians' NRx by **[Δ at 90 days, generated on the synthetic data — `[verify]`]**, with effects growing over the 30/60/90-day windows. The estimate survives Honest-DiD relative-magnitude violations up to **M = [value]**. Naive TWFE returned **[the attenuated/negative number]**; a Bacon decomposition attributes **[X%]** of that estimate to forbidden already-treated-as-control comparisons.

**Lesson.** The right estimator did not just "improve precision" — it *changed the sign of the conclusion.* The discipline that saved you was running the falsification and the decomposition *before* believing the headline number.

**Limit.** The ToT is the effect on the *treated* — the physicians the reps reached — under *parallel trends*, on *this* scale, for the *consenting, logged* sampling frame. It is not a population effect, it is not assumption-free, and (see §7) the sampling frame itself is shaped by consent opt-out, which interacts with parallel trends if opt-out correlates with prescribing dynamics. The number is defensible, not omniscient.

> **Dead-end cross-link (Chapter 6).** A tempting "improvement" is to add in-visit engagement — `Duration_vod` (dwell), `Reaction_vod` — to `xformla` "to control for how engaged the physician was." **Do not.** Those are *post-treatment colliders*: caused by the very message you are studying. Conditioning on them inside the DiD reopens a closed path and corrupts the message→NRx effect. This is the same collider discipline of Chapter 6, now operating inside Project 1. Chapter 11 makes it the central worked example.

---

## 6. Common misconceptions

- **"Staggered rollout? Just throw in unit + time fixed effects and read the treatment coefficient."** Confidently wrong. Under staggered timing with heterogeneous/dynamic effects, TWFE mixes in forbidden already-treated-as-control comparisons with negative weights and can flip the sign. Use group-time ATT (Callaway–Sant'Anna) or interaction-weighted (Sun–Abraham); use Goodman-Bacon to *show* the contamination.
- **"Nonzero pre-trends in my TWFE event study mean parallel trends is violated."** Not necessarily. Under TWFE, apparent pre-trends can be a *pure artifact* of treatment-effect heterogeneity (Sun–Abraham). Re-check with the clean estimator before abandoning the design.
- **"My parallel-trends test passed, so the design is valid."** A passed test is weak evidence — the test is underpowered, and conditioning your analysis on having passed it biases inference (Roth 2022). Report leads *and* an Honest-DiD sensitivity band; never treat the test as a green light.
- **"DiD gives me the effect on a typical physician."** No — it gives the effect on the *treated.* The population ATE is a different quantity the design does not identify.
- **"Parallel trends in levels means parallel trends in logs."** False. Parallel trends is functional-form dependent (Roth & Sant'Anna 2023). Choose the scale and defend it.

---

## 7. Evidence check + consent/compliance & patient-welfare check

**Evidence check.** The staggered-DiD canon cited here is *independent academic econometrics*, peer-reviewed and method-stable: Goodman-Bacon (2021), Callaway–Sant'Anna (2021), Sun–Abraham (2021), de Chaisemartin–D'Haultfœuille (2020), Roth (2022), Rambachan–Roth (2023), and the Roth–Sant'Anna–Bilinski–Poet (2023) survey. None is vendor material; none is hypothetical. The *application* to rep-visit cohort rollouts is this book's contribution and is presented as **conditional, not established** — the cohort-timing instrument is valid *only if* independence/parallel-trends holds, which is why the design is falsification-first. The worked-example magnitudes are generated on synthetic data with known ground truth and carry `[verify]` — they demonstrate that the *method* recovers truth, not that any real-world deck has a particular effect.

**Consent / compliance & patient-welfare check.** Two live issues. First, the **regulatory line**: this design estimates the *total* message→prescribing effect, which braids together the educational pathway (legally drivable) and the reciprocity/relationship pathways (regulatorily fraught — see Chapters 5 and 13). A ToT number alone does not tell a partner *which pathway* moved prescribing, and shipping targeting on the total effect without that decomposition risks optimizing a pathway counsel would not bless. Route the pathway question to Chapters 5 and 11. Second, the **consent collider**: the NRx panel's sampling frame is shaped by physician opt-out (`CLM_EXPLICIT_OPT_IN_vod` / `CLM_OPT_OUT_BEHAVIOR_vod`). If privacy-conscious, industry-skeptical physicians opt out *and* prescribe differently, the logged sample is a selected one, and selection interacts with parallel trends. Note it here as a threat; the full treatment (FCI, opt-out drift monitoring) is Chapter 13. The patient-welfare frame stays primary: the only defensible thing to *cause* is better-informed, on-label prescribing, never volume for its own sake.

---

## 8. Named Fellow artifact

**Artifact: the Project 1 staggered-DiD run, end to end, falsification-first, with a ToT readout.**

Produce a notebook (or `.py` + `.R` pair) that, on the synthetic rep-visit panel:

1. Builds the physician-by-time panel with cohort time *g*, relative time, and exclusion controls (tenure, baseline NRx, territory).
2. Runs naive TWFE and a Bacon decomposition; reports the forbidden-comparison weight and explains the resulting bias in one paragraph.
3. Runs the **falsification test first** — cohort-on-pre-prescribing regression *and* pre-treatment event-study leads — and states, explicitly, that the effect is only interpreted because falsification passed.
4. Estimates `ATT(g, t)` via Callaway–Sant'Anna (doubly robust, not-yet/never-treated control), aggregates to a dynamic event study and an overall ToT.
5. Attaches an Honest-DiD sensitivity band.
6. Closes with the **ToT readout paragraph** (§5 Step 6 template), with assumptions and sensitivity stated, and confirms the method **recovered the known synthetic ground truth** while TWFE did not.

This artifact is the Week-10/11 graded Project Run (200 pts). It is also the thing a partner can hand to a biostatistician.

---

## 9. Research finding for Fellows

The cohort-rollout natural experiment is the field's overlooked clean(ish) design. Pharma has spent fifteen years generating staggered message-variant rollouts whose timing was set by training logistics — exactly the variation modern DiD was built to exploit — and analyzing none of them causally. Project 1 is therefore *both* publishable (a clean application of the post-2020 DiD toolkit to a dataset the econometrics literature has never seen) *and* directly productizable (the ToT and its dynamic path feed straight into the persuadables ranking of Chapter 12). The finding is not "the deck works"; it is "the variation to find out whether any deck works has been sitting in the CRM the whole time, and the method to read it without fooling yourself now exists."

---

## 10. Exercises

1. **(Apply)** On the synthetic panel, run the naive TWFE regression and the Bacon decomposition. Report the share of total weight on forbidden ("Later vs Earlier Treated") comparisons and write two sentences explaining, mechanically, why that share produces the biased coefficient you observed.

2. **(Apply+)** Re-estimate with Callaway–Sant'Anna (doubly robust, never-treated control). Produce the event-study plot. Compare the overall ToT to the TWFE coefficient and to the planted synthetic ground truth. By how much, and in which direction, did TWFE mislead?

3. **(Apply+ / Evaluate — produces artifact)** Build the full falsification-first workflow as the Named Fellow Artifact (§8): cohort-on-pre-prescribing test, pre-trend leads, Callaway–Sant'Anna ToT, and an Honest-DiD sensitivity band. Deliver the notebook *and* the one-paragraph ToT readout with assumptions and sensitivity stated.

4. **(Evaluate)** Deliberately add `Duration_vod` (dwell) and `Reaction_vod` to the conditioning set, re-estimate, and observe how the ToT moves *away* from the synthetic ground truth. Write the one-paragraph memo a colleague would need to understand why "controlling for engagement" corrupted the estimate (cross-reference Chapter 6).

5. **(Apply — non-absorbing variant)** Modify the synthetic generator so a subset of cohorts *rolls the new deck back* (treatment turns off). Show that Callaway–Sant'Anna (absorbing-treatment design) now misbehaves, and re-estimate with de Chaisemartin–D'Haultfœuille's `did_multiplegt` family. Note which estimator the assignment's data actually warrants.

---

## 11. When to use AI (five-part block)

**1. When to use AI.** Use an LLM to *scaffold the code* — generate the `did` / `fixest::sunab` / `bacondecomp` / `HonestDiD` call structure, translate an R workflow into a Python `econml`/`differences` equivalent, and explain *why* a given estimator is appropriate for staggered, possibly non-absorbing treatment. Use it to draft the plain-English explanation of the Bacon decomposition for a non-technical brand team. Use it to sanity-check that your conditioning set contains only pre-treatment variables.

**2. When NOT to use AI.** Do **not** let the model *choose your estimator from the data* or *declare parallel trends satisfied.* Estimator choice depends on whether treatment is absorbing — a fact about your design the model cannot read off the data. And "parallel trends holds" is an *assumption about the unobserved counterfactual*; no LLM (and no test) can certify it. Do not let it add in-visit engagement to the controls because it is "predictive" — that is the collider error, and the model will cheerfully make it if you ask for "the best controls."

**3. LLM exercise (ready to paste).**

```
I have a physician-by-month panel: columns npi, month, nrx, cohort_g
(treatment-start month; 0 = never treated), territory, rep_tenure,
baseline_nrx. Treatment is a message-deck rollout staggered across
training cohorts; I am not yet sure whether decks ever get rolled back.

1. Explain, in plain language a brand manager would understand, why a
   naive two-way fixed-effects regression of nrx on a post-treatment
   dummy could return a NEGATIVE coefficient even if the deck helped
   every cohort. Name the mechanism.
2. Give me R code using the `did` package to estimate group-time ATTs
   with a doubly-robust estimator, a not-yet-treated control group, and
   conditioning on rep_tenure + baseline_nrx + territory, then aggregate
   to an event study and an overall ToT.
3. Tell me which TWO checks I must run BEFORE I interpret the ToT, and
   what result on each would kill the design.
4. Tell me what to do differently if decks DO get rolled back.

Do not tell me parallel trends is satisfied — I will test that myself.
List any variable in my schema I should NOT condition on, and why.
```

**4. CLI exercise (Claude Code).** In a repo containing the synthetic panel, ask Claude Code to: (a) write the falsification-first pipeline as a runnable script with `pytest` checks that *assert the method recovers the synthetic ground-truth ToT within tolerance* and *assert that naive TWFE does not*; (b) wire a `make verify` target that runs both and prints a pass/fail table. The validation artifact — "CS recovers truth, TWFE fails" — is the test suite itself.

**5. AI validation.** The check is recovery, not plausibility. The synthetic dataset has a known ground-truth ToT; your estimate (and the LLM's scaffolded code) is validated by *recovering that number* within its confidence interval — and, as a teaching artifact, by confirming that TWFE *fails* on the identical data. An estimate that "looks reasonable" but misses the planted truth is a failed validation, full stop.

---

## 12. AI Use Disclosure

This chapter was drafted with AI assistance for prose structuring and code scaffolding. Every method citation was verified against the Chapter 10 research notes and not generated by the model; the staggered-DiD canon (Goodman-Bacon, Callaway–Sant'Anna, Sun–Abraham, de Chaisemartin–D'Haultfœuille, Roth, Rambachan–Roth) is cited from primary sources. Worked-example magnitudes are flagged `[verify]` pending generation on the synthetic dataset. The bonus-earning disclosure for a Fellow: name the one assumption — parallel trends on the chosen scale, for the consenting sampling frame — that *no dataset and no estimator could supply*, and which the design therefore tests rather than proves.

---

## 13. What would change my mind

I would weaken the claim that Project 1 is "the most feasible, highest-value" study if: (a) on real partner data, the cohort-on-pre-prescribing falsification *fails* routinely — i.e., training-cohort timing turns out to be systematically correlated with physician prescribing (e.g., high-value territories staffed by senior reps trained first), which would make the "as-good-as-random" rollout a fiction; (b) message decks are revised so frequently and non-absorbingly that no clean treatment definition survives; or (c) the consent opt-out selection is severe enough that the logged sampling frame's parallel trends cannot be defended even with FCI (Chapter 13). Any of these would not kill DiD as a tool but would demote Project 1 from flagship to caveated.

---

## 14. Still puzzling

- How non-absorbing is a real message rollout, in practice? The literature's cleanest estimators (Callaway–Sant'Anna) assume absorbing treatment; reality is decks that get re-versioned. The de Chaisemartin–D'Haultfœuille family handles on/off treatment, but the *right treatment representation* for "deck version 3.2 vs 3.1 vs 2.0" is genuinely unsettled.
- What is the correct outcome scale? Parallel trends in NRx vs log(NRx) can disagree, and prescribing counts are bounded and skewed. The functional-form choice is load-bearing and under-discussed in pharma analytics.
- How do you aggregate ToT across cohorts when the *brand's* decision is forward-looking (which deck for the *next* rollout)? The population ToT is a backward-looking summary; the Rung-3 individual recommendation (Chapter 9) is the real product. The bridge from one to the other is not automatic.

---

## 15. Summary

Project 1 turns the training-cohort rollout — variation set by logistics, not by physicians — into a staggered-adoption difference-in-differences estimate of the treatment-on-the-treated effect of a message variant on prescribing. The estimand is the ToT, not the population ATE. The identifying assumption is parallel trends, which is functional-form dependent and *tested, not assumed.* The intuitive estimator, two-way fixed effects, is biased under staggered timing with heterogeneous effects — it averages in forbidden already-treated-as-control comparisons with negative weights and can flip the sign of a real positive effect. The fix is the modern canon: Callaway–Sant'Anna group-time ATTs (doubly robust, never/not-yet-treated controls), Sun–Abraham interaction-weighted event studies, de Chaisemartin–D'Haultfœuille for non-absorbing treatment, and Goodman-Bacon as the diagnostic that *shows* the bug. The discipline is falsification first: regress cohort on pre-prescribing, inspect pre-trend leads, attach an Honest-DiD sensitivity band — and only then read the ToT. On synthetic data with known ground truth, the right workflow recovers the planted effect while TWFE fails, which is precisely the proof a Fellow needs before trusting the method on data whose truth is unknown.

---

## 16. Key terms

- **Treatment-on-the-treated (ToT / ATT):** the average effect of the message variant on the physicians whose reps actually delivered it.
- **Parallel trends:** the assumption that, absent treatment, treated and control outcomes would have evolved identically; the identifying assumption of DiD.
- **Staggered adoption:** units begin treatment at different times.
- **Two-way fixed effects (TWFE):** unit + time fixed-effects regression; biased under staggered timing with heterogeneous effects.
- **Forbidden comparison:** a 2×2 DiD using an already-treated unit as the control — source of negative weights (Goodman-Bacon).
- **Group-time ATT, `ATT(g,t)`:** Callaway–Sant'Anna's clean per-cohort, per-period effect.
- **Interaction-weighted estimator:** Sun–Abraham's uncontaminated event-study estimator.
- **Honest DiD:** Rambachan–Roth sensitivity analysis bounding plausible parallel-trends violations.
- **Falsification-first:** testing the identifying assumptions before interpreting any effect.
- **Absorbing vs non-absorbing treatment:** once-on-always-on vs treatment that can turn off.

---

## 17. Bridge

Project 1 gave you an *average* causal number — the ToT, and its dynamic path. But two questions remain that an average cannot answer: *for whom* does promotion work (heterogeneity), and *through which pathway* does it work (education vs reciprocity)? Chapter 11 takes both on with Projects 2 and 3 — a causal forest that estimates a per-physician meal effect on fully public Open Payments data, and Double Machine Learning on the slide-sequence clickstream, where the very signals that tempt you as "controls" turn out to be the colliders that destroy the estimate. The collider discipline you were warned about inside the DiD becomes the entire lesson next.

---

## 18. Further reading

- Goodman-Bacon, A. (2021). "Difference-in-Differences with Variation in Treatment Timing." *Journal of Econometrics* 225(2):254–277. NBER w25018. — The decomposition; forbidden comparisons; negative weights.
- Callaway, B. & Sant'Anna, P. H. C. (2021). "Difference-in-Differences with Multiple Time Periods." *Journal of Econometrics* 225(2):200–230. arXiv:1803.09015. — Group-time ATTs; the `did` package.
- Sun, L. & Abraham, S. (2021). "Estimating Dynamic Treatment Effects in Event Studies with Heterogeneous Treatment Effects." *Journal of Econometrics* 225(2):175–199. arXiv:1804.05785. — Event-study contamination; the IW estimator.
- de Chaisemartin, C. & D'Haultfœuille, X. (2020). "Two-Way Fixed Effects Estimators with Heterogeneous Treatment Effects." *American Economic Review* 110(9):2964–2996. arXiv:1803.08807. — Negative-weights result; `did_multiplegt`.
- Roth, J. (2022). "Pre-test with Caution: Event-Study Estimates After Testing for Parallel Trends." *AER: Insights* 4(3):305–322. — Pre-testing distortion.
- Rambachan, A. & Roth, J. (2023). "A More Credible Approach to Parallel Trends" (Honest DiD). *Review of Economic Studies*. — Sensitivity bounds.
- Roth, J., Sant'Anna, P. H. C., Bilinski, A. & Poet, J. (2023). "What's Trending in Difference-in-Differences? A Synthesis of the Recent Econometrics Literature." *Journal of Econometrics* 235(2):2218–2244. arXiv:2201.01194. — The practitioner survey; best single "current state" cite.
- Roth, J. & Sant'Anna, P. H. C. (2023). "When Is Parallel Trends Sensitive to Functional Form?" *Econometrica* 91(2):737–747. — The functional-form rider.
- Baker, A., Callaway, B., Cunningham, S., Goodman-Bacon, A. & Sant'Anna, P. H. C. (2025). "Difference-in-Differences: A Practitioner's Guide." arXiv:2503.13323. — The applied practitioner's guide.
- Software: R `did` (Callaway–Sant'Anna), `fixest::sunab` (Sun–Abraham IW), `did_multiplegt` / `DIDmultiplegtDYN` (de Chaisemartin–D'Haultfœuille), `bacondecomp` (Goodman-Bacon diagnostic), `HonestDiD` (Rambachan–Roth).
- In-library: `_lib_did-applied-causal-inference_excerpt.md` — canonical 2×2 DiD, parallel trends, ATET identification, functional-form dependence, conditional-DiD-via-DML.
