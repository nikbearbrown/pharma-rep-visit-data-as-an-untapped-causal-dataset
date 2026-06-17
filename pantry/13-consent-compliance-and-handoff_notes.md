# Chapter 13 Notes — Consent, Compliance, and the Handoff

*Research-gathering notes for Week 13 (FINAL — Living-Model prototype + handoff brief). Grounded in
`rep-visit-as-causal-dataset_source.md` ("Consent architecture caveat", "Production / framing notes"),
the TIKTOC (Week 13 + Part 3 IP-firewall + Part 15 Risk register), the Living Model framework, and the
library/web research below. NOT chapter prose. Sourced or `[verify]`-flagged. Regulatory material is
**flagged and routed to counsel, not adjudicated** (Part 14: legal authority is out of scope).*
*(Supersedes the earlier stub `13-consent-compliance-and-the-handoff_notes.md`.)*

**Chapter scope (from TIKTOC Week 13):** consent/selection ethics (opt-out collider; FCI; drift
monitoring of the opt-out rate); the pharma regulatory line (educational pathway = the only legally
drivable one; PhRMA Code, FDA OPDP, Sunshine Act/Open Payments — flagged + routed to counsel); the
public-data/IP firewall + synthetic-data plan; the four-stage research→product pipeline and the
Fellows→partner (Doceree) handoff brief.

---

## SECTION A — FOUNDATIONS, MISCONCEPTION, WORKED EXAMPLE, SOURCE

### A.1 Consent as a collider / selection structure

Consent is **not just legal metadata; it changes what is observed.** Veeva `CLM_EXPLICIT_OPT_IN_vod`
governs logging; opt-out either suppresses the clickstream or anonymizes it (`CLM_OPT_OUT_BEHAVIOR_vod`
0/1 strips NPI from `Multichannel_Activity_vod`). If opt-out is **systematically correlated with
physician characteristics** (privacy-conscious, industry-skeptical, institutional limits) — which is
almost certain — then the observed dataset is a **selected sample**. Conditioning your analysis on
"physicians we have data for" is conditioning on a **collider / Berkson selection node** on the sampling
frame.

The fix (from the source essay): **put the consent/observation mechanism in the causal graph** and use
a **latent-variable-aware discovery method — FCI (Fast Causal Inference)** — rather than PC/GES, because
FCI is built to handle latent confounders and selection variables. The Living Model's **drift monitor
watches opt-out-rate shifts**, because a change in who opts out changes the bias structure (and silently
invalidates a model trained on the old consenting subsample).

**Why this is the same family as the Ch 6 collider:** Ch 6 taught the *post-treatment* collider
(dwell/reaction). Ch 13's consent collider is the *selection* collider — different position, same
structural disease (conditioning opens a non-causal path / biases the estimate). The book treats them
as two faces of one discipline.

### A.2 THE CENTRAL MISCONCEPTION

> **"Analyze the consenting physicians and generalize the model to everyone."** The consenting subsample
> is selected on traits plausibly correlated with responsiveness; a model fit on opt-ins can be
> *confidently wrong* when applied to opt-out-like physicians, who differ in institutional policy,
> rep relationship, and disposition. (TIKTOC Week 13 opening: "a model that works on the consenting
> subsample and silently generalizes wrong.") This is the Ch 13 analog of TIKTOC Part 2 misconception
> #2 (post-treatment collider) carried into selection.

### A.3 The regulatory line: the educational pathway is the only legally drivable one

The Ch 5 three-pathway graph (M1 educational / M2 relationship-maintenance / M3 reciprocity) has
**different regulatory status per pathway.** The book's position (flagged, routed to counsel — NOT
adjudicated):
- **M1 educational** — substantive, on-label, fair-balanced clinical information — is the **only
  pathway a Living Model should be *built to drive*.** Promotion must be truthful, non-misleading,
  fairly balanced, and consistent with FDA-approved labeling (FDA OPDP remit).
- **M3 reciprocity** (meals/gifts) — the $40B pathway — is regulatorily fraught (Sunshine Act
  disclosure; PhRMA Code limits on meals/gifts/entertainment). A model that *optimizes* the reciprocity
  pathway is optimizing exactly the thing regulators and the public scrutinize. **Flag and route to
  counsel.**
- **M2 relationship-maintenance** (samples, co-pay cards) — mixed; sits in a gray zone.

This is why Ch 12's portfolio optimization carries an **educational-pathway-only gate**: a high-CATE
physician whose effect runs through reciprocity is *excluded from the drivable target set*, not because
the effect isn't real but because it isn't legally/ethically drivable. The pathway decomposition (Ch 5,
Ch 11) is what makes this gate *operational* rather than aspirational.

**Three regulatory instruments to name (route to counsel, don't adjudicate):**
1. **PhRMA Code on Interactions with HCPs** (voluntary industry self-regulation; revisions effective
   Jan 1, 2022 `[verify current]`): meals must be modest, occasional, tied to a bona fide
   educational/scientific presentation, generally in-office/in-hospital; entertainment/recreation
   banned; alcohol banned at company events; tightened speaker-program rules; non-educational gifts
   prohibited.
2. **FDA OPDP** (Office of Prescription Drug Promotion, in CDER): regulates Rx promotion to be truthful,
   non-misleading, fairly balanced, on-label. Enforcement via **Untitled Letters** and **Warning
   Letters**.
3. **Physician Payments Sunshine Act → CMS Open Payments**: mandatory disclosure of manufacturer
   payments/transfers-of-value (meals, travel, consulting, speaker fees, ownership). The reciprocity
   pathway is *publicly visible* because of this — which is both the data source for Project 2 and the
   reason driving M3 is reputationally radioactive.

**Off-label / First-Amendment landscape (confirm, don't adjudicate — counsel call):** manufacturer
promotion must be on-label/substantiated; off-label promotion is restricted under FDCA misbranding, but
there is a live commercial-speech tension — *United States v. Caronia* (2d Cir. 2012) and *Amarin v.
FDA* (S.D.N.Y. 2015) protect *truthful, non-misleading* off-label speech in the 2nd Circuit; the
national status is unsettled. A "scientific/educational exchange" lane exists (unsolicited-request
responses, peer-reviewed reprints, CME) under condition-laden FDA guidance. **All of this is `[verify]`
+ route to counsel — the book flags the landscape, the partner's legal team decides.**

### A.4 The public-data / IP firewall + synthetic-data plan

(TIKTOC Part 3, the defining constraint.) The core telemetry is Veeva CRM data the **partner owns**.
The firewall:
- **In the public book/repo:** methods, the **synthetic rep-visit dataset (known ground truth)**, and
  the **public-data project** (Open Payments meals + Medicare Part D — Project 2). Nothing proprietary.
- **In `private/` (gitignored) / partner-internal:** real CRM field mappings, partner performance
  deltas, proprietary targeting methods. The **partner replicates the methods on real CRM data
  internally.**
- **Synthetic-first rationale:** because the real data is proprietary, the book ships a synthetic
  dataset with known ground-truth structure so Fellows can verify their methods *recover* it. The meals
  project uses real public data. (TIKTOC Risk 2 mitigation.)

### A.5 The four-stage research→product pipeline + the handoff brief

The handoff is **governance, not a presentation.** The four-stage pipeline (research → product):
1. **Research / identification** — state the do-question + identification strategy (IV / DiD / forest /
   DML), falsification-first.
2. **Synthetic validation** — show the method recovers known ground truth on the synthetic dataset.
3. **Public-data demonstration** — run on Open Payments + Part D (Project 2), reach a stated evidence
   level.
4. **Partner replication / product discovery** — partner runs the validated method on proprietary CRM
   data internally (behind the firewall); only then does product discovery begin.

The **Fellows→Doceree handoff brief** (the terminal deliverable, ~one page) must state, per finding:
**test / build / kill** recommendation; **evidence level**; **assumptions** (esp. IV independence,
no-post-treatment-conditioning, consent-selection handling); **compliance flags** (which pathway; route
to counsel items); **patient-welfare check**; **what proprietary replication is required**; and
**kill criteria**. Provenance + monitoring + human accountability are included *from the start*, per
the Living Model governance posture (the boundary between what the model decides and what a human
decides must be explicit — living-models lib).

**Worked example sketch (process incl. dead end → lesson → limit):**
- Fellow shows the meals causal forest (Project 2) recovers synthetic ground truth (Stage 2), then runs
  on public Open Payments + Part D (Stage 3), reaching, say, evidence level 2 (associational-plus-
  heterogeneity, not clean identification).
- **Dead end to teach:** "the public prototype works, so the product is ready." No — public Open-Payments
  evidence is observational; Stage 4 partner replication on the CRM panel (with the cohort-rollout IV) is
  required before product discovery. Second dead end: a Fellow writes public notes containing partner
  field mappings or performance deltas → **IP leakage** (firewall breach).
- **Lesson:** the handoff *gates*, it doesn't *bless*; evidence level + kill criteria are the product.
- **Limit:** the consent-selected sample caps external validity even after replication; the drift
  monitor (opt-out rate) is a permanent obligation, not a one-time check.

**Primary source anchor:** `rep-visit-as-causal-dataset_source.md`, "Consent architecture caveat" +
"Production / framing notes" (the Graham Wilkinson / Doceree pitch framing).

---

## SECTION B — EXAMPLES + FAILURE CASE

**Positive examples / evidence:**
- **Open Payments as a public, downloadable disclosure dataset** (CMS; mandated by Sunshine Act,
  ACA 2010 §6002; 42 U.S.C. §1320a-7h): general payments (meals etc.), research payments, ownership.
  Confirmed public + bulk-downloadable at openpaymentsdata.cms.gov. This is what makes Project 2
  firewall-safe.
- **The educational-pathway gate in action:** a message effect is productizable *only if* it routes
  through defensible on-label educational content (M1), not reciprocity (M3). The pathway decomposition
  (Ch 5/11) is what lets the brief say "drivable" vs "flag to counsel."
- **Synthetic-to-proprietary replication:** Fellow demonstrates recovery on synthetic ground truth →
  partner tests whether real CRM data has comparable quasi-experimental variation. The firewall pattern
  that lets the book be public while the product stays proprietary.

**The canonical FAILURE CASE (chapter opens on it):**
A model **fit on the consenting subsample that silently generalizes wrong** to opt-out-like physicians.
It validates beautifully in-sample (on opt-ins) and is deployed across the full panel, where the
opt-out-correlated traits (skepticism, institutional restriction) make its recommendations
systematically off — and because opt-out is *invisible by construction* (the data is suppressed), the
failure is hard to even detect. The drift monitor on opt-out rate is the only early warning.

Secondary failures: (1) **IP leakage** — public notes carrying partner-specific field mappings or
performance deltas (firewall breach). (2) **Driving the reciprocity pathway** — a model that optimizes
M3 because that's where the biggest CATE is, walking straight into the Sunshine-Act-visible,
PhRMA-Code-restricted zone. (3) **Treating the handoff as a demo** — shipping a "working prototype"
without evidence level, assumptions, kill criteria, or the Stage-4 replication requirement.

---

## SECTION C — CONNECTIONS

- **← Ch 6 (collider trap):** the consent collider is the *selection* sibling of Ch 6's post-treatment
  collider — same structural disease, revisited here with FCI as the latent/selection-aware tool.
- **← Ch 5 (three-pathway graph):** the regulatory line is the pathway graph's ethical/legal annotation;
  M1 drivable, M3 flagged. This chapter cashes out Ch 5's "each pathway has different regulatory status."
- **← Ch 7 (Markov equivalence / FCI):** FCI extends the equivalence-class machinery to latent +
  selection variables — the formal tool for the consent node.
- **← Ch 9 (Living Model / drift):** the opt-out-rate drift monitor is a concrete instance of the Ch 9
  drift-detection requirement; governance + provenance + human-accountability boundary are the Living
  Model posture applied to the handoff.
- **← Ch 11 (firewall made concrete):** Ch 11 first hits the public-vs-proprietary wall (Project 2
  public, Projects 1 & 3 not); Ch 13 formalizes it as the firewall + four-stage pipeline.
- **← Ch 12 (persuadables):** the educational-pathway gate and patient-welfare check are *constraints in
  Ch 12's optimization*; Sleeping Dogs ↔ opt-out-prone physicians overlap.
- **→ Terminal deliverable:** the whole book lands here — prototype (synthetic/public) + handoff brief.
- **Companion (governance):** living-models lib on the decide/authorize boundary, provenance, monitoring.

---

## SECTION D — CURRENT STATE / REFS / LAST-3-YR

**Consent / selection / FCI:**
- FCI (Fast Causal Inference) for latent-confounder + selection-variable causal discovery — Spirtes,
  Glymour, Scheines; and Zhang (2008) on FCI completeness with selection bias `[verify exact cite]`.
  (Selection-bias overview + FCI background per source essay.)
- The consent-as-collider / Berkson selection framing — `rep-visit-as-causal-dataset_source.md`
  ("Consent architecture caveat").

**Open Payments / Sunshine Act (CMS, verified):**
- CMS **Open Payments** program — mandated by the **Physician Payments Sunshine Act**, ACA 2010 §6002;
  statute **42 U.S.C. §1320a-7h**; regs **42 C.F.R. Part 403**. Public + downloadable
  (openpaymentsdata.cms.gov). Reports general payments (incl. meals), research payments, ownership.
  Covered recipients expanded in 2021 to add PAs, NPs, CNSs, CRNAs/AAs, certified nurse-midwives.
- **De-minimis reporting threshold** (CPI-adjusted annually; sub-threshold per-item payments need not
  be reported unless the aggregate annual threshold is exceeded):
  - 2020: $10.49 / $109.69 aggregate; 2021: $11.04 / $110.40 aggregate (CMS, verified).
  - **2025: ~$13.46 per item / ~$134.54 aggregate `[verify — secondary sources (qordata / law-firm
    advisories); not confirmed directly from a CMS page; CMS "Data Collection for Reporting Entities"
    renders the table dynamically].`**
  - **2026 figure: `[verify — not located / may not yet be released].`** Canonical source = the CMS
    Data Collection page.

**Pharma promotional regulation (verified, route to counsel):**
- **PhRMA Code on Interactions with HCPs** — voluntary; revisions effective Jan 1, 2022 `[verify no
  newer revision]`. URL: phrma.org statement + Code PDF.
- **FDA OPDP** — Rx promotion: truthful, non-misleading, fair balance, on-label; Untitled/Warning
  letters. URL: fda.gov OPDP overview.
- **Off-label / First Amendment:** *United States v. Caronia*, 703 F.3d 149 (2d Cir. 2012); *Amarin
  Pharma v. FDA* (S.D.N.Y. Aug. 7, 2015). Truthful non-misleading off-label speech protected in 2d Cir.;
  national status unsettled. Scientific-exchange lane under FDA guidance `[verify specific guidance].`
  **Route to counsel — do not adjudicate.**

**Evidence base for the pathway/welfare argument (shared with Ch 11):** DeJong 2016 (meals→prescribing,
associational); Larkin 2017 (DiD on AMC detailing restrictions — the causal-design counterpoint, shows
restricting detailing shifts prescribing toward generics); Yeh 2016; Mitchell 2021 systematic review
("consistent with but not definitively proving" causation).

**Recent developments:** healthcare-AI governance increasingly expects provenance, monitoring, and
explicit human accountability — fold into the handoff from the start. FDA signaling renewed scrutiny of
DTC/social-media drug advertising as of late 2025 `[verify]` (color, not load-bearing — this book is
HCP-facing, DTC is out of scope).

**Aging risk (high here):** Open-Payments thresholds (annual CPI), PhRMA Code revisions, Veeva consent
field names, and off-label case law all drift. Re-verify every cycle; the *structural* arguments
(consent collider, pathway gate, firewall) are stable.

---

## SECTION E — TEACHING

- **Open on the silent-generalization failure.** The model that validates on opt-ins and fails on
  opt-outs — and the fact that you *can't see* the failure because the opt-out data is suppressed.
- **Analogy:** a lab result is not a prescription — it needs interpretation, contraindications, and
  authorization. The handoff brief is the interpretation + contraindications + authorization layer, not
  the result itself.
- **Drill "the handoff is governance, not a demo."** Where Fellows get stuck: they see the handoff as a
  presentation of a working model. Reframe: it is a test/build/kill recommendation with evidence level,
  assumptions, compliance flags, welfare check, and kill criteria.
- **Make the firewall concrete with a leakage drill:** show a "bad" public note containing partner field
  mappings; have Fellows redact it. The firewall is a *habit*, not a policy paragraph.
- **Route, don't adjudicate.** Teach Fellows to *flag* regulatory questions (which pathway? on-label?
  meal threshold?) and route to counsel — not to render legal opinions. This is itself a professional
  competency (and TIKTOC Part 14 scope discipline).
- **Pathway gate as the ethical spine:** the educational-pathway-only rule is what makes the whole
  enterprise defensible; teach it as the feature, not the constraint.
- **Bloom's:** (Evaluate) put consent in the graph + choose FCI; (Evaluate) apply the regulatory line +
  patient-welfare check; (Create) write the product-handoff brief (test/build/kill) under the IP
  firewall.
- **Exercise:** write a one-page handoff with evidence level, assumptions, compliance flags,
  patient-welfare check, kill criteria, and the Stage-4 replication requirement.

---

## SECTION F — LIBRARY FILES

Relevant `_lib_` files in `pantry/`:
- **`_lib_math-weapons-of-math-destruction-how-big-data-increases-inequality-and-threatens-dem.md`**
  — O'Neil; the fairness/accountability/opaque-confident-model lens (22 hits) — directly supports the
  silent-generalization failure case and the "model that works on a selected subsample and harms the
  rest" argument. Core for this chapter.
- **`_lib_science-the-book-of-why-the-new-science-of-cause-and-effect.md`** — Pearl on colliders +
  selection bias (the consent collider's foundation).
- **`_lib_ai-applied-causal-inference-powered-by-ml-and-ai.md`** — causal-discovery / selection
  background (FCI family).
- **`_lib_science-calling-bullshit-the-art-of-skepticism-in-a-data-driven-world.md`** and
  **`_lib_math-the-art-of-statistics-...md`** — selection-bias / generalizability skepticism lens.
- **`_lib_business-flawless-consulting-4th-edition.md`** — the *handoff* as a contracting/governance act
  (test/build/kill, who-decides-what) — useful for the handoff-brief framing.
- **`_lib_economics-the-lean-startup-...md`** — the four-stage pipeline / test-build-kill validated-
  learning framing.

**Library-scan finding:** `fairness`, `accountability`, and `weapons of math destruction` are
well-represented in the library (WMD copied as `_lib_`; also in `psychology-noise`,
`ai-the-alignment-problem`). **No library book covers Open Payments or the Sunshine Act** — those are
entirely web-sourced (Section D). The regulatory + consent material is the most web-dependent of the
three chapters; the library supplies the *ethics/skepticism lens*, the web supplies the *legal/program
facts* (all `[verify]` + route to counsel).

---

## SECTION G — GAPS / FLAGS

- **★ RISK-1 BLOCKER (TIKTOC Part 15 #1; the book's gating risk).** *Proprietary-data access + partner
  framing + consent ethics are unresolved.* This chapter is where all three converge and where the
  blocker is most visible:
  - **Data access:** Stage 4 (partner replication) presupposes a settled data-access + IP-firewall
    agreement with Graham Wilkinson / Doceree. Until that exists, the handoff brief's "what proprietary
    replication is required" is hypothetical. **Owner: Nik + Graham; deadline: before Act III** (TIKTOC
    Part 16 #1).
  - **Partner framing:** the book's critical-of-pharma tone vs. a *named* partner (Risk 7) — Ch 5/12/13
    need partner sign-off; the reciprocity-pathway critique must be framed pro-evidence, not anti-partner.
  - **Consent ethics:** whether the consent-selection ethics + welfare check are adequately load-bearing
    (Risk 5) — the chapter must make consent pervasive, not an appendix. Recommend per-chapter welfare
    check (TIKTOC Part 16 #5).
  - **This blocker also gates the synthetic-data plan** (Risk 2): the synthetic dataset's ground-truth
    structure depends on what the partner agreement permits modeling.
- **[flag — route to counsel, do not adjudicate.]** Every regulatory claim (PhRMA Code current revision,
  FDA OPDP enforcement, Sunshine Act thresholds, off-label/Caronia/Amarin status) is flagged for legal
  review. The book *flags the landscape*; the partner's counsel decides. (TIKTOC Part 14: legal authority
  out of scope.)
- **[verify]** current Open Payments de-minimis threshold (2025 ≈ $13.46/$134.54 from secondary sources;
  2026 not located); PhRMA Code current revision; FCI-with-selection exact citations; FDA scientific-
  exchange guidance documents by name.
- **Gap — opt-out data is invisible by construction.** You cannot directly measure how the opt-out
  population differs (their data is suppressed/anonymized). FCI + the drift monitor are partial
  mitigations, not solutions; external validity beyond the consenting frame is fundamentally capped.
  Be honest that this is a structural limit, not a bug to be engineered away.
- **Gap — "evidence level" scale is undefined.** The handoff brief references evidence levels (e.g.,
  "level 2") but the book needs a defined evidence-level rubric (associational / heterogeneity /
  quasi-experimental-identified / replicated). Author to specify `[verify / design decision]`.
