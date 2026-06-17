# Chapter 13 — Consent, Compliance, and the Handoff
*The methods are tractable. The governance is the open research problem. A handoff that hides its own gating risk is a sales pitch.*

A Fellow builds a beautiful model. The meals causal forest recovers the synthetic ground truth exactly. On real public Open Payments data it produces a clean, heterogeneous CATE surface. The persuadables ranking has a higher Qini than the propensity list. Cross-validation is immaculate. The Fellow writes: *the prototype works; the product is ready.*

It is not, and the reason is invisible by construction.

Every physician in that training data *consented to being logged*. The ones who opted out — privacy-conscious, industry-skeptical, working under institutional restrictions on rep contact — are not in the data at all. Their clickstream was suppressed, their NPI stripped. The model was fit on a selected sample. When it is deployed across the full physician panel, including the opt-out-like physicians, its recommendations are systematically off — because the very traits that make a physician opt out (skepticism, institutional limits, a different relationship with reps) are plausibly the same traits that change how she responds to a message.

Here is the trap that makes this worse than an ordinary generalization error: **you cannot see the failure.** The opt-out data is suppressed, so there is no held-out set of opt-outs to validate against. The model validates beautifully on the only people it can see — the consenters — and fails silently on the people it cannot. The cross-validation that looked immaculate was immaculate *within the selected sample* and told you nothing about the population you deployed to.

The only early warning is structural: a drift monitor watching the opt-out rate, because a change in who opts out changes the bias. This chapter is about building a model — and a handoff — that takes that failure seriously instead of being surprised by it.

---

## Consent as a selection collider

Consent is not legal metadata. It changes what is observed, and that makes it a causal-graph object, not a checkbox.

In Veeva's CRM, `CLM_EXPLICIT_OPT_IN_vod` governs logging. Opt-out either suppresses the clickstream or anonymizes it; `CLM_OPT_OUT_BEHAVIOR_vod` strips the NPI from the activity record. (These field names are dated mid-2026 and drift with the Veeva CRM → Vault CRM migration; verify against the partner's current schema.) If opt-out is systematically correlated with physician characteristics — and privacy-consciousness, industry-skepticism, and institutional contact policies make that nearly certain — then "the physicians we have data for" is a selected set.

Conditioning your analysis on that set is **conditioning on a collider** — specifically a Berkson selection node on the sampling frame. It is the same structural disease you met in Chapter 6, where dwell time and reaction scores were post-treatment colliders. Consent is a *selection* collider: different position in the graph, identical pathology. Conditioning on it opens a non-causal path and biases the estimate. Whatever you learned to fear about controlling for post-treatment engagement, fear the same way about analyzing only the consenters.

The fix is to put the consent and observation mechanism in the causal graph as an explicit node, and to discover structure with a method that is built for latent confounders and selection variables.

![Directed causal graph. A latent physician-characteristics node (dashed circle) sends arrows to both an opt-out / consent node and a message-responsiveness node. The opt-out node points into an observed-sample node enclosed in a selection boundary marking conditioning on "physicians we have data for." The responsiveness node points into a prescribing-outcome node. Conditioning on the observed sample opens a non-causal path (drawn as a dashed hairline, not a causal edge) between opt-out and responsiveness.](../images/13-consent-compliance-and-handoff-fig-01.png)

*Figure 13.1 — Consent is a selection collider — a Berkson selection node on the sampling frame. Conditioning on "physicians we have data for" opens a non-causal path from physician characteristics to the outcome, and the opted-out population is invisible by construction. FCI models this and leaves edges honestly unoriented; PC and GES assume it away. FCI plus an opt-out-rate drift monitor are partial mitigations, not a fix.*

<!-- → [DIAGRAM: Causal graph with consent/opt-out as an explicit selection node — arrow from "physician characteristics" (unobserved) to both "opt-out" and "message responsiveness"; "opt-out" node connects to "observed sample" with a selection boundary marked. Caption: "Consent is a selection collider. Conditioning on 'physicians we have data for' opens a non-causal path from physician characteristics to the outcome. FCI models this; PC and GES assume it away."] -->

---

## Why FCI, not PC or GES

Chapter 7 taught that observational data identifies an equivalence class, not a single graph. The standard discovery algorithms — PC and GES — assume **causal sufficiency**: no unmeasured confounders and no selection bias. That assumption is exactly what the consent structure violates. There is an unmeasured selection mechanism, and it is the whole problem.

**FCI (Fast Causal Inference)** is the latent-variable-aware extension. It does not assume causal sufficiency. It returns a partial ancestral graph (PAG) that admits unobserved confounders and selection variables, marking edges it cannot fully orient because of them. For the consent node, that honesty is the point: FCI will not hand you a confident DAG when a selection collider is in play. PC or GES would — and the confident graph would be confidently wrong. (FCI traces to Spirtes, Glymour, and Scheines, *Causation, Prediction, and Search*; Jiji Zhang (2008), "On the completeness of orientation rules for causal discovery in the presence of latent confounders and selection bias," *Artificial Intelligence* 172(16–17):1873–1896, extended its orientation rules to a complete set covering the latent-confounder and selection-bias case.)

The Living Model's drift monitor watches opt-out-rate shifts as a first-class signal. If who opts out changes, the bias structure changes, and a model validated under the old consenting frame is silently invalidated under the new one. The drift monitor is not a nice-to-have. It is the only instrument that can see the opening case's failure coming.

---

## The regulatory line: the educational pathway is the only legally drivable one

Chapter 5 drew three pathways from message to prescribing — M1 educational, M2 relationship-maintenance, M3 reciprocity. Chapter 5 noted they carry different regulatory status. This chapter cashes that out, under a hard discipline: **the book flags the landscape; the partner's counsel decides. Nothing here is legal advice.**

The book's position, flagged and routed but not adjudicated:

**M1 — educational** (substantive, on-label, fair-balanced clinical information) is the only pathway a Living Model should be built to drive. Promotion that is truthful, non-misleading, fairly balanced, and consistent with FDA-approved labeling sits inside what the FDA Office of Prescription Drug Promotion (OPDP) regulates and, on current interpretation, permits.

**M3 — reciprocity** (meals, gifts) is regulatorily fraught. Payments are disclosed under the Sunshine Act; the PhRMA Code limits meals and gifts and entertainment. A model that *optimizes* the reciprocity pathway is optimizing precisely what regulators and the public scrutinize. Flag it. Route it to counsel.

**M2 — relationship-maintenance** (samples, co-pay cards) sits in a gray zone. Flag it.

This is why Chapter 12's optimization carried an educational-pathway-only gate: a high-CATE physician whose effect runs through reciprocity is excluded from the drivable target set — not because the effect is not real, but because it is not legally or ethically drivable. The pathway decomposition from Chapters 5 and 11 is what makes that gate operational rather than aspirational. Teach the gate as the feature, not the constraint.

Three instruments to name — route each to counsel, do not adjudicate:

**PhRMA Code on Interactions with HCPs.** Voluntary industry self-regulation; the revisions effective January 1, 2022 — the current version as this book closes — restated that meals be modest and occasional and tied to a bona fide informational/educational presentation, kept the prohibition on entertainment and recreation in connection with such presentations, and tightened speaker-program rules in response to the HHS-OIG November 2020 Special Fraud Alert on speaker programs. (Confirm against the partner's then-current PhRMA Code in case of a later revision.)

**FDA OPDP** (Office of Prescription Drug Promotion, within CDER). Regulates prescription-drug promotion to be truthful, non-misleading, fairly balanced, and on-label. Enforces via Untitled Letters and Warning Letters.

**Physician Payments Sunshine Act → CMS Open Payments.** Mandatory disclosure of manufacturer payments and transfers of value (42 U.S.C. §1320a-7h; 42 C.F.R. Part 403; ACA 2010 §6002). The reciprocity pathway is publicly visible because of this Act — simultaneously the data source for the meals project and the reason driving M3 is reputationally radioactive. The de minimis reporting threshold is CPI-adjusted annually; for calendar year 2025 a single payment under $13.46 need not be reported unless the aggregate to a covered recipient exceeds $134.54 — figures corroborated across multiple compliance/legal summaries of CMS guidance. (The 2026 figures had not been published as this book closed; verify both years against the primary CMS Open Payments page before relying on them.)

On off-label promotion: *United States v. Caronia*, 703 F.3d 149 (2d Cir. 2012), and *Amarin Pharma, Inc. v. FDA*, 119 F. Supp. 3d 196 (S.D.N.Y. 2015) protect truthful, non-misleading off-label speech in the Second Circuit; the national status is unsettled. A scientific-exchange lane exists under condition-laden FDA guidance — most directly the FDA guidance "Communications From Firms to Health Care Providers Regarding Scientific Information on Unapproved Uses of Approved/Cleared Medical Products" (the "SIUU" guidance; draft October 2023, finalized January 2025). **All of this is route-to-counsel, not a Fellow's call.** Routing rather than adjudicating is itself a professional competency.

---

## The public-data / IP firewall

The core telemetry — Veeva CRM rep-visit records — is partner-owned. The firewall is the structural rule that lets the book be fully public while the product stays proprietary.

In the public book and repo: methods; the synthetic rep-visit dataset with known ground-truth structure; and the public-data project — Open Payments meals plus Medicare Part D. Nothing proprietary.

In `private/` (gitignored) and partner-internal: real CRM field mappings, partner performance deltas, proprietary targeting methods. The partner replicates the validated methods on real CRM data internally.

The synthetic-first rationale: because real data is proprietary, the book ships a synthetic dataset with known ground truth so Fellows can verify their methods recover it. The meals project uses real public data. The firewall is a *habit*, not a policy paragraph. The failure mode is a Fellow who writes a public note containing a partner-specific field mapping or a real performance delta — IP leakage. Treat redaction as a reflex: anything that only makes sense because you saw the partner's data goes in `private/`.

---

## The four-stage pipeline and the handoff brief

The handoff is **governance, not a presentation.**

Where Fellows get stuck is treating the handoff as a demo of a working model. Reframe it: the handoff is a test/build/kill recommendation with evidence level, assumptions, compliance flags, a welfare check, kill criteria, and a statement of what proprietary replication is still required. A lab result is not a prescription — it needs interpretation, contraindications, and authorization. The handoff brief is that layer, not the result.

**Four stages:**

1. **Research and identification** — state the do-question and the identification strategy, falsification-first.
2. **Synthetic validation** — show the method recovers known ground truth on the synthetic dataset.
3. **Public-data demonstration** — run on Open Payments and Part D, reaching a stated evidence level.
4. **Partner replication and product discovery** — the partner runs the validated method on proprietary CRM data internally, behind the firewall. Only then does product discovery begin.

The evidence-level rubric, defined explicitly so it means something:

- **Level 1** — associational: correlation observed, no causal design.
- **Level 2** — associational plus heterogeneity: CATE surface estimated, still observational.
- **Level 3** — quasi-experimentally identified: cohort-rollout IV or clean DiD; causal claim defensible.
- **Level 4** — replicated on partner data: Stage 4 complete; the partner's biostatistician has reviewed it.

**The handoff brief, per finding, must state:**

- **Test/build/kill** recommendation with justification.
- **Evidence level** on the four-rung rubric.
- **Assumptions** — IV independence where used; no post-treatment conditioning; consent-selection handling (FCI); synthetic-recovery result.
- **Compliance flags** — which pathway; route-to-counsel items (M3 reciprocity exposure; any off-label question).
- **Patient-welfare check** — is the caused prescribing clinically appropriate, or merely more?
- **Proprietary replication required** — the explicit Stage 4 step.
- **Kill criteria** — the conditions that end the project.
- **Risk-1 statement** — name that data access, partner framing, and consent ethics are unresolved, and that "what replication is required" is hypothetical until the partner agreement exists.

| Required field | What it answers |
| --- | --- |
| Test / build / kill | The recommendation itself, with justification — what the team should do next, not "it works." |
| Evidence level | Where the finding sits on the four-rung rubric (1 associational, 2 + heterogeneity, 3 quasi-experimentally identified, 4 replicated on partner data). |
| Assumptions | IV independence where used; no post-treatment conditioning; consent-selection handling via FCI; the synthetic-recovery result. |
| Compliance flags | Which pathway (M1/M2/M3); route-to-counsel items (M3 reciprocity exposure, any off-label question) — flagged, never adjudicated. |
| Patient-welfare check | Whether the caused prescribing is clinically appropriate, or merely more. |
| Proprietary replication required | The explicit Stage 4 step the partner must run behind the firewall before product discovery. |
| Kill criteria | The specific conditions that end the project (e.g., cohort-rollout independence test fails). |
| Risk-1 statement | Names that data access, partner framing, and consent ethics are unresolved — and that "what replication is required" is hypothetical until a partner agreement exists. Risk 1 (Doceree-style data access) is an open blocker. |

*Table 13.1 — The handoff brief template: the terminal governance artifact. A brief missing any row is not a handoff; it is a sales pitch. The Risk-1 row is mandatory — Risk 1 (proprietary data access) remains an open blocker as the book closes; regulatory items are routed to counsel, never decided here.*

---

## Walking the meals project through the pipeline

We walk the meals causal forest through all four stages, dead ends included.

**Stage 2 — synthetic validation.** The forest recovers the synthetic ground truth: the physicians built to be reciprocity-responsive are the ones it flags. Good. The method works on data whose truth is known.

**Stage 3 — public-data demonstration.** Run on real Open Payments meals plus Part D prescribing. It produces a heterogeneous CATE surface and reaches evidence level 2 — associational plus heterogeneity. Note the ceiling: Open Payments is observational. There is no cohort-rollout instrument in public data, so this is not clean identification.

**Dead end one.** The Fellow writes: *the public prototype works, so the product is ready.* No. The cohort-rollout IV that would identify a clean effect lives only in the partner's CRM data. Stage 4 replication is required before product discovery. The prototype demonstrates the method; it does not bless the product.

**Dead end two.** A Fellow drafts the public handoff note and, to make it concrete, pastes in a real partner field mapping and a performance delta from a conversation with the partner. IP leakage — a firewall breach. The fix is reflexive redaction: those lines go to `private/`, and the public note describes the method and the synthetic and public results only.

**Lesson.** The handoff *gates*; it does not *bless*. "Level 2, observational, replicate before building, kill if the cohort-rollout independence test fails" is more valuable than "it works."

**Limit.** Even after Stage 4 replication, the consent-selected sample caps external validity. The model is honest only about the consenting frame; the opt-out population remains invisible by construction. The drift monitor on opt-out rate is a permanent obligation, not a one-time check. FCI plus the drift monitor are partial mitigations, not a solution. Be honest that this is a structural limit, not a bug to engineer away.

---

## The Living-Model prototype, described

The prototype the book has been building toward is not a model. It is a model plus the conditions under which it is permitted to exist.

A parameterized SCM over the three-pathway graph from Chapter 5, with edges identified from the cohort-rollout natural experiment where data permits, and oriented by elicited rep knowledge — the Causal Interview Bot output from Chapter 8 — where the data is Markov-equivalent. A counterfactual engine answering the Rung-3 question: *for this physician, which educational message next week?* A persuadables ranking and constrained allocation gated to the educational pathway, not the reciprocity pathway. The consent node modeled with FCI and an opt-out-rate drift monitor as a permanent first-class signal. A governance layer with provenance tags, a monitoring plan, and an explicit human-accountability boundary written down at the start — which decisions the model makes, and which a human makes — because that boundary cannot be assumed.

It runs on synthetic and public data in this book. The partner replicates on proprietary data behind the firewall. And the handoff brief that ships with it names, in plain language, what the model can do, how well it knows it, what would make the team kill it, and what it is still not permitted to do.

---

## What would change my mind

I would soften the consent-collider alarm if it could be shown that opt-out is not meaningfully correlated with treatment responsiveness — if, on data where the opt-out population is partially observable, opt-ins and opt-outs respond to messages indistinguishably. That would make the selection ignorable and FCI overkill.

I would revise the educational-pathway-only line if counsel and the evidence base concluded that a different pathway is defensibly drivable under current regulation. This is, by the book's own discipline, their call, not mine.

I would withdraw "governance is the hard part" if a partner deployment showed the methods failing for ordinary estimation reasons long before any governance constraint bit. The claim is that governance is the *binding* constraint. If estimation binds first, I am wrong about where the difficulty lives.

## Still puzzling

The opt-out population is invisible by construction. You cannot directly measure how opt-outs differ because their data is suppressed. Is there any honest way to bound the bias — a sensitivity analysis over plausible opt-out distributions — or must the handoff simply declare the ceiling and stop?

Risk 1 is unresolved as this book closes. Proprietary-data access, partner framing, and consent ethics are not settled. Stage 4 presupposes a data-access and IP-firewall agreement that does not yet exist. The book ends pointing at this blocker, not past it.

The evidence-level rubric proposed here is a design decision, not a standard. "Level 2" is only as meaningful as the shared rubric behind it, and a different partner might define the rungs differently.

---

## Exercises

**Warm-up**

1. *(Recall — tests the consent collider)* Draw the causal graph for the meals project including the consent node as a selection variable. Mark which edges FCI would leave unoriented because of latent confounding or selection, and write a paragraph explaining why PC or GES would return a confidently wrong graph here. *What this tests: whether you can place consent in the graph as a structural object, not a checkbox.*

2. *(Recall — tests the regulatory framework)* Name the three instruments the chapter routes to counsel and for each state, in one sentence, what question it governs. Then explain why M3 reciprocity is excluded from the drivable target set even when its CATE is the largest. *What this tests: whether you understand the pathway gate as a governance feature rather than an arbitrary restriction.*

3. *(Recall — tests the four-stage pipeline)* Describe what happens at each of the four stages and state, for each, what evidence level it can reach. Then explain why a Stage-3 result cannot authorize a product decision. *What this tests: whether you understand the pipeline as a validated-learning structure rather than a waterfall.*

**Application**

4. *(Apply — firewall discipline)* You are given a draft public note containing a partner field mapping and a real performance delta. Redact it for the public repo, move the sensitive content to a `private/` stub, and write two sentences stating the redaction rule you applied and why it is a habit rather than a policy paragraph. *What this tests: whether you can execute the firewall discipline reflexively on a live example.*

5. *(Apply — route-to-counsel memo)* Take the meals causal forest finding and write the route-to-counsel section of the handoff brief: enumerate every regulatory question it raises — pathway, on-label status, meal threshold, off-label exposure — *without answering any of them*, and identify which instrument (PhRMA Code, FDA OPDP, Sunshine Act, case law) each question belongs to. The deliverable is a flag list, not an opinion. *What this tests: whether you can enumerate the questions a finding raises without collapsing them into answers.*

6. *(Apply — drift monitor design)* Design the opt-out-rate drift monitor for the Living Model: what metric you would track, at what frequency, what threshold would trigger a model review, and what you would do when the threshold is crossed. Explain why a significant drift in opt-out rate invalidates a model validated on an earlier consenting frame. *What this tests: whether you understand the drift monitor as a permanent governance obligation rather than a one-time check.*

**Synthesis**

7. *(Synthesize — handoff brief)* Write the full one-page product-handoff brief for the meals causal forest finding: test/build/kill recommendation, evidence level on the defined rubric, assumptions, compliance flags, patient-welfare check, proprietary replication required, kill criteria, and the explicit Risk-1 statement. The brief must not adjudicate any regulatory question and must not omit the Risk-1 statement. *What this tests: whether you can produce the terminal governance artifact with every required component.*

8. *(Synthesize — consent + estimation)* Connect the consent-collider problem to the staggered-DiD design from Chapter 10. Explain how consent opt-out interacts with the parallel-trends assumption: under what conditions does opt-out selection violate parallel trends, and what additional check — beyond the standard cohort-on-pre-prescribing falsification — would you need to defend the design against the consent threat? *What this tests: whether you can trace a governance constraint back into an estimation design.*

**Challenge**

9. *(Challenge — prototype description)* Write a two-page description of the final Living-Model prototype: the SCM structure, the identification strategy for each edge, the source of structural information for edges the data cannot orient, the counterfactual engine's Rung-3 output, the persuadables ranking with its educational-pathway-only gate, the FCI-modeled consent node, the drift monitor, and the governance layer with its explicit human-accountability boundary. Your description must state, for each component, what public or synthetic data demonstrates it and what proprietary replication is required before deployment. *What this tests: whether you can describe a model plus the conditions under which it is permitted to exist — the book's terminal synthesis.*

---

## Prompts

### Figure 13.1 — Consent as a selection collider (Berkson selection node)

Generate a single self-contained HTML file (inline CSS, D3 7.9.0 from the cdnjs CDN) rendering a directed causal graph (DAG/PAG), not a quantitative chart. Nodes: a latent "physician characteristics" node at top drawn as a dashed circle; an "opt-out / consent" node mid-left drawn in the single red accent (the selection collider); a "message responsiveness" node mid-right; an "observed sample" node lower-left enclosed in a rectangular selection boundary labeled "condition on physicians we have data for"; a "prescribing outcome" node lower-right. Directed edges (solid, arrowhead marker): latent -> opt-out, latent -> responsiveness, opt-out -> observed sample, responsiveness -> outcome. Draw the path opened by conditioning as a dashed gray hairline with NO arrowhead between opt-out and responsiveness, labeled "non-causal path opened by conditioning (not a causal edge)". Brutalist palette, full font chains via CSS variables (light+dark), circles/rects rx=0, no gradients, reduced-motion guard; keep all text inside the viewBox and never on an arrow centerline. Caption: a Berkson selection collider; the opted-out population is invisible by construction; FCI models this, PC and GES assume it away; FCI plus drift monitor are partial mitigations, not a fix; PAG edge marks inferred. Deliverable: one HTML file.

---

## Chapter 13 Exercises: Consent, Compliance, and the Handoff

**Project:** The Causal Interview Bot (Capstone)
**This chapter adds:** The bot's consent and compliance guardrails — what it must NOT capture — plus the handoff brief that ships the working bot, its annotated prior DAG, and a test/build/kill recommendation as the book's terminal governance artifact.

### Exercise 1 — When to Use AI

This is the capstone: you are assembling the working Causal Interview Bot, its annotated prior DAG, and the handoff brief. Three sound uses of an LLM.

- **Task A.** Have the model draft the bot's *guardrail spec* — the explicit list of fields the bot must refuse to capture (NPI tied to opt-out behavior, anything that re-identifies an opted-out physician, free-text that could encode PII). *Why AI works here:* it is a structured enumeration you review line by line against the consent-collider argument and the firewall rule; a wrong line is visible.
- **Task B.** Ask the model to draft the route-to-counsel section of the handoff brief — enumerate every regulatory question the meals finding raises and name which instrument (PhRMA Code, FDA OPDP, Sunshine Act) each likely belongs to, *without answering any*. *Why AI works here:* flagging-and-routing is exactly the safe operation; you check that nothing was adjudicated.
- **Task C.** Have the model assemble the handoff-brief skeleton across files — pulling the evidence level, assumptions, and kill criteria from the Chapters 10–12 artifacts into Table 13.1's required rows. *Why AI works here:* it is collation of artifacts you produced; you verify every row traces to a real upstream result.

**The tell:** in each case you can independently evaluate the output against the consent argument, the routing rule, or the upstream artifacts. The model drafts and collates; it never decides what the law requires or what the bot is permitted to capture.

### Exercise 2 — When NOT to Use AI

Three judgments that are categorically not the model's.

- **Task A.** Do not let the model adjudicate any regulatory question. *Why AI fails here:* whether a pathway is legally drivable is a values/consent and legal judgment routed to the partner's counsel — "the book flags the landscape; counsel decides." The model will fluently state what the law "requires," which is exactly the move the discipline forbids. Reject any sentence that asserts a legal requirement.
- **Task B.** Do not let the model decide what the bot may capture about opted-out physicians. *Why AI fails here:* the consent collider is a values/consent firewall — the opted-out population is invisible by construction and must stay that way; whether a field re-identifies them is a welfare judgment, not a schema question. (Ground truth: you cannot validate against the opt-outs because their data is suppressed.)
- **Task C.** Do not let the model write or soften the Risk-1 statement. *Why AI fails here:* naming that data access, partner framing, and consent ethics are unresolved is a stance the model will tend to smooth into confidence; keeping Risk 1 open is a human professional judgment.

**The tell:** if the AI is the *reason* a regulatory item reads as resolved or a consent boundary as safe, it has made a value judgment it has no standing to make. It must draft and route while *you* adjudicate, exclude, and keep Risk 1 open. **Series connection:** Tier **T7 (Wisdom)** — consent, welfare, and regulatory adjudication are value judgments — *and* Tier **T6 (Collective)** for the handoff itself, because the brief is a governance artifact that hands accountability across people (Fellow → counsel → partner biostatistician), not a calculation any single agent completes.

### Exercise 3 — LLM Exercise

**What you're building (capstone):** the working Causal Interview Bot with its consent/compliance guardrails, an annotated prior DAG, and a one-page handoff brief — the book's terminal artifact assembled in one place.

**Tool:** Claude, as a **Claude Project** — the capstone *requires* persistent context: the bot's elicited priors (Ch8), the SCM and kept/excluded ledger (Ch11), and the gated persuadables list and exclusion log (Ch12) all live in Project knowledge, and the brief must collate across them. A one-off chat would force you to re-paste four chapters of artifacts.

**The Prompt:**

```
You are helping me assemble the capstone for a pharma causal-inference project:
the working Causal Interview Bot plus its handoff brief. In this Project you have
my bot's elicited priors, my SCM with the kept/excluded variable ledger, and my
gated persuadables list with its exclusion log.

Do five things:
1. Write the bot's CONSENT/COMPLIANCE GUARDRAIL SPEC: the explicit list of what
   the bot must NOT capture or store (NPI linked to opt-out behavior, anything
   that re-identifies an opted-out physician, free-text that could encode PII,
   any partner-specific field mapping or real performance delta). For each, one
   line on why.
2. Produce an ANNOTATED PRIOR DAG (in text): the three-pathway graph (M1
   educational, M2 relationship, M3 reciprocity) with the consent node added as a
   Berkson selection collider, each edge tagged with its identification source
   (cohort-rollout IV / elicited rep prior / Markov-equivalent-so-left-unoriented
   by FCI).
3. Draft the one-page HANDOFF BRIEF for the meals causal-forest finding with
   every required row: test/build/kill, evidence level (1-4 rubric), assumptions,
   compliance flags, patient-welfare check, proprietary replication required,
   kill criteria, and the Risk-1 statement.
4. Write the ROUTE-TO-COUNSEL section: enumerate every regulatory question the
   finding raises and name which instrument it likely belongs to (PhRMA Code,
   FDA OPDP, Sunshine Act, case law) WITHOUT answering any of them.
5. List anything you are unsure of as [verify].

Do NOT give legal advice or state what the law requires — flag and route only.
Do NOT soften or omit the Risk-1 statement. Do NOT propose capturing anything
about opted-out physicians.
```

**What this produces:** the guardrail spec, the annotated prior DAG, the full handoff brief, and the route-to-counsel section — the four pieces of the capstone in one document, ready for your adjudication and the partner's counsel.

**How to adapt:** swap in your own bot's priors and finding; for a different drug class, re-run the upstream chapters first. With ChatGPT or Gemini, you must paste the Ch8/Ch11/Ch12 artifacts inline because they lack the persistent Project context the capstone leans on — which is itself the argument for using a Claude Project across the series.

**Connection to previous chapters:** this brief collates the Chapter 10 falsification evidence level, the Chapter 11 SCM and assumptions, and the Chapter 12 gated persuadables list and exclusion log; the bot is the Chapter 8 artifact, now wrapped in guardrails; the consent node extends the Chapter 6 collider discipline to a selection collider.

**Preview of next chapter:** this is the capstone — there is no next chapter. The deliverable is the working bot, the annotated prior DAG, and the handoff brief together; the brief explicitly names what proprietary Stage-4 replication remains and keeps Risk 1 open as the book closes.

### Exercise 4 — CLI Exercise

**What you're building (capstone):** the handoff package assembled across files — guardrail spec, annotated prior DAG, and the handoff brief — with a firewall-scan and a brief-completeness check.

**Tool:** Cowork — favored here over plain Claude Code because the capstone assembles the brief *across multiple files* (the Ch8/Ch11/Ch12 artifacts, the DAG, the brief template) and Cowork's multi-file session is built for that cross-file collation and presentation. · **Skill level:** intermediate-to-advanced.

**Setup:**
- [ ] A repo with the synthetic upstream artifacts (synthetic + public only — real CRM telemetry is partner-proprietary, behind the firewall; `private/` is gitignored).
- [ ] The four upstream artifacts present: bot priors, SCM ledger, persuadables list + exclusion log, evidence levels.
- [ ] The Table 13.1 required-rows list available as a checklist file.

**The Task:**

```
Assemble the capstone handoff package. Read the synthetic/public artifacts in
artifacts/ and the required-rows checklist in docs/handoff_rows.md. Do NOT read or
write private/ or any file matching *_real_* — proprietary, out of scope. Treat
redaction as a reflex.

1. Write handoff/guardrails.md: the bot's consent/compliance guardrail spec
   (what it must NOT capture).
2. Write handoff/prior_dag.md: the annotated prior DAG with each edge's
   identification source and the consent node as a selection collider.
3. Write handoff/brief.md: the one-page brief with EVERY Table 13.1 row.
4. Run a firewall scan over the whole public repo for partner-specific field
   mappings, real performance numbers, or anything that only makes sense given
   proprietary data; write handoff/redaction_report.md.
5. Write a check that FAILS if handoff/brief.md is missing any required row, if
   any sentence adjudicates a regulatory question (asserts what the law
   requires), or if the Risk-1 statement is absent.

Stopping condition: brief.md has all rows, the adjudication check finds zero
asserted legal requirements, the Risk-1 statement is present, and the redaction
report is written. After it passes, print every line the firewall scan flagged so
I can confirm each is a real synthetic value and not a leak — and confirm the bot
guardrail spec forbids capturing anything about opted-out physicians.
```

**Expected output:** the guardrail spec, the annotated DAG, the complete brief, a redaction report, and a passing completeness/adjudication/Risk-1 check.

**What to inspect:** the redaction report (the scan over-flags synthetic values and misses context-dependent leaks — audit the audit by hand), and that no brief sentence states a legal requirement.

**If it goes wrong:** the agent may "complete" the brief by writing a confident regulatory conclusion to fill the compliance-flags row. Recovery: the adjudication check must fail on any sentence asserting what the law requires; route those to counsel instead. The agent may also pass the redaction scan while missing a context-dependent leak — so the by-hand audit step is mandatory, not optional.

**CLAUDE.md note:** add — "Synthetic and public data only; never read or write `private/` or `*_real_*`; redaction is a reflex. Regulatory items are flagged and routed to counsel, never adjudicated — no sentence may assert what the law requires. The Risk-1 statement is mandatory and never softened. The bot must never capture anything that re-identifies an opted-out physician."

### Exercise 5 — AI Validation Exercise

**What you're validating (capstone):** the assembled handoff package — your Exercise 3/4 output, or a pre-generated flawed brief that adjudicated a regulatory question ("this meal value is within the Sunshine Act threshold, so it is compliant"), captured an opted-out physician's NPI, and dropped the Risk-1 statement.

**Validation type:** governance + firewall + consent audit (no single ground truth — the opt-out population is invisible by construction). **Risk level:** **high** — because a confident, complete-looking brief that adjudicates the law or leaks PII is precisely the artifact this chapter warns is "a sales pitch, not a handoff."

**Setup:** run the assembled package against the checklist; for the consent dimension, remember there is no held-out opt-out set to validate against — the check is structural, not statistical.

**The Validation Task:**

```
Validation Checklist — Chapter 13 (Consent, Compliance, Handoff)
Mark each Pass / Fail / Cannot-determine.

[ ] Correctness: does every brief row trace to a real upstream artifact
    (evidence level to the Ch10 design, assumptions to the Ch11 SCM, the gated
    list to Ch12)?
[ ] Completeness: are ALL Table 13.1 rows present, including the Risk-1 statement?
[ ] Scope: is the analysis limited to the consenting/logged frame, with the
    opt-out population named as invisible by construction and a drift monitor
    specified?
[ ] Chapter-specific 1 — routing not adjudicating: is every regulatory item
    FLAGGED and routed to counsel, with no sentence asserting what the law
    requires?
[ ] Chapter-specific 2 — consent guardrail: does the bot spec forbid capturing
    anything that re-identifies an opted-out physician, and does the package
    contain no PII and no partner-specific field mapping or real performance
    delta?
[ ] Failure-mode check (fluent-but-wrong): does the brief either (a) adjudicate a
    regulatory question instead of routing it to counsel, or (b) capture
    consent/PII that must not be captured? Either is a FAIL even if the brief
    reads polished and complete.
[ ] Ground truth: is the missing-ground-truth limit acknowledged — that the
    opt-out population cannot be validated against and the ceiling is declared,
    not engineered away?
```

**What to do with findings:** all pass — the capstone is shippable as a governed handoff (working bot + annotated prior DAG + brief), Risk 1 open. One fail — fix and re-run before the package leaves your hands. Multiple fails, especially regulatory adjudication or captured consent/PII, the package is not a handoff and must not ship; that is the chapter's terminal warning.

**AI Use Disclosure prompt:** *Write two sentences naming exactly what an AI tool did in your capstone work and the one judgment you supplied that it could not — for example, a route-to-counsel flag you raised that the model would have answered, a consent boundary you set on what the bot may capture, or the Risk-1 statement you insisted on keeping when the model tried to soften it.* (Mandatory.)

**Series connection:** the failure mode is **consent/PII captured that must not be / a regulatory question adjudicated instead of routed to counsel**, with no ground truth to catch it because the opt-out population is suppressed. Tier **T7 (Wisdom)** for the consent, welfare, and regulatory value judgments, and Tier **T6 (Collective)** for the handoff — the brief is the artifact that carries accountability across the Fellow, counsel, and the partner, and a brief that hides its own gating risk is a sales pitch, not a handoff.

---

## References (fact-check pass)

All regulatory, legal, and method citations were verified CONFIRMED against FDA, CMS, PhRMA, and court-record sources; every regulatory item is explicitly routed to counsel rather than adjudicated; no corrections required. See `factchecks/13-consent-compliance-and-handoff-assertions.md`.

1. Zhang, Jiji. "On the Completeness of Orientation Rules for Causal Discovery in the Presence of Latent Confounders and Selection Bias." *Artificial Intelligence* 172, no. 16–17 (2008): 1873–1896. [CONFIRMED]
2. Spirtes, Peter, Clark Glymour, and Richard Scheines. *Causation, Prediction, and Search.* 2nd ed. Cambridge, MA: MIT Press, 2000. [CONFIRMED — FCI provenance]
3. PhRMA Code on Interactions with Health Care Professionals, revisions effective January 1, 2022. [CONFIRMED]
4. HHS Office of Inspector General. Special Fraud Alert: Speaker Programs. November 2020. [CONFIRMED]
5. FDA. "Communications From Firms to Health Care Providers Regarding Scientific Information on Unapproved Uses of Approved/Cleared Medical Products" (SIUU). Revised draft October 2023; final January 2025 (Fed. Reg. Jan. 7, 2025). [CONFIRMED]
6. Physician Payments Sunshine Act, 42 U.S.C. §1320a-7h; 42 C.F.R. Part 403; ACA 2010 §6002. CY2025 de minimis $13.46 / aggregate $134.54. [CONFIRMED]
7. *United States v. Caronia*, 703 F.3d 149 (2d Cir. 2012). [CONFIRMED]
8. *Amarin Pharma, Inc. v. FDA*, 119 F. Supp. 3d 196 (S.D.N.Y. 2015). [CONFIRMED]
