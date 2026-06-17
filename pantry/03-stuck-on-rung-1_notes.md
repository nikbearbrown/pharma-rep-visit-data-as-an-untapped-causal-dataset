# Chapter 3 Notes — Stuck on Rung 1

*Research-gathering notes for Week 3 (Act I). Pearl's Ladder of Causation; why the rungs are
"sealed"; why the pharma analytical status quo — content optimization, rep-performance management,
and Next-Best-Action engines — lives entirely on Rung 1; the proxy-substitution error; the
47-second-slide ambiguity as the seed of the epistemic-gap argument; the Rung-2 budget question in
do-notation. Notes only — not chapter prose. Every claim sourced or `[verify]`.*

Primary sources: `pantry/rep-visit-as-causal-dataset_source.md` ("The status quo is stuck on Rung 1")
and TIKTOC Week 3 entry. Conceptual spine: `pantry/_lib_science-the-book-of-why-the-new-science-of-
cause-and-effect.md` (Pearl & Mackenzie, *The Book of Why*, Ch. 1, "The Ladder of Causation").

---

## A. Foundations

**Pearl's Ladder of Causation — the three rungs.** Pearl frames causal reasoning as three tiers of
*queries* (after Turing: classify a system by what it can answer), each strictly more powerful than
the one below. He names them seeing, doing, imagining.
- **Rung 1 — Association ("What if I *see*?").** Detect regularities in passive observation. The
  defining object is the conditional probability P(Y | X) — "the probability of Y given that you see
  X." Pearl's running example: a store manager asking "how likely is a customer who bought toothpaste
  to also buy floss?" Correlation/regression are Rung-1 tools. *Source: Book of Why lib file, Ch. 1,
  "At the first level, association, we are looking for regularities in observations… P(floss |
  toothpaste)."*
- **Rung 2 — Intervention ("What if I *do*?").** Predict the effect of a deliberate action that
  *changes* the world. Written P(Y | do(X)). Pearl: "what will happen to our floss sales if we
  *double the price* of toothpaste?" — distinct from "what happened on past occasions when the price
  was high," because past high prices arose for confounded reasons (shortages, market-wide hikes).
  *Source: same, "We step up to the next level… intervention… P of floss given do toothpaste."*
- **Rung 3 — Counterfactual ("What if I had done otherwise?").** Reason about a specific unit in a
  world contrary to fact: "my headache is gone — was it the aspirin?" Requires going back, negating
  an observed fact, and re-running. *Source: same, "the top rung… the level of counterfactuals."*

**Why the rungs are "sealed" (the load-bearing claim of the chapter).** Pearl proves the levels
differ *fundamentally* — each unlocks queries the rungs below cannot answer. The operational
statement: **"We cannot answer questions about interventions with passively collected data no matter
how big the data set or how deep the neural network."** Rung-1 data alone cannot climb to Rung 2.
The *only* bridge is a causal model: "A sufficiently strong and accurate causal model can allow us to
use rung-1 observational data to answer rung-2 interventional queries. Without the causal model, we
could not go from rung 1 to rung 2." This is why Pearl places present-day deep learning "squarely on
rung one… sharing the wisdom of an owl." *Source: Book of Why lib file, Ch. 1 (verbatim above).*

**The do-operator.** do(X = x) denotes an *external* manipulation that forces X to value x,
**replacing** X's structural equation with the constant and **severing all incoming arrows** to X
(the other mechanisms stay intact). The causal effect is E[Y | do(X = x)], which differs in general
from the observational E[Y | X = x]. Pearl introduced the operator in 1995 and developed do-calculus
(the rule set for deciding when a do-expression is identifiable from observational data + a graph) in
*Causality* (Cambridge UP; 1st ed. 2000, 2nd ed. 2009). *Sources: Pearl & Mackenzie, The Book of
Why (2018); Pearl, Causality, 2nd ed. (2009); do-calculus overview, https://en.wikipedia.org/wiki/
Do-calculus.* [verify exact page for the do(X=x) definition in Causality 2nd ed. before print.]

**The Rung-2 question budget actually needs (the chapter's destination, in do-notation).** The brand
allocating a message budget is not asking a Rung-1 question; it is asking a Rung-2 one:
> **P(NRx | do(message = safety-first)) vs P(NRx | do(message = efficacy-first)).**
Which *deliberately assigned* message produces more new prescriptions? *Source: rep-visit source
essay, "The status quo is stuck on Rung 1."* **The flagged claim:** the source essay asserts "the
do-operator is in no deployed pharma NBA engine." Soften to a sourced/`[verify]` claim — vendor
methods are proprietary, so this is plausible and consistent with how the platforms describe
themselves (Sec. B), but not externally provable. Frame as "no major vendor *describes* its engine as
estimating a do-expression; all describe engagement-proxy optimization." [verify — proprietary.]

**Common misconception (fix it explicitly).**
> "Prediction + personalization = causal targeting."
Wrong. A model that predicts P(NRx | X) extremely well, then personalizes the action, is still on
Rung 1: it ranks *who is likely to prescribe*, not *for whom the message causes prescribing*. A
high-predicted-NRx physician may be a "sure thing" who prescribes regardless (zero incremental
effect). The forecast-vs-intervention gap is Pearl's whole point. *Bridge to Ch 12 (persuadables).*

---

## B. The pharma status quo is all Rung 1 (with vendor confirmation)

The source essay names three Rung-1 practices. Each is verified below against how the field actually
describes itself.

**(a) Content optimization — slide dwell / drop-off / reaction.** Veeva CLM telemetry
(`Duration_vod` seconds-per-slide, `Display_Order_vod` navigation, `Reaction_vod` rep +/0/− button)
is mined for which slides hold attention and earn positive reactions. This is pure association: what
*co-occurs* with engagement, never what engagement *causes* in NRx. *Source: rep-visit source essay,
"What the data actually is."* (And: dwell/reaction are not just Rung-1 — Ch 6 shows they are
post-treatment **colliders**, so conditioning on them to get the message→NRx effect is a second,
distinct structural error. Flag the forward link, don't develop it here.)

**(b) Rep-performance management — calls vs quota, causation assumed.** Reps are measured on call
volume against quota and the lift is *attributed* to their calling. This silently assumes
calls → prescribing, the canonical Rung-1-masquerading-as-Rung-2 error: the high-converting rep may
simply cover already-loyal physicians. *Source: source essay; classic incrementality critique, Sec.
D (Lewis & Rao).*

**(c) Next-Best-Action engines — XGBoost / collaborative filtering optimizing engagement proxies.**
This is the heart of the chapter. NBA platforms recommend message/channel/timing per HCP. The
**verified** point: they optimize *engagement proxies* (email opens, click-throughs, call duration,
visit acceptance, next-prescription probability), not the causal effect of the action on prescribing.
- **Aktana** (Contextual Intelligence Engine, ML + business rules): published an ML method in the
  *Journal of the PMSA* for "identifying message sequences that maximize **open and click-through
  rates**," predicting "the probability of an email being opened or clicked through." Self-describes a
  decade of refinement over "100M+ field suggestions." (Acquired by PharmaForceIQ, Jan 2026.)
  *Sources: https://www.aktana.com/blog/aktanas-machine-learning-method-open-rate-results-published-
  in-pmsa-journal/ ; https://www.aktana.com/resources/case-study-nba-activation/*
- **IQVIA Next Best Action** (part of "Orchestrated Analytics"): "omnichannel orchestration and
  role-specific recommendations," "dynamic call planning," AI/ML + generative AI for engagement —
  framed throughout as engagement optimization, not prescribing causation. *Source: https://
  www.iqvia.com/solutions/commercialization/commercial-analytics/orchestrated-analytics/next-best*
- **Indegene NBA** ("powered by orchestration and decision engines"): "right message, right channel,
  right time," reports "campaign engagement" uplift (a top-10 pharma: "+25% campaign engagement") —
  again an engagement metric, not an NRx-causal estimate. *Sources: https://www.indegene.com/what-we-
  think/blogs/next-best-action-in-pharma ; https://www.indegene.com/what-we-think/case-studies/
  improve-campaign-engagement*
- **Analyst framing:** IntuitionLabs' NBA guide + platform comparison describe the category as
  engagement orchestration / decisioning. *Sources: https://intuitionlabs.ai/articles/next-best-
  action-pharma-guide ; https://intuitionlabs.ai/articles/ai-hcp-engagement-platforms-pharma-
  comparison* [verify — analyst, not primary.]

**The takeaway to land:** across the three biggest vendors the optimization *target* is an
engagement proxy. None publicly claims to estimate P(NRx | do(message)). That is the empirical basis
for the chapter's "no do-operator in deployed NBA" claim — stated as a sourced inference, not a proof.

**The proxy-substitution error (why engagement-as-prescribing-stand-in fails).** Substituting an
easy-to-measure proxy (opens, clicks, duration) for the hard target (incremental NRx) breaks for
three reasons: (1) **the proxy can be uncorrelated-with or anti-correlated-with the target** — an
open-rate model ranks physicians who *like email*, not physicians whose *prescribing changes*; (2)
**optimizing a proxy degrades it as a measure** (Goodhart / Campbell: once a metric becomes a target,
it stops measuring what it did — [verify exact Goodhart phrasing/cite before print]); (3) the proxy
sits on a different rung — even a *perfect* open-rate predictor answers a Rung-1 question and is mute
on the Rung-2 budget question. *Sources: source essay; Bottou et al. 2013 (Sec. D) is the formal
treatment — optimizing measured response in an ad system overstates causal value; the same logic
transfers directly to HCP targeting.*

**Worked example (the chapter's opening / failure case).** An NBA engine optimizes email-open rate —
"a proxy someone decided was 'close enough' to prescribing" (TIKTOC). It surfaces physicians who open
everything (engaged, but would prescribe anyway = "sure things"). Measured conversion looks high;
*incremental* conversion is near zero. The budget is spent confirming loyalty, not creating it. The
right population — **persuadables** — is invisible to a Rung-1 ranker. *Forward to Ch 12.*

---

## C. The 47-second-slide ambiguity (seed of the epistemic gap)

A single observation — `Duration_vod` = 47 s on the safety slide — is consistent with at least three
incompatible causal stories: (1) **compelled**, reading carefully (positive educational signal);
(2) **skeptical**, hunting for the flaw (negative, and often mis-coded "neutral" by the rep button);
(3) **polite while distracted** (noise). *Identical telemetry, opposite causal inferences.* Telemetry
records *what happened*, never *why*. The experienced rep who has called on Dr. Martinez 40 times
knows which story is true — but that knowledge is not in any field of the database, and it evaporates
on territory change. *Source: rep-visit source essay, "The epistemic gap."* This is the seed of the
book's central argument and the explicit forward bridge: the gap motivates the Causal Interview Bot
(Ch 8) and the structural-knowledge-is-necessary result (Ch 7). Plant it here; do not resolve it.

---

## D. Current state / key refs / last few years

**Settled.** Causal (interventional) effects require a counterfactual/interventional comparison;
observational correlation alone cannot establish an intervention effect (Pearl's rung-separation
theorem). *Source: Book of Why; Pearl, Causality (2009).*

**The keystone applied citation — KEEP, it is real and apt.**
- **Bottou, Peters, Quiñonero-Candela, Charles, Chickering, Portugaly, Ray, Simard & Snelson (2013),
  "Counterfactual Reasoning and Learning Systems: The Example of Computational Advertising," *Journal
  of Machine Learning Research* 14:3207–3260.** Preprint **arXiv:1209.2355**. Demonstrated on Bing
  ad placement: a system that optimizes *measured* engagement is not estimating the causal effect of
  its own actions; counterfactual/causal estimation is required to predict the consequences of system
  changes. The direct intellectual ancestor of the pharma-NBA critique. *Verified citation:
  https://jmlr.org/papers/volume14/bottou13a.html ; https://arxiv.org/abs/1209.2355 ;
  https://leon.bottou.org/papers/bottou-jmlr-2013* (Existing notes cited arXiv:1209.2355 correctly —
  retained, now with full author list + JMLR vol/pages.)

**Advertising incrementality (the analogue literature — measured ≠ causal returns):**
- **Lewis & Rao (2015), "The Unfavorable Economics of Measuring the Returns to Advertising," *QJE*
  130(4):1941–1973** — even very large observational samples can't pin down ad ROI; experiments
  needed. [verify exact pages.]
- **Gordon, Zettelmeyer, Bhargava & Chapsky (2019), "A Comparison of Approaches to Advertising
  Measurement: Evidence from Big Field Experiments at Facebook," *Marketing Science* 38(2):193–225**
  — observational methods diverge sharply from RCT ground truth. [verify exact pages.]
  *Both transfer to HCP targeting: observational ad-response overstates true causal return.*

**Public-data analogue of rep influence (drug-company meals → prescribing):**
- **Newham & Valente (2024), "The Cost of Influence: How Gifts to Physicians Shape Prescriptions and
  Drug Costs," arXiv:2203.01778** — Open Payments + ML heterogeneous-effect estimation; a
  public-data stand-in for rep influence. *Source: https://arxiv.org/abs/2203.01778.* [verify final
  venue/year — currently a working paper/preprint.] *(This is Project 2's basis, Ch 11.)*

**Contested / emerging.** How much commercial NBA is genuinely causal vs predictive is **opaque** —
vendor methods are proprietary, so the "no do-operator deployed" claim rests on public self-
descriptions (Sec. B), not disclosure. Causal ML (causal forests, DML, uplift modeling) has made
heterogeneous-effect estimation accessible, but the *measurement design* (a valid identification
strategy) remains the scarce input, not the algorithm. *Source: source essay; Ch 4/10/11 forward.*

---

## E. Teaching

- **The do-operator feels abstract until it's a budget decision.** Anchor it immediately to
  P(NRx | do(message=safety-first)) vs P(NRx | do(message=efficacy-first)) — the literal question a
  brand manager funds. Pearl's "double the price of toothpaste" is the warm-up; the message-variant
  budget is the load-bearing instance.
- **Classification drill (per TIKTOC LO "Analyze").** Give 10 real pharma metrics (open rate, call
  attainment vs quota, slide dwell, reaction score, visit acceptance, predicted next-Rx propensity,
  channel-mix lift, sample-to-Rx ratio, share-of-voice, NBA acceptance rate) and have Fellows tag
  each Rung 1 / 2 / 3 and justify. Most land on Rung 1; the point is felt, not told.
- **State the Rung-2 question (per TIKTOC LO "Evaluate").** Deliverable: write the do-expression the
  budget needs for one named decision, and explain why the engagement metric currently optimized
  cannot answer it.
- **Analogy (keep — it's exactly right).** A forecast predicts rain; carrying an umbrella does not
  *cause* rain. Prediction (Rung 1) and intervention (Rung 2) are different questions. Pearl's own
  "seeing smoke vs making smoke" is the sharper version — use both.
- **Where students get stuck:** conflating a great predictor with a great policy. Use the
  sure-thing/persuadable split (Ch 12 preview) to make the failure concrete and dollar-denominated.

---

## F. Library files (in `pantry/`)

- `_lib_science-the-book-of-why-the-new-science-of-cause-and-effect.md` — **primary conceptual
  source.** Ch. 1 "The Ladder of Causation" gives the three rungs, the seeing/doing/imagining frame,
  the toothpaste-floss examples, P(floss | do(toothpaste)), and the rung-separation argument
  ("no machine can derive explanations from raw data — it needs a push"). All Sec. A verbatim quotes
  are sourced here.
- `_lib_counterfactuals-book-of-why_excerpt.md` — focused excerpt on the counterfactual rung (Rung 3
  detail; supports the Ch 7/9 forward bridge).
- `_lib_math-causal-inference.md` and `_lib_math-causal-inference-mit-press-essential-knowledge-
  series.md` — do-operator, intervention vs observation, association-vs-causation at the conceptual
  level (good for the classification drill).
- `_lib_science-calling-bullshit-the-art-of-skepticism-in-a-data-driven-world.md` and
  `_lib_math-weapons-of-math-destruction...md` — proxy-metric / Goodhart-style failure intuition
  (engagement-as-stand-in critique, Sec. B).
- (Note: the legacy MD library does **not** contain Bottou et al. 2013, Lewis & Rao, Gordon et al.,
  or the vendor NBA descriptions — those are web-sourced in Secs. B & D and must be cited directly.)

---

## G. Gaps / flags

- **[verify — proprietary]** "The do-operator is in no deployed pharma NBA engine." Soften from a
  universal to a sourced inference: no major vendor (Aktana, IQVIA, Indegene) *publicly describes*
  its engine as estimating P(NRx | do(message)); all describe engagement-proxy optimization. Vendor
  internals are closed, so the claim is plausible, not proven. State it that way in the chapter.
- **[verify]** Exact page for the do(X=x) operator definition in Pearl, *Causality* (2nd ed., 2009),
  and the 1995 origin date for do-calculus (cited from standard knowledge; re-check originals).
- **[verify]** Goodhart/Campbell exact phrasing + citation for the proxy-substitution point
  (Goodhart 1975; Campbell 1979) before using a direct quote.
- **[verify]** Exact pages for Lewis & Rao (2015, *QJE*) and Gordon et al. (2019, *Marketing
  Science*); final venue/year for Newham & Valente (arXiv:2203.01778 — working paper as of notes).
- **Aktana corporate status:** acquired by PharmaForceIQ Jan 2026 — keep the *method* citation (PMSA
  open-rate paper) which stands regardless of the acquisition; flag the org change if naming the
  vendor in present tense.
- **Tension to flag for the author:** dwell/reaction appear in this chapter as Rung-1 content metrics
  but are *also* post-treatment colliders (Ch 6). Decide whether to plant the collider seed here
  (one sentence) or hold it entirely for Ch 6. Recommend: one forward-pointing sentence, no mechanism.
- **Adoption Risk tie-in:** the "engagement proxy ≠ prescribing" claim is the conceptual root of the
  whole book's value proposition — make sure the chapter's proxy-substitution argument is airtight
  and sourced (Bottou), since later chapters depend on the reader having accepted it.
