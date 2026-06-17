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

<!-- → [DIAGRAM: Causal graph with consent/opt-out as an explicit selection node — arrow from "physician characteristics" (unobserved) to both "opt-out" and "message responsiveness"; "opt-out" node connects to "observed sample" with a selection boundary marked. Caption: "Consent is a selection collider. Conditioning on 'physicians we have data for' opens a non-causal path from physician characteristics to the outcome. FCI models this; PC and GES assume it away."] -->

---

## Why FCI, not PC or GES

Chapter 7 taught that observational data identifies an equivalence class, not a single graph. The standard discovery algorithms — PC and GES — assume **causal sufficiency**: no unmeasured confounders and no selection bias. That assumption is exactly what the consent structure violates. There is an unmeasured selection mechanism, and it is the whole problem.

**FCI (Fast Causal Inference)** is the latent-variable-aware extension. It does not assume causal sufficiency. It returns a partial ancestral graph (PAG) that admits unobserved confounders and selection variables, marking edges it cannot fully orient because of them. For the consent node, that honesty is the point: FCI will not hand you a confident DAG when a selection collider is in play. PC or GES would — and the confident graph would be confidently wrong. (FCI traces to Spirtes, Glymour, and Scheines; Zhang 2008 extended its completeness to the selection-bias case. [verify exact cite])

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

**PhRMA Code on Interactions with HCPs.** Voluntary industry self-regulation; revisions effective January 1, 2022 tightened meals (modest, occasional, tied to a bona fide educational presentation, generally in-office), banned entertainment and alcohol at company events, and tightened speaker-program rules. [verify no newer revision]

**FDA OPDP** (Office of Prescription Drug Promotion, within CDER). Regulates prescription-drug promotion to be truthful, non-misleading, fairly balanced, and on-label. Enforces via Untitled Letters and Warning Letters.

**Physician Payments Sunshine Act → CMS Open Payments.** Mandatory disclosure of manufacturer payments and transfers of value (42 U.S.C. §1320a-7h; 42 C.F.R. Part 403; ACA 2010 §6002). The reciprocity pathway is publicly visible because of this Act — simultaneously the data source for the meals project and the reason driving M3 is reputationally radioactive. The de minimis reporting threshold is CPI-adjusted annually; 2025 figures circulate from secondary sources at approximately $13.46 per item and $134.54 aggregate, but these are not confirmed directly from a CMS page. [verify — 2025 and 2026 thresholds against primary CMS source]

On off-label promotion: *United States v. Caronia*, 703 F.3d 149 (2d Cir. 2012), and *Amarin Pharma v. FDA* (S.D.N.Y. 2015) protect truthful, non-misleading off-label speech in the Second Circuit; the national status is unsettled. A scientific-exchange lane exists under condition-laden FDA guidance. [verify specific guidance by name] **All of this is route-to-counsel, not a Fellow's call.** Routing rather than adjudicating is itself a professional competency.

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

<!-- → [TABLE: Handoff brief template — rows for each required field (test/build/kill, evidence level, assumptions, compliance flags, welfare check, replication required, kill criteria, Risk-1 statement), with a one-line description of what each answers. Caption: "The terminal governance artifact. A brief missing any row is not a handoff; it is a sales pitch."] -->

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

**LLM exercise (copy-paste prompt):**

> "I am writing a one-page product-handoff brief for a pharma analytics finding — a causal-forest estimate of how promotional meals affect prescribing, estimated on public CMS Open Payments plus Medicare Part D data. The brief must NOT give legal advice — it must FLAG regulatory questions and route them to counsel. Do three things: (1) draft a route-to-counsel section that enumerates every regulatory and compliance question this finding raises and for each names which framework it likely belongs to (PhRMA Code, FDA OPDP, or Physician Payments Sunshine Act); (2) do NOT answer any of the questions or state what the law requires — only flag and route; (3) add a one-line patient-welfare check. Mark anything you are unsure of as [verify]."

**CLI exercise.** Ask your CLI agent to scan a draft public repo (provided or synthetic) for firewall violations: occurrences of partner-specific field mappings, real performance numbers, or anything that only makes sense given proprietary data. Have it produce a redaction report. Then check its report by hand — it will miss context-dependent leaks and over-flag harmless synthetic values. The skill is auditing the audit.

**AI Validation exercise.** Validate any AI-drafted brief against three tests: every regulatory statement is flagged and routed, never adjudicated — reject any sentence that asserts what the law requires; every statute, case, or threshold carries a `[verify]` tag or a checked source; and the Risk-1 statement is present and names data access, partner framing, and consent ethics as unresolved. A brief that reads as confident and complete is suspect. This domain's honest output is hedged and routed.

**AI Use Disclosure**

*Write two sentences naming exactly what an AI tool did in your work for this chapter and what judgment you supplied that the AI could not. The judgment most specific to this chapter: a route-to-counsel flag you raised that the AI would have answered, a pathway or welfare exclusion you made on domain grounds, a [verify] tag you attached to a regulatory claim the AI stated as fact, or the Risk-1 statement you insisted on keeping when the model tried to soften it.*

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
