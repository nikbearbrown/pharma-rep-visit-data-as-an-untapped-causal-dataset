# Pharma Rep Visit Data as an Untapped Causal Dataset
## A Living Model Waiting to Be Built

**Working title:** Pharma Rep Visit Data as an Untapped Causal Dataset — A Living Model Waiting to Be Built
**Series:** Irreducibly Human / AI+1 · Living Model family · Northeastern University College of Engineering
**Author:** Nik Bear Brown · Humanitarians AI (publisher) · with the Humanitarians AI Fellows
**Document:** Full TOC Draft — compiled from phase outputs, grounded in `pantry/`
**Version:** 1.0
**Status:** Pre-draft — proprietary-data access + partner framing + consent ethics unresolved (Risk 1, blocker)

---

## Document structure
1. Book Concept and Thesis · 2. Learner Profile · 3. Book Type and Deployment · 4. Field Positioning ·
5. Three-Act Learning Arc · 6. Prerequisite Map · 7. Assessment Structure · 8. Chapter-by-Chapter TOC ·
9. Chapter Anatomy · 10. Case Study Strategy · 11. Hard Topics / Contested / Aging · 12. Market ·
13. Feature List · 14. Out of Scope · 15. Adoption Risk Register · 16. Open Questions

---

# PART 1 — BOOK CONCEPT AND THESIS

## Book concept summary
> This book teaches **data-capable Humanitarians AI Fellows to turn pharma rep-visit telemetry into a
> Living Model — a parameterized causal model of what messages actually cause prescribing** — by
> reading the data for the natural experiment hidden in training-cohort rollouts, building the causal
> graph (and avoiding its collider traps), extracting the rep's structural knowledge through a Causal
> Interview Bot to orient the edges the data cannot, and shipping targeting that ranks physicians by
> **causal responsiveness** rather than baseline prescribing. It is the *applied-dataset practicum* of
> the Living Model family. It succeeds if the Fellow can take rep-visit data (real or synthetic),
> state an identification strategy, estimate a defensible message→prescribing effect, and hand a
> partner a model that answers a Rung-2/Rung-3 question instead of a Rung-1 correlation.

## One-sentence logline
Pharma has run a 15-year natural experiment on physicians and never analyzed it — the rep talks like a
rep, the system listens like a causal scientist, and the data was always there.

## Central thesis
"Every Next-Best-Action engine in pharma answers a Rung-1 question (what correlates with engagement)
when the decision that allocates $40B of promotion is a Rung-2/Rung-3 question (what message *causes*
prescribing, for *which* physician) — and the rep-visit dataset already contains both the
quasi-experimental variation to identify the effect and, through the reps, the structural knowledge to
orient the causal graph the data alone cannot (Verma–Pearl). The gap between what is done and what is
possible is an architecture problem: a Living Model, elicited from reps and identified from the natural
experiment, not a bigger predictive model."

## Thesis test
- **Act One** establishes the dataset, why the status quo is stuck on Rung 1, and the natural
  experiment hiding in every training rollout. ✓
- **Act Two** builds the causal model: the three-pathway graph, the collider traps (incl. consent),
  why the data can't decide (Markov equivalence), the Causal Interview Bot, and the Living Model
  architecture. ✓
- **Act Three** runs it: the three research projects, the persuadables-vs-sure-things reframe, and the
  consent/compliance/IP-firewall handoff to the partner. ✓

---

# PART 2 — LEARNER PROFILE

## Primary reader
A **Humanitarians AI Fellow** — a data-capable graduate student / early-career data scientist — working
with a pharma marketing-technology partner. Comfortable with Python, ML, and reading a paper; **not** a
pharma commercial insider and **not** a senior causal-inference researcher (the book teaches the causal
moves it needs on this one dataset).

**Specific person:** a second-year MS Fellow handed a pharma CRM extract (or a synthetic stand-in) and
a vendor "AI NBA engine," who can see the engine optimizes email opens and senses that's the wrong
target, and wants to build the thing that estimates what a message actually *causes*.

## Prior knowledge assumed
- Python + ML basics (regression, trees/boosting, train/test, calibration)
- Comfort reading a dataset dictionary and a methods paper
- General AI literacy

## Prior knowledge NOT assumed (taught here, on this dataset)
- Pearl's ladder, the do-operator, DAGs — re-established (companion: *Causal Reasoning* / Living Model book)
- Instrumental variables, difference-in-differences, double ML, mediation, colliders, Markov
  equivalence/Verma–Pearl, FCI — taught at the depth this dataset requires
- KEBN expert elicitation; parameterized SCMs and counterfactual reasoning
- Pharma commercial mechanics (Veeva CLM, NPI, NRx, Open Payments, MLR) — taught

## Prior misconceptions
1. "A model that predicts prescribers well tells you who to detail." — propensity ≠ persuadability;
   the sure-things are waste (Ch 12).
2. "Slide dwell time / reaction score is a useful feature." — they're **post-treatment colliders**;
   conditioning on them biases the message effect (Ch 6).
3. "If a tool's DAG fits the data, it's the right DAG." — Markov equivalence: data identifies a class,
   not a graph (Ch 7).
4. "Rep notes are soft/anecdotal." — they're the only source of the structural knowledge that orients
   edges the data can't (Ch 7–8).
5. "We can just A/B test it." — often slow/expensive/ethically constrained; the natural experiment is
   already in the data (Ch 4).

## Motivation type
Professional/research (build something a partner could resource) + academic. Terminal deliverable: a
Living-Model targeting prototype + a product-handoff brief, not a textbook exercise.

## Prerequisite map
| Prerequisite | Safe to assume? | Where addressed |
|---|---|---|
| Python + ML basics | Yes | — |
| Pearl's ladder / do-operator | Probably not | Ch 3 (re-established) |
| IV / DiD / DML / mediation / colliders | No | Ch 4, 5, 6, 10, 11 (primary instruction) |
| Markov equivalence / Verma–Pearl / FCI | No | Ch 7, 13 |
| KEBN elicitation / parameterized SCM | No | Ch 8, 9 |
| Veeva CLM / NPI / NRx / Open Payments / MLR | No | Ch 2 (+ throughout) |

**Front-loading:** the dataset + the ladder go first (Ch 2–3); each causal method is taught at first
use on this dataset, not as a standalone theory course.

---

# PART 3 — BOOK TYPE AND DEPLOYMENT

**PRIMARY:** Fellows lab / practitioner monograph — a focused, single-dataset causal practicum that
ends in a built model + handoff brief.
**SECONDARY:** graduate seminar module (applied causal inference / health analytics; chapter = week).
**NOT:** a general causal-inference textbook (it teaches only the moves this dataset needs), a pharma
marketing survey, or a Veeva admin manual.

**Deployment:** Humanitarians AI fellowship cohorts collaborating with a pharma martech partner
(Doceree); secondary as a graduate seminar. **Signals to a reviewer:** three labeled acts
(Dataset / Model / Run It), a research-finding per chapter, three runnable projects, a terminal
Living-Model prototype + handoff brief.

**The two defining constraints (present from Ch 2):**
- **Proprietary data + IP firewall.** The core telemetry is Veeva CRM data the partner owns. The book
  teaches and tests on **public data (Open Payments, Part D) and synthetic rep-visit data**; the
  partner replicates on real CRM data internally. Nothing proprietary enters the public book/repo;
  sensitive material lives in `private/` (gitignored).
- **Consent + welfare are load-bearing, not an appendix.** Physician opt-out creates a Berkson/collider
  selection structure (Ch 6, 13); the educational pathway is the only legally drivable one (Ch 5, 13).

---

# PART 4 — FIELD POSITIONING

This book sits inside a deliberate family and fills a clean gap:
- **The Living Model book** (`pantry/living-models.md`) is the *framework* — drift, latency, risk-as-two-
  numbers, Pearl's ladder, the parameterized-SCM/counterfactual/monitoring architecture. This book is
  its **applied case** on one killer dataset.
- **Causal Reasoning: Irreducibly Human** is the *identification practicum* (DAGs, the identification
  layer in the abstract). This book is the *dataset practicum* — the same logic on rep-visit telemetry.
- **Pearl** made the argument; **Cunningham / Hernán–Robins** teach methods to economists/epidemiologists.
  None teach a Fellow to turn *this* dataset into a deployed causal targeting model.
- **Pharma NBA vendor material** claims AI but operates on Rung 1 (engagement proxies). This book shows
  why that's the wrong question and builds the right one.

**Positioning:** *For Fellows building causal models on pharma commercial data with a martech partner,
this is the applied Living-Model practicum that turns rep-visit telemetry into causal targeting —
unlike NBA vendor tools that optimize engagement proxies on Rung 1, and unlike general causal texts
that never touch this dataset or the rep-knowledge elicitation it requires.*

---

# PART 5 — THREE-ACT LEARNING ARC

**Arc statement:** takes the reader from *a Fellow who sees rep-visit data as a CRM log* to *a Fellow
who can build a Living Model that ranks physicians by causal responsiveness*, by first reading the
dataset and the natural experiment in it (Act I), then building and identifying the causal model —
including eliciting the rep knowledge the data can't supply (Act II), then running the projects and
shipping the targeting + handoff under the consent/IP constraints (Act III).

- **Act I — The Untapped Dataset (Ch 1–4):** what the data is, why the status quo is stuck on Rung 1,
  the training-cohort natural experiment. *Transition:* the Fellow can name the do-question and the
  instrument that identifies it.
- **Act II — Building the Causal Model (Ch 5–9):** three-pathway graph, collider traps (incl. consent),
  Markov equivalence (why interviews are necessary), the Causal Interview Bot, the Living Model. *Transition:*
  the Fellow has a parameterized SCM elicited from reps + identified from the experiment.
- **Act III — Running It (Ch 10–13):** the three projects, persuadables targeting, consent/compliance/
  firewall + the Fellows→partner handoff. *Exit:* a built prototype + a defensible handoff brief.

**Pebble-in-the-pond:** Ch 1 puts the logging iPad and the unrun experiment on the page; the Fellow's
own thread (a project track + a synthetic or public dataset) is chosen by Ch 4 and carried to the brief.

---

# PART 6 — PREREQUISITE MAP (dependency chain)
| Ch | Depends on | Note |
|---|---|---|
| 1 | — | Hook: the data as an unrun experiment |
| 2 | 1 | The Veeva CLM schema + the assembled panel |
| 3 | 2 | Pearl's ladder; why the status quo is Rung 1 |
| 4 | 3 | The training-cohort natural experiment / IV |
| 5 | 4 | The three-pathway causal graph |
| 6 | 5 | Collider traps (dwell/reaction; consent) |
| 7 | 5,6 | Markov equivalence → why rep knowledge is required |
| 8 | 7 | The Causal Interview Bot (KEBN elicitation) |
| 9 | 8 | Building the parameterized SCM + counterfactual engine + drift |
| 10 | 4,9 | Project 1: staggered DiD on message variants |
| 11 | 6,9 | Projects 2 & 3: meals (causal forest) + clickstream (DML) |
| 12 | 9 | Persuadables vs sure-things targeting |
| 13 | all | Consent/compliance/IP firewall + handoff |

**Load-bearing:** Ch 4 (the identification strategy), Ch 6 (the collider discipline), Ch 7–8 (why and
how rep knowledge enters). Skipping any breaks Act III.

---

# PART 7 — ASSESSMENT STRUCTURE
| Component | Points | Weeks |
|---|---|---|
| Reading responses (4 × 30) | 120 | 1, 3, 6, 7 |
| Weekly lab exercises (7 × 25) — code/CLI on public or synthetic data | 175 | 2–6, 10, 11 |
| Midterm: identify-the-graph + falsification design | 100 | 4 |
| Causal Interview Bot transcript + elicited prior DAG | 105 | 8 |
| Project run (one of the three, on public/synthetic data) | 200 | 11 |
| **Final: Living-Model targeting prototype + product-handoff brief** | 200 | 13 |
| DAG-defense workshop (continuous) | 100 | — |
| **Base total** | **1000** | |
| Own-thread bonus (up to 7 × 5) | +35 | per lab |

**Bonus rule:** a lab run on the Fellow's own thread earns +5 when the AI Use Disclosure names one
edge orientation or assumption that required human/rep structural knowledge no dataset could supply.

---

# PART 8 — CHAPTER-BY-CHAPTER TOC

## ACT ONE — THE UNTAPPED DATASET (Weeks 1–4)

### WEEK 1 — The Experiment Nobody Knows They're Running
**One-line:** Fellows learn to see rep-visit telemetry as a dense behavioral *and* causal dataset, not
a CRM log.
**LOs:** (Understand) describe what the iPad logs and why it's causal-grade; (Analyze) distinguish a
behavioral signal from a causal claim in this data; (Apply/own-thread) state the do-question your
project will answer.
**Opening:** the iPad mid-visit — `Duration_vod`/`Reaction_vod` logging silently — and the claim that
pharma has run a 15-year natural experiment it never analyzed.
**Core:** what the data is at altitude; the gap between "cool dataset" and "causal dataset"; the book's
spine (Rung-1 status quo vs the Rung-2/3 question).
**Research finding:** the whole dataset class is unanalyzed causally — the open territory.
**Bridge:** to use it, be precise about what it contains → Ch 2.

### WEEK 2 — What the Data Actually Is
**One-line:** Fellows learn the Veeva CLM schema and how the physician-by-time panel is assembled.
**LOs:** (Understand) the two CLM data planes + the warehouse layer; (Apply) assemble a (synthetic)
physician-by-time panel; (Analyze) identify which fields are treatment, outcome, instrument, covariate,
or collider.
**Opening:** the `Call_Clickstream_vod` payload written at session end — read it field by field.
**Core:** passive telemetry (`Duration_vod`, `Display_Order_vod`, `Reaction_vod`, `Call_Channel_vod`);
rep-entered (`Call2_Key_Message_vod__c`, samples, free-text notes); warehouse (IQVIA/Symphony NRx,
Open Payments, cohort/tenure). **Research finding:** the four ingredients of a causal panel are all
present. **Bridge:** with the data mapped, why is nobody using it causally? → Ch 3.

### WEEK 3 — Stuck on Rung 1
**One-line:** Fellows learn Pearl's ladder and why content optimization, rep-performance, and NBA
engines are all observational.
**LOs:** (Understand) the three rungs + the do-operator; (Analyze) classify a current pharma analytic
practice by rung; (Evaluate) state the Rung-2 question budget actually needs.
**Opening:** an NBA engine optimizing email-open rate — a proxy someone decided was "close enough" to
prescribing.
**Core:** the ladder; why observational metrics can't answer do-questions; the 47-second-slide
ambiguity (compelled vs skeptical vs polite). **Research finding:** the do-operator is in no deployed
pharma NBA engine. **Bridge:** the data has the variation to answer it — where? → Ch 4.

### WEEK 4 — The Natural Experiment in Every Training Rollout
**[MIDTERM]**
**One-line:** Fellows learn to use staggered training-cohort rollout as an instrument and to test its
assumptions.
**LOs:** (Apply) set up the IV (treatment/instrument/outcome) for a message transition; (Analyze) test
relevance/independence/exclusion; (Create) design the falsification test.
**Opening:** Group A (trained month 1, new deck) vs Group B (trained month 2, old deck) — physicians
got the message their rep's training schedule gave them.
**Core:** IV framework; the three conditions (relevance F>10; independence — falsify by regressing
cohort on pre-rollout prescribing; exclusion — control tenure + baseline); when each fails.
**Assessment:** Midterm (identify the graph + design the falsification test). **Bridge:** the
experiment identifies an *average* effect — but the effect runs through several pathways → Ch 5.

## ACT TWO — BUILDING THE CAUSAL MODEL (Weeks 5–9)

### WEEK 5 — The Causal Graph and the Three Pathways
**One-line:** Fellows learn to draw the message→prescribing graph as a three-pathway mediation and why
the pathways matter ethically and commercially.
**LOs:** (Apply) draw the educational / reciprocity / relationship-maintenance pathways; (Analyze)
predict why conflating them misleads investment; (Evaluate) tie each pathway to its regulatory status.
**Opening:** two visits with identical NRx lift — one drove it through education, one through a meal —
and why that difference is everything.
**Core:** M1 educational (legally drivable), M2 relationship maintenance (samples/coupons), M3
reciprocity (the $40B pathway); mediation vs total effect. **Research finding:** Rung-1 NBA cannot
separate the pathways; a Living Model can. **Bridge:** but some "features" you'd condition on are traps.

### WEEK 6 — The Collider Trap (and the Consent Collider)
**One-line:** Fellows learn that slide dwell time, reaction scores, and physician consent are
post-treatment / selection colliders — conditioning on them biases the estimate.
**LOs:** (Analyze) identify a collider by structural position; (Evaluate) predict the bias from
conditioning on dwell/reaction; (Analyze) explain consent opt-out as a Berkson selection structure.
**Opening:** an analysis that "controls for engagement" and gets a confidently wrong message effect.
**Core:** post-treatment colliders; why conditioning opens a closed path; the consent architecture
(`CLM_EXPLICIT_OPT_IN_vod`, opt-out missingness) as collider/selection; FCI as the latent-aware tool.
**Research finding:** opt-out missingness is almost certainly correlated with physician characteristics
— a sampling-frame collider most analyses ignore. **Bridge:** even with colliders handled, the data
can't orient every edge → Ch 7.

### WEEK 7 — Markov Equivalence: Why the Data Can't Decide
**One-line:** Fellows learn why observational data identifies only a Markov-equivalence class, and why
rep structural knowledge is mathematically necessary to orient causal edges.
**LOs:** (Understand) Markov equivalence + Verma–Pearl; (Analyze) enumerate the equivalent stories for
a correlation (the peer-influence example); (Evaluate) state what evidence would orient the edge.
**Opening:** peer influence and prescribing are correlated — three causal stories fit equally well.
**Core:** Markov-equivalent structures; Verma–Pearl (data → class, not graph); why a rep's "I've
watched it go one way, never the other" orients the edge. **Research finding:** the rep interviews are
the *source* of structural information, not supplementary data. **Bridge:** so how do you extract it? → Ch 8.

### WEEK 8 — The Causal Interview Bot
**One-line:** Fellows learn to design an elicitation chatbot that lets a rep talk like a rep while the
system extracts a structured causal prior (KEBN).
**LOs:** (Create) write ladder-structured interview prompts (Rung 1/2/3) in rep-natural language;
(Apply) extract candidate confounders, susceptibility indices, and mediator weights from a transcript;
(Evaluate) turn responses into an annotated prior DAG with confidence + contradictions.
**Opening:** "Walk me through your last visit with Dr. Martinez" — and what a causal scientist hears in
the answer.
**Core:** the three-rung interview; NLP extraction; the knowledge bottleneck (Feigenbaum: CPT
explosion, linguistic ambiguity, recognition-explanation gap); output = structured prior DAG.
**Assessment:** run an interview (role-play or transcript) → produce the elicited prior DAG.
**Bridge:** priors + data → a working model → Ch 9.

### WEEK 9 — From Priors to a Living Model
**One-line:** Fellows learn to combine the elicited prior, the identified effects, and the data into a
parameterized SCM with a counterfactual engine and drift monitoring.
**LOs:** (Create) parameterize the SCM (KEBN: edges, CPTs, provenance/confidence); (Apply) run an
individual counterfactual (abduction→action→prediction): "which message for THIS physician next week?";
(Evaluate) specify drift monitoring (esp. opt-out-rate shifts).
**Opening:** the difference between the population IV estimate and the decision the brand actually needs
(Rung 2 → Rung 3).
**Core:** parameterized SCM; the counterfactual procedure; structural-change/drift detection; tying it
to the Living Model architecture. **Research finding:** the Rung-3 individual recommendation is the
operational payoff no NBA engine produces. **Bridge:** now run it on real projects → Act III.

## ACT THREE — RUNNING IT (Weeks 10–13)

### WEEK 10 — Project 1: Staggered DiD on Message-Variant Transitions
**[PROJECT RUN]**
**One-line:** Fellows run the most feasible, highest-value study: the treatment-on-treated effect of a
message variant on NRx, using the cohort rollout.
**LOs:** (Create) implement the staggered-DiD/IV design on a (synthetic or partner) panel; (Analyze)
run the falsification test first; (Evaluate) report the effect with assumptions stated.
**Opening:** the falsification test that must pass before any effect is believed.
**Core:** `Call2_Key_Message_vod__c` + cohort timing + NPI NRx + territory FE; staggered DiD
(TWFE-bias trap; Callaway–Sant'Anna); the ToT readout. **Research finding:** this is publishable and
directly productizable; the cohort-rollout instrument is the field's overlooked natural experiment.
**Bridge:** two more projects extend the toolkit → Ch 11.

### WEEK 11 — Projects 2 & 3: Meals (Causal Forest) and Slide Sequencing (DML)
**One-line:** Fellows estimate heterogeneous meal effects (separating reciprocity from education) and
the causal effect of slide sequencing — without conditioning on colliders.
**LOs:** (Create) causal-forest heterogeneous effects of Open-Payments meals by specialty/baseline;
(Apply) Double ML on `Call_Clickstream_vod` sequence with the SCM specified first; (Evaluate) explain
why naive conditioning on reaction/dwell corrupts the DML estimate.
**Opening:** a slide-sequencing "optimization" that conditions on engagement and blocks its own causal
path. **Core:** causal forest + meal pathway separation (meals = the public-data-runnable project); DML
on high-dim clickstream treatment; the collider discipline from Ch 6 in action. **Research finding:**
Project 2 is fully public-data-runnable (Open Payments); Projects 1 & 3 need proprietary or synthetic
data — the firewall in practice. **Bridge:** with effects estimated, who do you actually target? → Ch 12.

### WEEK 12 — From Propensity to Persuadables
**One-line:** Fellows learn to rank physicians by causal responsiveness, not baseline volume, and to
allocate detailing as constrained portfolio optimization.
**LOs:** (Analyze) distinguish sure-things (waste) from persuadables; (Evaluate) critique a propensity-
ranked NBA target list; (Create) produce a causal-responsiveness ranking + allocation.
**Opening:** the high-propensity prescriber who would have prescribed anyway — budget burned, not
targeted. **Core:** the sure-things problem; persuadables (often mid-tier, specific concern, pre-peer-
influence market); treatment-oriented allocation; constrained portfolio optimization. **Research
finding:** the targeting list a Living Model produces differs systematically from the NBA list — a
testable, productizable claim. **Bridge:** before any of this ships, the consent and IP constraints
must be settled → Ch 13.

### WEEK 13 — Consent, Compliance, and the Handoff
**[FINAL — LIVING-MODEL PROTOTYPE + HANDOFF BRIEF]**
**One-line:** Fellows learn to handle the consent/selection ethics, the regulatory line, and the
public-data/IP firewall, and to hand the partner a defensible model.
**LOs:** (Evaluate) put the consent mechanism in the causal graph + choose FCI; (Evaluate) apply the
regulatory line (educational pathway only) and a patient-welfare check; (Create) write the
product-handoff brief (test/build/kill) under the IP firewall.
**Opening:** a model that works on the consenting subsample and silently generalizes wrong.
**Core:** consent as collider (revisited); FCI; the educational-vs-reciprocity regulatory boundary;
the public-data/IP firewall (public/synthetic in the book; partner replicates on proprietary data);
the four-stage research→product pipeline; the Fellows→Doceree handoff. **Final deliverable:** a
Living-Model targeting prototype (on synthetic/public data) + a product-handoff brief. Closes the arc.

---

# PART 9 — CHAPTER ANATOMY TEMPLATE
Every chapter (series-consistent):
1. Overview (what you'll be able to do) · 2. Learning objectives (Bloom's) · 3. Opening case
(failure-first) · 4. Core sections (concept → example → application; define terms at first use) ·
5. Worked example on this dataset (process incl. dead ends → lesson → limit) · 6. Common misconceptions ·
7. **Evidence check** (independent vs vendor vs hypothetical vs contested) + **Consent/compliance &
patient-welfare check** · 8. **Named Fellow artifact** (panel build → ladder classification → IV design
→ pathway graph → collider audit → equivalence map → elicited prior DAG → parameterized SCM → project
run → persuadables ranking → handoff brief) · 9. Research finding for Fellows · 10. Exercises (≥3; ≥1
Apply+; ≥1 produce) · 11. **Five-part AI block** (When to Use AI / When NOT / LLM exercise / CLI
exercise / AI Validation) · 12. AI Use Disclosure · 13. **What would change my mind** · 14. **Still
puzzling** · 15. Summary · 16. Key terms · 17. Bridge · 18. Further reading (sourced)
**Enforcement:** a draft missing items 5, 7, 11, 13, or 14 is incomplete.

---

# PART 10 — CASE STUDY STRATEGY
- **One continuing dataset** (rep-visit telemetry) is the spine; every chapter advances the *same*
  worked case — assemble → identify → model → run → target → hand off.
- **Synthetic-first:** because the real data is proprietary, the book ships a **synthetic rep-visit
  dataset** with known ground-truth structure so Fellows can verify their methods recover it; the meals
  project uses real public Open Payments data.
- Escalation: Act I single-concept reads; Act II judgment (which edges, which colliders); Act III full
  synthesis (no single right answer; the partner's real call).
- **Sourcing requirement:** every empirical/method claim is cited or `[verify]`-flagged; Veeva field
  names and the IV/DiD/DML/FCI methods cited to primary sources.

---

# PART 11 — HARD TOPICS, CONTESTED CLAIMS, AGING RISK
| Claim | Status | Position |
|---|---|---|
| Cohort timing is a valid instrument | Conditional | Only if independence holds — falsify first; present as testable, not assumed |
| Dwell/reaction are useful model features | Common but wrong | Post-treatment colliders; teach the structural error |
| Rep knowledge can orient causal edges | Defensible (Verma–Pearl) | Necessary in principle; quality of elicitation is the open empirical question |
| Meals cause prescribing | Association-strong, causal-contested | Separate reciprocity vs education; cite primary causal work, `[verify]` |
| Consent missingness is ignorable | False | Berkson/selection collider; model it (FCI) |

**Hard chapters:** Ch 6 (colliders — inverts intuition), Ch 7 (Markov equivalence — abstract), Ch 8
(elicitation — needs a real/role-play rep). **Aging risk:** Veeva field names + CLM specifics (verify
each offering); causal methods + the structural argument are stable.

---

# PART 12 — MARKET POSITIONING
Primary "market": the Humanitarians AI fellowship + Doceree partner pipeline (the book is the operating
manual for that Fellows project). Secondary: graduate applied-causal-inference / health-analytics
seminars; professional cohorts. Clean differentiation (Part 4): the only book that turns *this* dataset
into a Living-Model causal targeting system, with rep-knowledge elicitation and the consent/IP firewall
load-bearing.

---

# PART 13 — FEATURE LIST
| Feature | Priority | Effort |
|---|---|---|
| Single-dataset 13-chapter arc | ESSENTIAL | Low |
| **Synthetic rep-visit dataset (known ground truth)** | ESSENTIAL | High |
| The three project runbooks (DiD / causal forest / DML) | ESSENTIAL | Medium |
| Causal Interview Bot spec + prompt set | ESSENTIAL | Medium |
| Living-Model prototype + handoff brief (terminal) | ESSENTIAL | Medium |
| Public-data/IP firewall + `private/` discipline | ESSENTIAL | Low |
| Five-part AI blocks + AI Use Disclosure | ESSENTIAL | Medium |
| Accuracy/[verify] pass | ESSENTIAL | Medium |
| DAG animation (cohort→message→physician→Rx) | IMPORTANT | Medium |
| Instructor/fellowship-lead manual | IMPORTANT* | High |

*Essential for any lead who is not the author.

---

# PART 14 — OUT OF SCOPE
| Topic | Why |
|---|---|
| General causal-inference theory | Taught only as this dataset needs; companions cover the rest |
| Veeva administration / engineering | The book models; the partner operates the platform |
| DTC / patient-facing marketing | HCP-facing dataset is the subject |
| Building NBA platforms in production | The lab tests/prototypes; the partner builds |
| Legal/regulatory authority | Flagged + routed to counsel, not adjudicated |
| Partner's proprietary data/methods | Out of scope by design (the firewall) |

---

# PART 15 — ADOPTION RISK REGISTER
| # | Risk | Likelihood | Impact | Status |
|---|---|---|---|---|
| **1** | **Proprietary-data access + partner framing + consent ethics unresolved** | **High** | **High** | **BLOCKER — settle with Graham/Doceree before Act III; gates the synthetic-data plan too** |
| 2 | Synthetic dataset not realistic enough to teach the methods | Med | High | Build ground-truth synthetic; validate recovery; meals project uses real public data |
| 3 | Reader skill spread (ML-yes, causal-no) | Med | Med | Teach each method at first use; companion books as prereq |
| 4 | Identification assumptions oversold (IV independence) | Med | High | Falsification-first discipline; assumptions stated every chapter |
| 5 | Consent/selection ethics underplayed | Med | High | Consent collider + welfare check made pervasive (Ch 6, 13) |
| 6 | Field/Veeva-schema velocity | High | Med | Stable principle + dated field example; annual review |
| 7 | Critical-of-pharma tone vs named partner | Med | High | Pro-evidence framing; partner sign-off on Ch 5/12/13 |

---

# PART 16 — OPEN QUESTIONS
| # | Question | Stakes | Deadline | Owner |
|---|---|---|---|---|
| 1 | Partner agreement: data access, IP firewall terms, framing sign-off | All of Act III | Before Act III | Nik + Graham |
| 2 | Specs for the synthetic rep-visit dataset (which ground-truth structure?) | Every lab/project | Before Ch 2 drafting | Author + Fellows |
| 3 | Companion-prereq vs self-contained causal teaching depth | Reader fit, length | Pre-draft | Author |
| 4 | Causal Interview Bot: real reps vs role-play for the elicitation chapter | Ch 8 credibility | Before Ch 8 | Nik + partner |
| 5 | Is patient-welfare a per-chapter check or a capstone? | Ethics posture | Pre-draft | Author (recommend: per-chapter) |

---

*Full TOC Draft v1.0 — grounded in `pantry/rep-visit-as-causal-dataset_source.md` and the Living Model
framework (`pantry/living-models.md`). Blocker before Act III: Risk 1 (proprietary-data access + partner
framing + consent ethics).*
