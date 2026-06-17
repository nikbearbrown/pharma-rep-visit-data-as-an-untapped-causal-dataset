# AI-Driven Pharma Marketing Research: A Framework for Discussion

*Bear Brown LLC and Humanitarians AI — for Graham Wilkinson, Doceree*

---

## What This Is

This is not a standard book. It is a **working framework** for a conversation we have had versions of before — at Kinesso, at IPG — now with one thing genuinely different: the AI tooling has made it possible to build, document, and staff a research program at a depth that was not practical before.

What you are looking at is two full courses, built and ready:

**Book A — *AI-Driven Programmatic HCP Marketing*** covers the full martech stack: NPI targeting, the evidence problem behind script-lift claims, model architecture (including what "Mixture of Experts" actually means versus what vendors mean by it), uplift and incrementality, brand association as a measurable outcome, and the external innovation lab operating model that governs how Fellows work with a partner.

**Book B — *Pharma Rep Visit Data as an Untapped Causal Dataset*** goes deeper on one dataset: the CLM telemetry that has been logging physician behavior for fifteen years without anyone asking a causal question of it. The training-cohort rollout is a natural experiment. The reps who have called on the same physicians forty times hold structural knowledge the data cannot contain. Together they make a causal study of pharmaceutical promotion buildable on one dataset — which is what these chapters build, end to end.

The difference from previous conversations is that these are not white papers or proposals. They are fully developed curricula with working code, synthetic datasets with known ground truth, exercise blocks, and a Fellows pipeline of 100–200 people who can staff a project within weeks. When we talked about doing this kind of work at Kinesso, the scaffolding had to be built from scratch each time. It already exists now.

---

## The Innovation Lab Offer

Bear Brown LLC and Humanitarians AI operate as an **external innovation lab**. The model is straightforward:

We scope the question, build a proof of concept on public or synthetic data, test whether it holds — and only then work alongside your team to implement anything that survives. Because Humanitarians AI runs Fellows across many disciplines, a project that needs extra hands for a few weeks can be staffed on a per-hour basis through the nonprofit rather than through Northeastern. Low commitment for you; high evidence before you build a team around it.

The contract that makes this safe is an **IP firewall**: Fellows work only on public data (CMS Open Payments, Medicare Part D, ICER value reports) and synthetic stand-ins. Your team replicates anything promising on proprietary data internally. Nothing confidential crosses into the published work. The firewall is not bureaucratic overhead — it is what makes it possible to publish honest findings, including critical ones, without exposing anything you own.

---

## Prescribing-Habit Persistence: A Worked Example

One project worth making concrete, because it sits at the intersection of what Doceree does and what the research can establish.

**The question:** how sticky is prescribing behavior across drug classes, and where is brand loyalty structurally real versus rented short-term attention?

The industry asserts that share-of-mind predicts share-of-market with a lag — that building mental availability today translates into prescribing tomorrow. This is the assumption behind a substantial fraction of HCP marketing spend. It has never been rigorously tested on linked survey-plus-claims data. It is an open research question, not an established fact.

**The study:** use Medicare Part D prescribing as a behavioral panel — physician by quarter by drug class. Measure how much a physician's prescribing in a given class predicts her prescribing in subsequent quarters, controlling for formulary, competitive activity, and specialty. Identify where persistence is structurally high (the physician has a durable patient-type rule: "for *this* kind of patient, this drug") versus where it is low (the physician is making a fresh decision each time, susceptible to the last rep visit or formulary change).

**Why it matters to Doceree:** a platform that delivers messages at the point of clinical decision has different value depending on where in the persistence distribution a physician sits. A loyalty-confirmed prescriber hearing the message at the moment she opens the chart is a sure thing — high conversion, zero increment. A physician whose prescribing pattern shows low persistence for this class, with a documented unresolved clinical question, is a persuadable. The message may actually move her.

The research can establish which physicians are which, using public data. Your platform can establish *when* they are in the chart. That combination is the product opportunity — and it is the thing neither party can build alone.

**What public data reaches:** an associational finding with heterogeneity — which physician and practice characteristics predict persistence, and where the variance is. Evidence Level 2 on the rubric we use. Causal identification of whether a specific message *caused* durable loyalty (as opposed to coinciding with it) requires the cohort-rollout natural experiment in the CLM data, which lives on your side of the firewall.

**Kill criterion (written before running):** if prescribing persistence turns out to be uniformly high across the physician population — if everyone is a loyalty-confirmed prescriber and there is no persuadable segment — the premise of the targeting optimization collapses and the project stops. That is a finding too, and it saves you from building infrastructure around an assumption that does not hold.

---

## The Candidate Project List

These are starting points, not commitments. Each project below links to the chapter that works it end to end **where it lives in this book**; projects that belong to the companion book are tagged so you can find them there.

**Track A — Lift and attribution**
- **Script-lift audit** — reconstruct a vendor-style lift number from public data (Open Payments promotion dose × Part D prescribing, joined on NPI), then re-estimate it with a credible design and report the gap: how much of the headline is selection — the platform aiming at physicians who would have prescribed anyway — versus genuine effect. — *(Book A)*
- **EHR-lift design memo** — specify the on/off health-system rollout that *would* identify point-of-care lift, with a power analysis for the pilot it would take. On public data this is a design, not a result, and saying so precisely is the contribution. — *(Book A)*
- **Channel decomposition** — attribute an observed prescribing change across promotion channels, with an explicit accounting of what cannot be separated without individual-level exposure data. — *(Book A)*

**Track B — Brand and physician identity**
- **Prescribing-habit persistence** *(detailed above)* — how sticky prescribing is across drug classes, and where brand loyalty is structurally durable versus rented short-term attention. — *(Book A)*
- **Share-of-mind → share-of-market** — test the industry's leading-indicator claim, that mindshare today predicts market share tomorrow, on linked survey-plus-claims data where it has never been rigorously checked. — *(Book A)*
- **Brand equity vs. clinical value** — does physician loyalty track ICER clinical-value scores once promotion intensity is controlled? An equity-not-value finding is a patient-welfare result, not only a marketing one. — *(Book A)*
- **Loss-of-exclusivity event study** — use generic entry as a natural shock to measure how much branded share survives, and whether the survivors were the equity leaders — the cleaner causal companion to the persistence study. — *(Book A)*

**Track C — Model architecture**
- **Ensemble vs. Mixture-of-Experts benchmark** — put a vendor "MoE" architecture against a well-tuned gradient-boosted baseline on an NPI-propensity task, judged on calibration, not just AUC. In the worked run the plain model wins — a "no" that saves an engineering build. — *(Book A)*
- **Physician archetypes** — are the physician "segments" a stable structure in the data, or a round number chosen to fit a slide? Tests whether the clustering geometry survives scrutiny. — *(Book A)*

**Track D — Ethics and accountability**
- **Proxy-discrimination test** — do targeting models quietly encode physician "susceptibility" or protected-correlated proxies rather than clinical need? — *(Book A)*
- **Co-pay coupon generic suppression** — apply the Dafny–Ody–Schmitt cross-state design to Doceree-relevant drug classes to estimate how much coupons suppress cheaper generics and raise system cost. Tests a partner product line, so framing is cleared first. — *(Book A)*
- **Accountability framework** — what monitoring, disclosure, and auditability a deployed targeting system should carry — the direct input to a patient-welfare gate. — *(Book A)*

**Depth practicum — rep-visit causal model**
- **Staggered DiD on message-variant transitions** — use the training-cohort rollout as a natural experiment to estimate what a *message variant* actually causes a physician to prescribe at 30/60/90 days, falsification test first. — *[Ch 10](10-staggered-did-message-variants.md)*
- **Meals causal forest** — per-physician heterogeneous effects of promotional meals on public Open Payments data, separating genuine education from reciprocity — runnable end to end today. — *[Ch 11](11-meals-causal-forest-and-clickstream-dml.md)*
- **Slide-sequence Double Machine Learning** — does slide order change prescribing? The cautionary case: the engagement signals you would naively control for are *caused* by the message, so conditioning on them breaks the estimate. — *[Ch 11](11-meals-causal-forest-and-clickstream-dml.md)*

---

## Where to Start

If one project needs to be concrete for a first conversation: **the meals causal forest** is the only fully public causal result in the set — it runs on CMS Open Payments and Medicare Part D, no partner data required, and produces a real heterogeneous-effect estimate today. **The script-lift audit** strikes at the number your clients are most likely to defend and reframes it as de-risking their own attribution claims. **The prescribing-habit persistence study** is the one that connects most directly to what Doceree's point-of-care platform is actually doing at the moment of clinical decision.

Everything above Evidence Level 3 — clean causal identification, deployed models, product features — is your team's to build. The lab's highest-value output is telling you exactly what proprietary work to fund, and what to kill before you build a team around it.

---

*Nik Bear Brown, PhD, MBA*
*Humanitarians AI · Bear Brown LLC*
*bear@bearbrown.co · humanitarians.ai*
ʕ •ﻌ•ʔ

## References (fact-check pass)

1. Dafny, L., Ody, C. & Schmitt, M., "When Discounts Raise Costs: The Effect of Copay Coupons on Generic Utilization," *American Economic Journal: Economic Policy* 9(2) (2017). [CONFIRMED — the named cross-state copay-coupon / generic-suppression design exists and is correctly attributed.]
2. CMS Open Payments; Medicare Part D; ICER value reports — named public data sources are real and correctly identified.
