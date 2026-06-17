# Chapter 1 — The Experiment Nobody Knows They're Running — Research Notes

**Chapter one-line (TIKTOC):** Fellows learn to see rep-visit telemetry as a dense behavioral *and*
causal dataset, not a CRM log.
**LOs:** (Understand) describe what the iPad logs and why it's causal-grade; (Analyze) distinguish a
behavioral signal from a causal claim in this data; (Apply/own-thread) state the do-question your
project will answer.
**Research finding for Fellows:** the whole dataset class is unanalyzed causally — the open territory.

> Scope note: this chapter is the *hook* and *altitude* chapter. It establishes WHAT the data is at
> 30,000 ft and the central framing ("15-year unrun natural experiment"; behavioral-signal vs
> causal-claim). The field-by-field schema is Ch 2; the ladder/Rung-1 argument is Ch 3. Keep this
> chapter from doing their jobs.

---

## A. Conceptual foundations

### A1. The rep-visit iPad as a behavioral dataset, not a CRM log
- **Explanation.** During a detailing visit the Veeva CRM iPad (the CLM media player) silently logs,
  per slide: seconds dwelt, navigation order (skips/loops/back-tracks), the rep's real-time
  reaction button, the channel (face-to-face / remote), all packaged at session end — timestamped and
  keyed to rep ID, physician NPI, territory, and training cohort. The rep does nothing to trigger this;
  it is passive telemetry. (Source essay, "Hook" & "What the data actually is"; field behavior verified
  against Veeva CRM Help — see F.)
- **Common misconception.** "It's a CRM, so it's a record-keeping log — call notes and activity stamps."
  Reframe: a CRM *schema* hosts the data, but what accrues is slide-level behavioral telemetry on
  hundreds of thousands of physician interactions per year — closer to a clickstream/session-analytics
  dataset than to a contact log. The CRM framing is exactly why nobody treats it as analyzable behavior.
- **Worked example.** A single visit produces a `Call_Clickstream_vod` payload: slide 3 viewed 47s,
  slide 4 skipped, slide 3 revisited, reaction "neutral" on the safety slide. That one row is a
  micro-behavioral trace; multiplied across an NPI over months it is a longitudinal behavioral series.
- **Source.** `rep-visit-as-causal-dataset_source.md` (Hook); Veeva CRM Help, "Tracking CLM Call
  Clickstream Activity."

### A2. Behavioral signal vs causal claim (the spine distinction)
- **Explanation.** The data is *behaviorally* dense (it records what happened in fine grain) but a
  behavioral signal is not a causal claim. "Physicians who dwelled longer on the safety slide
  prescribed more" is a Rung-1 association; "delivering the safety-first message *caused* more
  prescribing" is a Rung-2 claim that the raw telemetry alone cannot license. This chapter plants the
  distinction; Ch 3 formalizes it with Pearl's ladder.
- **Common misconception.** "Rich, granular, timestamped data = causal data." Granularity buys
  resolution, not identification. A million high-resolution observations of a confounded process give a
  precisely-estimated *biased* number.
- **Worked example (the 47-second ambiguity, previewed).** `Duration_vod`=47s on the safety slide is
  consistent with (1) compelled/reading carefully, (2) skeptical/hunting the flaw, (3) polite while
  distracted. Identical behavioral datum, three different causal stories. (Fully developed in Ch 3's
  epistemic-gap section and Ch 7's Markov-equivalence chapter.)
- **Source.** Source essay ("The epistemic gap"); Pearl & Mackenzie, *The Book of Why* (ladder), see
  Ch 3 notes.

### A3. The "15-year unrun natural experiment" framing
- **Explanation.** Pharma has logged this telemetry at scale for ~15 years (Veeva CRM dominance + iPad
  detailing era, roughly 2010s onward). Across those years, message decks changed, rolled out in
  cohorts, varied by territory — generating quasi-experimental variation in *which message reached
  which physician when*. That variation is the raw material of a natural experiment. Nobody has run the
  analysis. (The *mechanism* of the natural experiment — training-cohort rollout as instrument — is
  Ch 4; here it is asserted as the motivating claim.)
- **Common misconception.** "If there were a usable experiment in there, someone would have found it."
  The data sits in commercial/CRM silos analyzed for engagement reporting and rep performance, not
  causal identification; the people with the data lack the causal frame, and the people with the causal
  frame lack the data. That gap *is* the book's opportunity.
- **Worked example.** Group A reps (trained month 1) deliver a new deck; comparable Group B reps
  (trained month 2) still deliver the old deck that same month. The physicians did not choose their
  message — they got it because of when their rep was scheduled to train. (Setup only; identification
  in Ch 4.)
- **Source.** Source essay (Hook; "The natural experiment in every training rollout").

### A4. Scale: the pharma field force (why this dataset is large and uniform)
- **Explanation.** Veeva CRM is the dominant pharma field-sales system (source essay: ">80% of pharma
  field sales"). Because one platform standardizes the schema across most of the industry, the
  telemetry is both *vast* and *comparable* — a near-census of US pharma detailing in a shared data
  model. That uniformity is what makes a cross-firm or large-panel causal study even conceivable.
- **Common misconception.** "Each company's CRM data is bespoke." The Veeva CLM object model
  (`Call_Clickstream_vod`, `Reaction_vod`, etc.) is shared infrastructure; the *content* differs but
  the *schema* is common. [verify the ">80%" market-share figure to a current Veeva/industry source —
  see G.]
- **Worked example.** A method that works on one brand's `Call2_Key_Message_vod__c` panel transfers,
  schema-wise, to another's — the firewall problem (Ch 13) is about data *ownership*, not data *shape*.
- **Source.** Source essay ("What the data actually is"); Veeva market-share figure flagged.

---

## B. Domain examples + a failure case

- **Domain example — the silent logger.** Mid-visit, the rep flips slides while talking; the iPad
  writes `Duration_vod`/`Display_Order_vod`/`Reaction_vod` without any deliberate data-entry act. The
  behavioral record is a *byproduct* of normal selling, which is why it's both rich and unexamined.
- **Domain example — the assembled panel (preview).** Append NPI-level NRx (IQVIA Xponent / Symphony),
  CMS Open Payments meals, territory history, rep tenure, and cohort timing, and you have a
  physician-by-time panel: treatment (message delivered), outcome (NRx), candidate instrument (cohort),
  covariates (tenure, baseline). (Full assembly = Ch 2.)
- **Failure case — "cool dataset" mistaken for "causal dataset."** A team excited by the granularity
  builds an engagement dashboard (dwell, drop-off, reaction heatmaps) and calls it "understanding what
  works." It answers *what physicians did*, never *what the message caused*. The richer the dashboard,
  the more confident — and the more confidently Rung-1. This is the motivating failure the whole book
  corrects; Ch 3 diagnoses it formally.

---

## C. Connections / dependencies

- **Prereqs:** none (Ch 1 is the hook; depends on nothing — TIKTOC Part 6).
- **Unlocks:** Ch 2 (the schema, so the altitude claim becomes field-level precision); the
  behavioral-vs-causal seed germinates in Ch 3 (the ladder) and Ch 7 (Markov equivalence).
- **Adjacent / family:** `living-models.md` Ch 1 ("The Dashboard That Lied") makes the *same*
  observational-dashboard failure in a generic enterprise setting and introduces Pearl's ladder — this
  chapter is its pharma-specific instantiation. Reuse that chapter's framing of "a model that predicts
  well but cannot answer a do-question."
- **Own-thread:** by end of chapter the Fellow states the *do-question* their project will answer
  (carried to Ch 4 identification and the final brief).

---

## D. Current state (settled / contested / last-3-yr)

- **Settled.** Veeva CRM/CLM logs slide-level dwell, navigation, reaction, channel, clickstream
  (verified, Veeva CRM Help, see F). Pharma marketing-to-physicians is a large spend category (HCP
  promotion is the majority share of pharma promotional budgets — CBO/MM+M, see F).
- **Contested / to-frame-carefully.** The claim that the dataset is "causally unanalyzed" is a
  *positioning* claim; vendors run heavy analytics on it (engagement optimization, NBA — Ch 3) but
  those are Rung-1. Frame as "no *deployed* engine answers the do-question," not "nobody has ever
  touched the data." (Aligns with TIKTOC Part 11 + central thesis.)
- **Last-3-yr.** The pharma NBA/orchestration market is consolidating fast — e.g., PharmaForceIQ
  announced acquisition of Aktana (Jan 7, 2026) for AI field NBA ([verify the dramatized "first
  optichannel-in-a-box" vendor phrasing if quoted]). Relevant because it shows the field doubling down
  on Rung-1 engagement orchestration, sharpening this book's contrast.
- **Key refs:** source essay; Veeva CRM Help; CBO promotional-spending publication; *The Book of Why*
  (ladder, for the causal frame this chapter foreshadows).

---

## E. Teaching considerations

- **Lead with the iPad scene, not the schema.** The hook is sensory (a physician, a tablet, slides
  flipping) — keep field names to a teaser (`Duration_vod`, `Reaction_vod`) and defer the dictionary
  to Ch 2. Over-loading Ch 1 with schema kills the hook.
- **Make the behavioral-vs-causal cut early and concretely.** The 47-second ambiguity is the single
  best teaching device; introduce it here as a *puzzle* (don't resolve it — resolution is Ch 3/7).
- **Resist over-claiming "nobody knows."** Sophisticated readers will object (vendors analyze this
  constantly). Pre-empt: the claim is about the *rung*, not the *effort*. This protects credibility and
  sets up Ch 3.
- **Connect to the reader's own thread immediately.** The Apply LO ("state your do-question") is the
  pebble-in-the-pond; have the Fellow write it down in Ch 1 even if they revise it by Ch 4.
- **Watch the partner-tone risk (TIKTOC Risk 7).** "Pharma has no idea" reads as a dunk on the named
  partner; soften to "the question hasn't been asked of this data" / pro-evidence framing.

---

## F. Library files (in `pantry/`, prefixed `_lib_`)

- `_lib_science-the-book-of-why-the-new-science-of-cause-and-effect.md` — Pearl & Mackenzie; the ladder
  (foreshadowed here, formalized Ch 3). Primary source for the behavioral-vs-causal distinction.
- `_lib_ai-applied-causal-inference-powered-by-ml-and-ai.md` — applied causal methods (DAGs, do, ML);
  background for "granularity ≠ identification."
- `_lib_math-causal-inference-mit-press-essential-knowledge-series.md` — accessible causal-inference
  primer; useful for the Rung-1 vs Rung-2 framing for a non-specialist Fellow.
- `_lib_math-naked-statistics-...md`, `_lib_math-the-art-of-statistics-...md`,
  `_lib_science-calling-bullshit-...md` — "correlation/dashboard ≠ cause" popular treatments, good for
  the failure-case framing and reader-level analogies.
- (Note: per task, these causal titles were already copied by the sibling-book run; reference, don't
  re-copy.)

**Web sources verified this pass:**
- Veeva CRM Help — Tracking CLM Call Clickstream Activity:
  https://crmhelp.veeva.com/doc/Content/CRM_topics/Multichannel/CLM/DefaultFunct/AnalyzingCLM/TrackCLM.htm
- Veeva CRM Help — Capturing Reactions to Presentations:
  https://crmhelp.veeva.com/doc/Content/CRM_topics/Multichannel/CLM/DefaultFunct/DisplayingPres/CaptureReactions.htm
- CBO — Promotional Spending for Prescription Drugs: https://www.cbo.gov/publication/25006
- MM+M 2023 Healthcare Marketers Trend Report (HCP = majority of promo spend):
  https://www.mmm-online.com/home/channel/features/healthcare-marketers-trend-report-2023-a-trim-off-the-top/

---

## G. Gaps & flags

- **[verify] Veeva ">80% of pharma field sales" market share.** Source essay asserts it; needs a current
  Veeva/industry citation. Veeva has also migrated CRM → "Vault CRM" (separate help domain
  `vaultcrmhelp.veeva.com` appeared in search) — note the platform-name shift; field names appear to
  carry over but confirm before Ch 2 drafting.
- **[verify] "15 years."** Plausible (iPad detailing + Veeva era ~2010–) but pin the start date to a
  source rather than asserting a round number.
- **[flag] "$40B promotion pathway" (appears in source essay / Ch 5).** Public figures: DTC ≈ $14B
  (2023); HCP-directed is the majority share but I did not find a clean "$40B" total this pass. Do NOT
  print "$40B" without a source — see Ch 3 notes G for the same flag.
- **[flag] Positioning claim "causally unanalyzed."** True only at the *deployed do-operator* level;
  must be phrased to survive an expert reader (see E, D).
- **[flag] Partner-tone (Risk 7).** "Nobody knows / has no idea" needs softening given the named
  Doceree partner.
