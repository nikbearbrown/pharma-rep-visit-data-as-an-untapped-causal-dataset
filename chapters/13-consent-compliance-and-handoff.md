# Chapter 13: Consent, Compliance, and the Handoff

## 1. Overview

This is the last chapter, and it is the one where the book stops being a methods course and becomes a question of whether you are *allowed* to build the thing you now know how to build.

You have a Living Model. You can identify a message→prescribing effect from the training-cohort natural experiment (Chapter 4), keep the colliders out of it (Chapter 6), orient the edges the data cannot (Chapters 7–8), parameterize the SCM (Chapter 9), run the three projects (Chapters 10–11), and rank physicians by causal responsiveness (Chapter 12). What you do *not* yet have is the right to deploy any of it — because three things have been deferred since Chapter 2 and now all come due at once: **consent is a selection structure that quietly invalidates a model trained on the people who opted in; the regulatory line makes the educational pathway the only one a model should be built to drive; and the data that would prove all of this on real physicians is proprietary, behind a firewall you may not cross.**

By the end of this chapter you will be able to put the consent mechanism in the causal graph and say why FCI — not PC or GES — is the right discovery tool for it; apply the educational-pathway-only regulatory line and a patient-welfare check, *routing every legal question to counsel rather than answering it*; respect the public-data/IP firewall in practice; and write the terminal deliverable — a **product-handoff brief** with a test/build/kill recommendation per finding — alongside a description of the **Living-Model prototype** the book has been building toward.

One thing this chapter will *not* do is pretend the path is clear. The single largest risk to this entire project — proprietary-data access, partner framing, and consent ethics, all unresolved — converges here. I will name it as an open blocker in the handoff brief rather than paper over it. A handoff that hides its own gating risk is not a handoff; it is a sales pitch.

## 2. Learning objectives

- **(Evaluate)** Place the consent/opt-out mechanism in the causal graph as a selection collider, explain why a model trained on consenters silently generalizes wrong, and justify FCI over PC/GES.
- **(Evaluate)** Apply the regulatory line — educational pathway as the only legally drivable one — and a mandatory patient-welfare check, *flagging and routing* PhRMA Code / FDA OPDP / Sunshine Act questions to counsel rather than adjudicating them.
- **(Create)** Write the product-handoff brief (test/build/kill, with evidence level, assumptions, compliance flags, welfare check, kill criteria, and the proprietary-replication requirement) under the IP firewall, and describe the final Living-Model prototype.

## 3. Opening case (failure-first)

A Fellow builds a beautiful model. The meals causal forest recovers the synthetic ground truth exactly; on real public Open Payments data it produces a clean, heterogeneous CATE surface. The persuadables ranking from Chapter 12 has a higher Qini than the propensity list. Cross-validation is immaculate. The Fellow writes: *the prototype works; the product is ready.*

It is not, and the reason is invisible by construction.

Every physician in that training data *consented to being logged*. The ones who opted out — privacy-conscious, industry-skeptical, working under institutional restrictions on rep contact — are not in the data at all; their clickstream was suppressed and their NPI stripped. The model was fit on a *selected sample*. When it is deployed across the full physician panel, including the opt-out-like physicians, its recommendations are systematically off — because the very traits that make a physician opt out (skepticism, institutional limits, a different relationship with reps) are plausibly the same traits that change how she responds to a message.

And here is the trap that makes this worse than an ordinary generalization error: **you cannot see the failure.** The opt-out data is suppressed, so there is no held-out set of opt-outs to validate against. The model validates beautifully on the only people it can see — the consenters — and fails silently on the people it cannot. The cross-validation that looked immaculate was immaculate *within the selected sample* and told you nothing about the population you deployed to.

The only early warning is structural: a **drift monitor watching the opt-out rate**, because a change in who opts out changes the bias. This chapter is about building a model — and a handoff — that takes that failure seriously instead of being surprised by it.

## 4. Core sections

### 4.1 Consent as a collider / selection structure

Consent is not just legal metadata. *It changes what is observed*, and that makes it a causal-graph object, not a checkbox.

In Veeva, `CLM_EXPLICIT_OPT_IN_vod` governs logging. Opt-out either suppresses the clickstream or anonymizes it: `CLM_OPT_OUT_BEHAVIOR_vod` (0/1) strips the NPI from `Multichannel_Activity_vod`. (These field names are dated mid-2026 and drift; verify against the partner's current schema.) If opt-out is **systematically correlated with physician characteristics** — and privacy-consciousness, industry-skepticism, and institutional contact policies make that nearly certain — then "the physicians we have data for" is a *selected* set.

Conditioning your analysis on that set is **conditioning on a collider** — specifically a Berkson selection node on the sampling frame. It is the same structural disease you met in Chapter 6, where dwell time and reaction scores were *post-treatment* colliders. Consent is a *selection* collider: different position in the graph, identical pathology — conditioning on it opens a non-causal path and biases the estimate. The book treats them as two faces of one discipline. Whatever you learned to fear about controlling for engagement, fear the same way about analyzing only the consenters.

The fix, from the source essay: **put the consent/observation mechanism in the causal graph as an explicit node**, and discover structure with a method that is *built for latent confounders and selection variables*.

### 4.2 Why FCI, not PC or GES

Chapter 7 taught Markov equivalence: observational data identifies an equivalence *class*, not a single graph. The standard discovery algorithms — PC and GES — assume *causal sufficiency*: no unmeasured confounders and no selection bias. That assumption is exactly what the consent structure violates. There *is* an unmeasured selection mechanism, and it is the whole problem.

**FCI (Fast Causal Inference)** is the latent-variable-aware extension. It does not assume causal sufficiency; it is designed to return a structure (a PAG — partial ancestral graph) that *admits* unobserved confounders and selection variables, marking edges it cannot fully orient because of them. For the consent node, that honesty is the point: FCI will not pretend it has identified a clean DAG when a selection collider is in play. (FCI traces to Spirtes, Glymour & Scheines; Zhang 2008 extended its completeness to the selection-bias case `[verify exact cite]`.) Use FCI here. PC/GES would hand you a confident graph that is confidently wrong.

The Living Model's **drift monitor watches opt-out-rate shifts** as a first-class signal — because if *who* opts out changes, the bias structure changes, and a model validated under the old consenting frame is silently invalidated under the new one. The drift monitor is not a nice-to-have; it is the only instrument that can see the opening case's failure coming.

### 4.3 The regulatory line: the educational pathway is the only legally drivable one

The Chapter 5 graph had three pathways from message to prescribing — M1 educational, M2 relationship-maintenance, M3 reciprocity. Chapter 5 noted they carry *different regulatory status*. This chapter cashes that out, and does so under a hard discipline: **the book flags the landscape; the partner's counsel decides. Nothing here is legal advice.**

The book's *position* (flagged, routed to counsel, not adjudicated):

- **M1 educational** — substantive, on-label, fair-balanced clinical information — is the **only pathway a Living Model should be built to drive.** Promotion that is truthful, non-misleading, fairly balanced, and consistent with FDA-approved labeling sits inside what the FDA Office of Prescription Drug Promotion (OPDP) regulates.
- **M3 reciprocity** — meals and gifts, the ~$40B pathway — is regulatorily fraught: payments are disclosed under the Sunshine Act and the PhRMA Code limits meals/gifts/entertainment. **A model that *optimizes* the reciprocity pathway is optimizing precisely the thing regulators and the public scrutinize.** Flag and route to counsel.
- **M2 relationship-maintenance** — samples, co-pay cards — sits in a gray zone; mixed status, flag it.

This is why Chapter 12's optimization carried an **educational-pathway-only gate**: a high-CATE physician whose effect runs through reciprocity is *excluded from the drivable target set* — not because the effect isn't real, but because it isn't legally or ethically drivable. The pathway decomposition from Chapters 5 and 11 is what makes that gate *operational* instead of aspirational. Teach the gate as the feature, not the constraint: the educational-pathway-only rule is what makes the whole enterprise defensible.

**Three instruments to name (route to counsel; do not adjudicate):**

1. **PhRMA Code on Interactions with HCPs** — voluntary industry self-regulation; the revisions effective January 1, 2022 tightened meals (modest, occasional, tied to a bona fide educational presentation, generally in-office), banned entertainment/recreation and alcohol at company events, and tightened speaker-program rules. `[verify no newer revision]`
2. **FDA OPDP** (Office of Prescription Drug Promotion, within CDER) — regulates prescription-drug promotion to be truthful, non-misleading, fairly balanced, and on-label; enforces via Untitled Letters and Warning Letters.
3. **Physician Payments Sunshine Act → CMS Open Payments** — mandatory disclosure of manufacturer payments and transfers of value (meals, travel, consulting, speaker fees, ownership); statute 42 U.S.C. §1320a-7h, regulations 42 C.F.R. Part 403, enacted under ACA 2010 §6002. The reciprocity pathway is *publicly visible* because of this Act — which is simultaneously the data source for the meals project (Chapter 11) and the reason driving M3 is reputationally radioactive. The de-minimis reporting threshold is CPI-adjusted annually (e.g., 2021: ~$11.04 per item / ~$110.40 aggregate, CMS-verified; 2025 figures circulate from secondary advisories at ~$13.46 / ~$134.54 `[verify — not confirmed directly from a CMS page]`; the 2026 figure was not located `[verify]`).

**Off-label / First-Amendment landscape (confirm; do not adjudicate — counsel call).** Manufacturer promotion must be on-label and substantiated; off-label promotion is restricted under FDCA misbranding provisions, but there is a live commercial-speech tension. *United States v. Caronia*, 703 F.3d 149 (2d Cir. 2012), and *Amarin Pharma v. FDA* (S.D.N.Y. 2015) protect *truthful, non-misleading* off-label speech in the Second Circuit; the national status is unsettled. A "scientific/educational exchange" lane exists (unsolicited-request responses, peer-reviewed reprints, CME) under condition-laden FDA guidance `[verify specific guidance]`. **All of this is `[verify]` + route to counsel.** Your job as a Fellow is to *flag* the regulatory question — which pathway? on-label? above the meal threshold? — and hand it to legal, not to render an opinion. Routing rather than adjudicating is itself a professional competency.

### 4.4 The public-data / IP firewall and the synthetic-data plan

The core telemetry is Veeva CRM data the **partner owns** (the partner here is Doceree). The firewall is the structural rule that lets the book be fully public while the product stays proprietary:

- **In the public book/repo:** methods; the **synthetic rep-visit dataset** with known ground-truth structure; and the **public-data project** — Open Payments meals plus Medicare Part D (the Chapter 11 meals project). Nothing proprietary.
- **In `private/` (gitignored) / partner-internal:** real CRM field mappings, partner performance deltas, proprietary targeting methods. The **partner replicates the validated methods on real CRM data internally.**
- **Synthetic-first rationale:** because the real data is proprietary, the book ships a synthetic dataset with known ground truth so Fellows can verify their methods *recover* it. The meals project uses real public data.

The firewall is a *habit*, not a policy paragraph. The failure mode is a Fellow who writes a public note containing a partner-specific field mapping or a real performance delta — IP leakage, a firewall breach. Treat redaction as a reflex: anything that only makes sense because you saw the partner's data goes in `private/`.

### 4.5 The four-stage research→product pipeline and the handoff brief

The handoff is **governance, not a presentation.** Where Fellows get stuck is thinking the handoff is a demo of a working model. Reframe it: the handoff is a *test/build/kill recommendation* with evidence level, assumptions, compliance flags, a welfare check, kill criteria, and a statement of what proprietary replication is still required. (Useful framing: a lab result is not a prescription — it needs interpretation, contraindications, and authorization. The handoff brief is the interpretation-plus-contraindications-plus-authorization layer, not the result.)

The four-stage pipeline:

1. **Research / identification** — state the do-question and the identification strategy (IV / DiD / forest / DML), falsification-first.
2. **Synthetic validation** — show the method recovers known ground truth on the synthetic dataset.
3. **Public-data demonstration** — run on Open Payments + Part D (the meals project), reaching a *stated evidence level*.
4. **Partner replication / product discovery** — the partner runs the validated method on proprietary CRM data internally, behind the firewall. *Only then* does product discovery begin.

The **handoff brief**, per finding, must state: the **test/build/kill** recommendation; the **evidence level**; the **assumptions** (especially IV independence, no post-treatment conditioning, and consent-selection handling); the **compliance flags** (which pathway; which items route to counsel); the **patient-welfare check**; **what proprietary replication is required** (Stage 4); and the **kill criteria**. Provenance, monitoring, and an explicit human-accountability boundary are included *from the start* — the Living Model governance posture: the line between what the model decides and what a human decides must be written down, not assumed.

One honest gap: the brief refers to "evidence levels," but the book needs a *defined* rubric. Use a four-rung scale and state it explicitly — **(1) associational; (2) associational-plus-heterogeneity; (3) quasi-experimentally identified; (4) replicated on partner data** — so "level 2" means something the partner can audit. `[design decision]`

## 5. Worked example on this dataset (process, including dead ends)

We walk the meals causal forest (the public-data project) through the pipeline.

**Stage 2 — synthetic validation.** The Fellow shows the forest recovers the synthetic ground truth: the physicians we *built* to be reciprocity-responsive are the ones it flags. Good — the method works.

**Stage 3 — public-data demonstration.** Run the forest on real public Open Payments meals + Part D prescribing. It produces a heterogeneous CATE surface and reaches, say, **evidence level 2** — associational-plus-heterogeneity. Note the ceiling: Open Payments is *observational*. There is no cohort-rollout instrument in public data, so this is not clean identification.

**Dead end one.** The Fellow writes: *the public prototype works, so the product is ready.* No. Public Open Payments evidence is observational; the cohort-rollout IV (Chapter 4) that would identify a clean effect lives only in the partner's CRM data. Stage 4 replication is *required* before product discovery. The prototype demonstrates the *method*; it does not bless the *product*.

**Dead end two.** A Fellow drafts the public handoff note and, to make it concrete, pastes in a real partner field mapping and a performance delta from a conversation with the partner. **IP leakage** — a firewall breach. The fix is reflexive redaction: those lines go to `private/`, and the public note describes the *method* and the *synthetic/public* results only.

**Lesson:** the handoff *gates*, it does not *bless*. The evidence level and the kill criteria *are* the product. A brief that says "level 2, observational, replicate before building, kill if the cohort-rollout independence test fails" is more valuable than a brief that says "it works."

**Limit:** even after Stage 4 replication, the consent-selected sample caps external validity. The model is honest only about the consenting frame; the opt-out population remains invisible by construction. The drift monitor on opt-out rate is a *permanent obligation*, not a one-time check — and FCI plus the drift monitor are partial mitigations, not a solution. Be honest that this is a structural limit, not a bug to be engineered away.

## 6. Common misconceptions

- **"Analyze the consenting physicians and generalize the model to everyone."** The consenting subsample is selected on traits plausibly correlated with responsiveness; a model fit on opt-ins can be *confidently wrong* on opt-out-like physicians. This is the carried-forward sibling of the post-treatment-collider misconception — selection, not post-treatment, but the same disease.
- **"Consent missingness is ignorable."** False. It is a Berkson/selection collider on the sampling frame. Model it (FCI), monitor it (opt-out-rate drift); do not assume it away.
- **"The handoff is a presentation of a working model."** No — it is governance: a test/build/kill recommendation with evidence level, assumptions, compliance flags, welfare check, and kill criteria.
- **"Drive the pathway with the biggest CATE."** If the biggest CATE runs through reciprocity (M3), driving it walks straight into the Sunshine-Act-visible, PhRMA-Code-restricted zone. Drive only the educational pathway.
- **"We can render the legal call ourselves if we read the rules."** No — flag and route to counsel. The book maps the landscape; the partner's legal team decides.

## 7. Evidence check + Consent/compliance & patient-welfare check

**Evidence check.**

- *Independent / verified facts:* CMS Open Payments and the Sunshine Act (42 U.S.C. §1320a-7h; 42 C.F.R. Part 403; ACA 2010 §6002) — public, bulk-downloadable, mandatory disclosure; covered recipients expanded in 2021. This is what makes the meals project firewall-safe.
- *Method background:* FCI for latent-confounder and selection-variable discovery (Spirtes-Glymour-Scheines; Zhang 2008 `[verify exact cite]`). The consent-as-collider framing follows the source essay.
- *Regulatory (verified existence; status route-to-counsel):* PhRMA Code (revisions effective Jan 1, 2022 `[verify current]`); FDA OPDP (Untitled/Warning Letters); *Caronia* (2d Cir. 2012) and *Amarin* (S.D.N.Y. 2015). **Every regulatory claim is flagged for legal review.**
- *Contested / `[verify]`:* the 2025 Open Payments de-minimis threshold (~$13.46/$134.54, secondary sources only); the 2026 figure (not located); FDA scientific-exchange guidance by name. The pathway/welfare evidence base (DeJong 2016 associational; Larkin 2017 DiD on AMC detailing restrictions; Mitchell 2021 review — "consistent with but not definitively proving" causation) shows meals→prescribing is association-strong, causal-contested.
- *Out of scope:* DTC / patient-facing advertising and any adjudication of legal authority — both flagged and routed, per the book's scope discipline.

**Consent/compliance & patient-welfare check.** This is the chapter where the check stops being a section and becomes the whole point.

1. **Consent.** The model must place consent in the graph (FCI), state the consent-selection assumption in the handoff, and ship with an opt-out-rate drift monitor. External validity is *capped at the consenting frame* — state that limit, do not hide it.
2. **Compliance.** Each finding carries a pathway label and explicit route-to-counsel flags for any M2/M3 exposure or off-label question. The educational-pathway-only gate is a hard constraint on what is drivable.
3. **Patient welfare — mandatory, not optional.** A caused prescription is a good outcome only if it is the right drug for that patient. The handoff brief carries a patient-welfare check on every finding; a model that increases prescribing indifferent to clinical appropriateness fails the gate regardless of its Qini. The series recommends this as a *per-chapter* check, and here it is non-negotiable.

## 8. Named Fellow artifact

**Write the product-handoff brief (test/build/kill) under the IP firewall, and describe the final Living-Model prototype.**

**The handoff brief (~one page, per finding).** For the meals causal forest, produce a brief stating:

- **Recommendation:** test / build / kill (e.g., *test* — replicate on partner CRM data before building).
- **Evidence level:** on the four-rung rubric (e.g., level 2 — associational-plus-heterogeneity, observational ceiling).
- **Assumptions:** IV independence (where used), no post-treatment conditioning, consent-selection handling (FCI), and the synthetic-recovery result.
- **Compliance flags:** which pathway drives the effect; route-to-counsel items (M3 reciprocity exposure; any off-label question).
- **Patient-welfare check:** is the caused prescribing clinically appropriate, or merely more?
- **Proprietary replication required:** the explicit Stage-4 step (cohort-rollout IV on partner CRM panel).
- **Kill criteria:** the conditions that end the project (e.g., cohort-timing independence test fails; opt-out-rate drift exceeds threshold; effect is reciprocity-only).
- **Risk-1 statement (do not omit):** name that data access, partner framing, and consent ethics are unresolved, and that Stage-4 "what replication is required" is *hypothetical until the partner agreement exists.*

**The final Living-Model prototype (description).** Describe the prototype the book has built toward: a parameterized SCM (Chapter 9) over the three-pathway graph (Chapter 5), with edges identified from the cohort-rollout natural experiment (Chapter 4) where data permits and *oriented by elicited rep knowledge* (Chapters 7–8) where the data is Markov-equivalent; a counterfactual engine answering the Rung-3 question — *for THIS physician, which educational message next week?*; a persuadables ranking and constrained allocation (Chapter 12) gated to the educational pathway; the consent node modeled with FCI and an opt-out-rate drift monitor; and a governance layer (provenance, monitoring, explicit human-accountability boundary). It runs on synthetic and public data in the book; the partner replicates on proprietary data behind the firewall.

This artifact closes the book. It is not a model — it is a model *plus the conditions under which it is permitted to exist.*

## 9. Research finding

**The hardest part of building a causal targeting model on this dataset is not estimation — it is that the data is selected by consent in a way that caps external validity, the most valuable pathway is the least drivable, and the data that would prove it on real physicians is locked behind a firewall and an unresolved partner agreement.** The methods are tractable. The *governance* is the open research problem — and naming it honestly, with a drift monitor and a kill criterion attached, is the contribution. A handoff brief that states its own gating risk is worth more to a partner than a prototype that hides it.

## 10. Exercises

1. **(Evaluate)** Draw the causal graph for the meals project *including* the consent node as a selection variable. Mark which edges FCI would leave unoriented because of latent confounding or selection, and write a paragraph on why PC or GES would return a confidently wrong graph here.

2. **(Evaluate / Apply)** You are handed a "bad" public note (provided) that contains a partner field mapping and a performance delta. Redact it for the public repo, move the sensitive content to a `private/` stub, and write two sentences on the redaction rule you applied. (The firewall as a habit.)

3. **(Apply+)** Take a finding (the meals causal forest result) and write the route-to-counsel memo: list every regulatory question it raises (pathway? on-label? meal threshold? off-label exposure?), *without answering any of them*, and identify which instrument (PhRMA Code / OPDP / Sunshine Act / case law) each question belongs to. The deliverable is a flag list, not an opinion.

4. **(Create / produces artifact)** Write the full one-page product-handoff brief for the meals finding: recommendation, evidence level (on the defined rubric), assumptions, compliance flags, patient-welfare check, proprietary-replication requirement, kill criteria, and the explicit Risk-1 statement. This is the book's terminal deliverable.

## 11. When to Use AI

**When to use AI.** Use an LLM to draft the *structure* of the handoff brief, to generate a redaction checklist for the firewall, to produce candidate kill criteria you then sharpen, and to *explain* why FCI differs from PC/GES so you can teach it. Use it to draft the route-to-counsel memo's *question list* — LLMs are good at enumerating regulatory questions.

**When NOT to use AI.** Do *not* let an LLM render a regulatory or legal judgment — it will confidently state a "rule" that is wrong, out of date, or jurisdiction-specific, and the whole discipline of this chapter is *route, don't adjudicate.* Do not trust an LLM's recollection of a statute number, a court case, a PhRMA Code revision date, or a de-minimis threshold; every one of those is `[verify]`. Do not let it decide a patient-welfare call or a pathway exclusion. And never let it write the Risk-1 statement *out* of your brief because it "sounds more confident" without it.

**LLM exercise (ready to paste).**

> *I am writing a one-page product-handoff brief for a pharma analytics finding (a causal-forest estimate of how promotional meals affect prescribing, estimated on public CMS Open Payments + Medicare Part D data). The brief must NOT give legal advice — it must FLAG regulatory questions and route them to counsel. (1) Draft a route-to-counsel section: enumerate every regulatory/compliance question this finding raises, and for each, name which framework it likely belongs to (PhRMA Code on HCP interactions, FDA OPDP promotional regulation, or the Physician Payments Sunshine Act / Open Payments). (2) Do NOT answer any of the questions or state what the law requires — only flag and route. (3) Add a one-line patient-welfare check. Mark anything you are unsure of as [verify].*

**CLI exercise.** Using your CLI agent, ask it to scan a draft public repo (provided/synthetic) for firewall violations: occurrences of partner-specific field mappings, real performance numbers, or anything that only makes sense given proprietary data. Have it produce a redaction report. Then *check its report by hand* — it will miss context-dependent leaks (a "real" number disguised as an example) and over-flag harmless synthetic values. The skill is auditing the audit.

**AI Validation.** Validate any AI-drafted brief against three things: (1) every regulatory statement is *flagged and routed*, never adjudicated — reject any sentence that asserts what the law requires; (2) every statute/case/threshold carries a `[verify]` tag or a checked source; (3) the Risk-1 statement is present and names data access, partner framing, and consent ethics as unresolved. A brief that reads as confident and complete is suspect — this domain's honest output is hedged and routed.

## 12. AI Use Disclosure

Your disclosure for this chapter's brief must name **one judgment AI could not make**: a route-to-counsel flag you raised that the AI would have answered, a pathway/welfare exclusion you made on domain grounds, a `[verify]` you attached to a regulatory claim the AI stated as fact, or the Risk-1 statement you insisted on keeping. State the model, what it drafted, and where — and why — you overrode it. In a chapter whose whole posture is "route, don't adjudicate," the human override *is* the competency being assessed.

## 13. What would change my mind

I would soften the consent-collider alarm if it could be shown that opt-out is *not* meaningfully correlated with treatment responsiveness — if, on data where the opt-out population is partially observable (a setting with delayed or partial consent), the opt-ins and opt-outs respond to messages indistinguishably. That would make the selection ignorable and the FCI machinery overkill. I would also revise the educational-pathway-only line if counsel and the evidence base concluded that a different pathway is in fact defensibly drivable under current regulation — this is, by the book's own discipline, *their* call, not mine. And I would withdraw the "governance is the hard part" finding if a partner deployment showed the methods failing for ordinary estimation reasons long before any governance constraint bit. The claim is that governance is the *binding* constraint; if estimation binds first, I am wrong about where the difficulty lives.

## 14. Still puzzling

- **The opt-out population is invisible by construction.** You cannot directly measure how opt-outs differ, because their data is suppressed. FCI and the drift monitor are partial mitigations; external validity beyond the consenting frame is *fundamentally capped*. Is there any honest way to bound the bias, or must the handoff simply declare the ceiling and stop?
- **Risk 1 is unresolved as this book closes.** Proprietary-data access, partner framing, and consent ethics are not settled. Stage 4 — the only stage that proves anything on real physicians — presupposes a data-access and IP-firewall agreement that does not yet exist, and the critical-of-pharma framing needs partner sign-off on exactly the chapters (5, 12, 13) that carry the argument. The book ends pointing at this blocker, not past it.
- **The evidence-level rubric is a design decision, not a standard.** The four-rung scale here is proposed, not validated; a different partner might define the rungs differently, and "level 2" is only as meaningful as the shared rubric behind it.

## 15. Summary

The last chapter is where the methods meet the constraints that decide whether any of it ships. Consent is a *selection collider*: a model trained on the physicians who opted in silently generalizes wrong to the physicians who opted out — and you cannot see the failure, because the opt-out data is suppressed. Put consent in the graph, discover with FCI rather than PC/GES, and watch the opt-out rate with a drift monitor. The regulatory line makes the *educational pathway the only legally drivable one*; the reciprocity pathway, however large its CATE, is flagged and routed to counsel, never adjudicated by the Fellow. The IP firewall keeps proprietary data out of the public book — synthetic and public data here, partner replication behind the wall — and the firewall is a habit of redaction, not a policy paragraph. The terminal deliverable is a *handoff brief*: test/build/kill, with evidence level, assumptions, compliance flags, a mandatory patient-welfare check, the proprietary-replication requirement, and kill criteria — governance, not a demo. And the brief names Risk 1 honestly: data access, partner framing, and consent ethics are unresolved, and a handoff that hides its own gating risk is a sales pitch. The Living Model the book built is not a model alone; it is a model plus the conditions under which it is permitted to exist.

## 16. Key terms

- **Consent / selection collider** — the opt-out mechanism (`CLM_EXPLICIT_OPT_IN_vod`, `CLM_OPT_OUT_BEHAVIOR_vod`) as a Berkson selection node on the sampling frame; conditioning on "physicians we have data for" biases the estimate.
- **FCI (Fast Causal Inference)** — latent-variable- and selection-aware causal discovery; returns a PAG admitting unobserved confounders/selection; the right tool where PC/GES's causal-sufficiency assumption fails.
- **Opt-out-rate drift monitor** — a first-class Living-Model signal; a shift in who opts out changes the bias structure and invalidates a model trained on the old consenting frame.
- **Educational-pathway-only gate** — the rule that only M1 (on-label, fair-balanced education) is built to drive; M3 reciprocity and M2 relationship-maintenance are flagged and routed to counsel.
- **Route to counsel** — flag regulatory questions (PhRMA Code, FDA OPDP, Sunshine Act, off-label/Caronia/Amarin) and hand them to legal; never adjudicate. A professional competency.
- **IP firewall** — proprietary partner (Doceree) data stays out of the public book/repo; public + synthetic data only; sensitive material in `private/` (gitignored); partner replicates internally.
- **Four-stage pipeline** — research/identification → synthetic validation → public-data demonstration → partner replication/product discovery.
- **Product-handoff brief** — the terminal governance deliverable: test/build/kill + evidence level + assumptions + compliance flags + patient-welfare check + replication requirement + kill criteria + Risk-1 statement.
- **Risk 1** — the book's gating blocker: proprietary-data access + partner framing + consent ethics, unresolved.

## 17. Bridge — a closing forward-look

There is no next chapter, so this is not a handoff to one — it is the handoff the whole book has been about.

You arrived able to read rep-visit telemetry as a CRM log. You leave able to read it as what it is: a fifteen-year natural experiment that pharma ran on physicians and never analyzed, dense enough to identify what messages *cause* prescribing, structured enough to need a rep's knowledge to orient the edges the data cannot, and constrained enough — by consent, by regulation, by a firewall — that building the model responsibly is a harder problem than building it at all. That asymmetry is the book's last lesson. The Rung-1 NBA engine optimizing email opens was never failing for lack of data or compute; it was failing because it asked the wrong question and stopped before the hard part. The hard part is here: identify the effect, orient it with human knowledge, gate it to the educational pathway, model the consent that selects your sample, and hand the partner a brief that says, in plain terms, *what we know, how well we know it, what would make us kill it, and what we are still not allowed to do.*

What happens next is not yours to finish in a textbook. The cohort-rollout instrument still has to be run on real CRM data behind the firewall. The partner agreement that gates Stage 4 still has to be signed. The reps still have to be interviewed, the welfare gate still has to be defended account by account, and the opt-out rate still has to be watched for as long as the model lives. The model is "living" in exactly this sense: it is never finished, because the data drifts, the consent frame shifts, and the regulatory line moves. Your job was never to ship a final answer. It was to build the thing that keeps asking the right question honestly — and to be candid, in the handoff, about everything it still cannot do. The data was always there. Now you know what to ask of it, and what not to.

## 18. Further reading

- CMS Open Payments program (openpaymentsdata.cms.gov) and the Physician Payments Sunshine Act — 42 U.S.C. §1320a-7h; 42 C.F.R. Part 403; ACA 2010 §6002. The public disclosure dataset that makes the meals project firewall-safe. `[verify current thresholds]`
- PhRMA *Code on Interactions with Health Care Professionals* (phrma.org; revisions effective Jan 1, 2022). `[verify no newer revision]`
- FDA Office of Prescription Drug Promotion (OPDP) overview (fda.gov) — truthful, non-misleading, fair-balance, on-label standards; Untitled/Warning Letters.
- *United States v. Caronia*, 703 F.3d 149 (2d Cir. 2012); *Amarin Pharma, Inc. v. FDA* (S.D.N.Y. 2015). — the truthful-off-label-speech commercial-speech tension. **Route to counsel.**
- Spirtes, Glymour & Scheines, *Causation, Prediction, and Search*; Zhang, J. (2008) on FCI completeness with latent confounders and selection bias. `[verify exact cite]` — the FCI foundations.
- Pearl, J. *The Book of Why* (`_lib_science-the-book-of-why-...md`) — colliders and selection bias; the consent collider's conceptual foundation.
- O'Neil, C. *Weapons of Math Destruction* (`_lib_math-weapons-of-math-destruction-...md`) — the fairness/accountability lens on confident models that work on a selected subsample and harm the rest; core for the silent-generalization failure.
- *The Lean Startup* (`_lib_economics-the-lean-startup-...md`) and *Flawless Consulting* (`_lib_business-flawless-consulting-4th-edition.md`) — the four-stage validated-learning pipeline and the handoff-as-governance/contracting framing.
- Larkin, I. et al. (2017) — DiD on academic-medical-center detailing restrictions (the causal-design counterpoint on detailing→prescribing); DeJong (2016) and Mitchell (2021 review) for the associational meals→prescribing evidence base. `[verify]`
