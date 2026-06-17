# Source: Pharma Rep Visit Data as an Untapped Causal Dataset — "A Living Model Waiting to Be Built"

*Foundational source essay for this book (author-provided). The intellectual spine the TIKTOC and
chapters are built from. Companion framework: `living-models.md` (the full Living Model textbook).*

---

## Hook
Pharmaceutical companies have been running a massive natural experiment on physicians for fifteen
years. They have no idea. Every rep visit, the iPad logs everything — which slides, how many seconds
on each, the positive/neutral/negative reaction button, samples left, emails opened, questions —
timestamped, linked to rep ID, physician NPI, territory, training cohort. That is not a CRM. It is a
behavioral dataset dense enough to build a Living Model. Nobody is using it that way.

## What the data actually is
Veeva CRM (>80% of pharma field sales), two data planes:
- **Passive telemetry (CLM media player):** `Duration_vod` (seconds per slide), `Display_Order_vod`
  (navigation sequence — loops/skips), `Reaction_vod` (rep's +/neutral/− button per slide in real
  time), `Call_Channel_vod` (F2F/Teams/remote), `Call_Clickstream_vod` (packages it all at session end).
  Runs silently; rep does nothing.
- **Rep-entered post-call:** products detailed + priority, key messages delivered, samples, and the
  free-text call notes (the objection, the colleague mentioned, the formulary issue, the switch signal).
- **Warehouse layer appended:** NPI-level NRx from IQVIA Xponent / Symphony; CMS Open Payments
  meals/gifts; territory history; rep tenure; training-cohort timing.
Net: a physician-by-time panel — slide-level behavioral telemetry, rep-observed qualitative signal,
downstream NRx by NPI, and quasi-experimental variation in which messages were delivered when.

## The status quo is stuck on Rung 1 (Pearl's Ladder)
Content optimization (slide dwell/drop-off/reaction), rep performance (calls vs quota, causation
assumed), and NBA engines (XGBoost/collaborative filtering optimizing email opens/clicks/call
duration — a proxy, not NRx) are all **observational (Rung 1)**. The budget question is Rung 2:
P(NRx | do(message = safety-first)) vs P(NRx | do(message = efficacy-first)). The do-operator is in
no deployed pharma NBA engine. The data to estimate it has accrued for 15 years.

## The natural experiment in every training rollout
New clinical message (e.g., efficacy-first → safety-first) rolls out in **training cohorts** by
region/geography/hiring calendar — timing set by admin logistics, not physician characteristics.
Group A trained month 1 delivers the new deck; comparable Group B trained month 2 delivers the old
deck in that window. Physicians didn't choose their message; they got it because of when their rep
trained. **IV framework:** Treatment D = message variant exposure; Instrument Z = training-cohort
assignment; Outcome Y = NPI NRx at 30/60/90 days. Three conditions: **Relevance** (cohort predicts
delivery; first-stage F>10), **Independence** (cohort timing uncorrelated with physician
characteristics — falsify by regressing cohort on pre-rollout prescribing; a significant coefficient
kills it), **Exclusion** (cohort affects NRx only via message — threatened if early training also
improves selling skill; defend by controlling rep tenure + baseline performance).

## The data as a Living Model problem
Causal graph: training cohort (Z) → message variant (D) → knowledge/attitude (M1 educational) → NRx
(Y); plus D → relationship-maintenance signals (M2: samples, co-pay cards, follow-up) → Y; and Open
Payments meals/gifts (M3 reciprocity) → Y. **Three-pathway mediation**, each with different
ethical/regulatory status (educational = legally drivable; reciprocity = drives $40B promo;
relationship maintenance = samples/coupons). Rung-1 NBA can't separate them; a Living Model on the
parameterized graph estimates each pathway's direct contribution. **Collider warning:** `Duration_vod`,
reaction scores, in-visit signals look like confounders but are **post-treatment colliders**;
conditioning on them to estimate the total message→NRx effect is a structural error. **Counterfactual
layer (Rung 3):** the brand needs "for THIS physician, which message sequence next week?" —
individual counterfactual (abduction-action-prediction on a parameterized SCM), not a population IV.

## The epistemic gap (why = not in the data)
Telemetry captures what happened, not why. `Duration_vod`=47s on the safety slide could be (1)
compelled/reading carefully (+ educational), (2) skeptical/hunting the flaw (−, mis-coded neutral),
(3) polite while distracted (noise). Identical data, different causal inference. The experienced rep
knows which story is true (she's called on Dr. Martinez 40 times). That knowledge evaporates on
territory change and never reaches the model.

## The Causal Interview Bot
A chatbot. The rep talks like a rep; the system listens like a causal scientist. Structured on
Pearl's Ladder but the rep never hears "confounder/mediator/collider/counterfactual."
- **Rung 1 (association):** "Walk me through the last visit with Dr. Martinez — what did she say at the
  safety data?" → NLP extracts candidate confounders (formulary, competitive detailing, prior
  colleague switch, patient mix).
- **Rung 2 (intervention):** "If you led with patient-outcomes data instead of mechanism, what would
  happen? Anything that would *definitely* move her?" → extracts susceptibility indices (heterogeneous
  treatment effects not in any database).
- **Rung 3 (counterfactual):** "That account that converted — would it have without the MSL visit? The
  ones that didn't — what would you have done?" → maps to mediator weights (educational vs
  relationship vs peer-influence).
Output = a **structured prior**: a draft DAG with candidate nodes/edges, each annotated with rep
evidence, confidence, and contradictions to resolve. This is the **KEBN elicitation** step; reps are
the domain experts; closes Feigenbaum's knowledge bottleneck (CPT explosion, linguistic ambiguity,
recognition-explanation gap).

## Markov equivalence (why interviews are mathematically necessary)
Peer influence and NRx are correlated. Three Markov-equivalent stories: (A) peer influence → adoption;
(B) both effects of underlying openness (common cause); (C) high prescribers become influencers
(reverse). Observational data can't distinguish them (Verma–Pearl: data identifies the equivalence
class, not the graph). The rep's structural knowledge ("I've watched Dr. Johnson switch within two
weeks of a colleague; never the other way") orients the edge. The interviews are the source of
structural information, not supplementary data.

## Research agenda (3 projects)
1. **Staggered DiD on message-variant transitions** (most feasible, highest value): cohort rollout as
   instrument; link visits to NPI NRx panels; estimate ToT of message variant on 30/60/90-day NRx.
   Needs `Call2_Key_Message_vod__c`, cohort timing (HR), IQVIA Xponent NRx, territory FE.
   Falsification first: regress cohort on historical prescribing.
2. **Causal impact of promotional meals** (moderate): match Open Payments meals by NPI/time; causal
   forest for heterogeneous effects by specialty/baseline/competition; separate educational vs
   reciprocity pathway.
3. **Slide-sequence optimization via DML on CLM clickstream** (most complex, highest ceiling): treat
   `Call_Clickstream_vod` sequence as high-dim treatment; Double ML for the causal effect of ordering
   on NRx — but reaction/dwell/in-visit signals are post-treatment colliders; specify the SCM before
   the DML residualization or it corrupts the estimate.

## The targeting reframe — persuadables, not sure-things
NBA ranks high-propensity prescribers (the "sure things" — would prescribe anyway = waste). A Living
Model ranks **persuadables**: physicians for whom the right message at the right time *causes* a
change — often mid-tier prescribers with specific clinical concerns in markets peer influence hasn't
reached. Treatment-oriented frame: not "who's most likely to prescribe" but "for whom does detailing
*cause* prescribing." Different populations. Output ranks by causal responsiveness, not baseline volume.

## Consent architecture caveat
Veeva `CLM_EXPLICIT_OPT_IN_vod`: physician consent governs logging; opt-out either suppresses
clickstream or anonymizes (`CLM_OPT_OUT_BEHAVIOR_vod` 0/1, strips NPI from `Multichannel_Activity_vod`).
Systematic missingness correlated with physician characteristics (privacy-conscious, industry-skeptical,
institutional limits) = a Berkson/collider structure on the sampling frame. Ignoring it = conditioning
on a collider. Put the consent mechanism in the causal graph; use **FCI** (latent-variable-aware
discovery) not PC/GES; the drift monitor watches opt-out-rate shifts that change the bias structure.

## Production / framing notes
The TikTok hook: "pharma's been running a 15-year natural experiment on physicians and has no idea —
the rep talks like a rep, the AI listens like a causal scientist, the data was always there, nobody
asked the right questions." The defensible version: "every NBA engine in pharma answers the wrong
question; here's the architecture for the right one." Has legs as a conference talk, white paper, and
a **pitch to Graham Wilkinson at Doceree about what the Fellows project actually builds.** The DAG
(training cohort → message variant → physician → prescription), animated in three steps, is the image
that must land.
