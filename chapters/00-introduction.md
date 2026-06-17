<!--
    00-introduction.md
    INTRODUCTION — the roadmap chapter. Argument-first, reader-facing.
-->

# Introduction

The iPad is logging while she talks.

Dr. Alvarez, an endocrinologist, is forty seconds into the safety slide and the rep has not noticed the tablet is writing a record — `Duration_vod` ticking past forty-five seconds, `Display_Order_vod` noting that this slide was reached without a skip, a small reaction button waiting to be tapped. The rep is reading the doctor's face, not the screen. He taps "neutral," wraps the call, and drives to the next one. The session packages itself into a clickstream record keyed to the physician's NPI, the territory, the rep's training cohort. Multiply that by hundreds of thousands of visits a year, across fifteen years, across an industry that mostly runs on one CRM platform, and you have one of the largest behavioral datasets ever assembled about how professional persuasion lands on its target.

Now freeze on the forty-seven seconds Dr. Alvarez eventually spent on that slide. What does that number mean? She might have been reading carefully — a positive educational signal. She might have been hunting for the methodological flaw because she distrusts the data — a negative signal the rep cheerfully logged as "neutral." Or she got a text and the slide just stayed up — noise. Identical datum. Three incompatible causal stories. The telemetry records *what happened* and is mute on *why*. And the *why* is exactly what anyone allocating a promotional budget needs to know.

**The gap, and the claim.** Pharma has the data to answer a causal question and answers a correlational one instead; this book closes that gap on a single dataset. The central argument is testable: the rep-visit dataset already contains *both* the quasi-experimental variation needed to identify message effects — the staggered training-cohort rollout that decided which physician heard which message when — *and*, through the reps who lived the visits, the structural knowledge to orient the parts of the causal graph the data alone can never decide. Neither half is enough on its own. Together they make a causal study of pharmaceutical promotion buildable by one person.

**Who this is for.** You are a data-capable Fellow or practitioner. You can write Python, fit a model, read a regression table. You are *not* assumed to be a pharma insider, a Veeva administrator, or a senior causal-inference researcher. If you have never heard of closed-loop marketing or the do-operator, you are exactly the reader; the book teaches both. If you already publish in *Econometrica*, you will find the methods familiar and the domain new.

**What this book is.** It is an applied, single-dataset causal practicum. One dataset, worked end to end, from "what is this data" to "here is the governed handoff that ships a prototype." Along the way it teaches a vocabulary precisely: association versus intervention versus counterfactual; the do-operator; identification versus resolution; instruments and the exclusion restriction; colliders and selection bias; Markov equivalence; mediation; heterogeneous effects and persuadables. You learn these by using them on real public data and on a synthetic dataset whose answer you can check.

**What this book is not.** It is not a comprehensive causal-inference text — when you need the full treatment of a method, we cite the canonical source and move on. It is not a CRM manual; field names are illustrative, not authoritative, and the platform is mid-migration. It does not touch proprietary data and gives no legal advice. The prerequisites are modest but real: comfortable Python and basic statistics and regression. Everything causal is built from the ground up.

**The concept running underneath everything.** Pearl's ladder, and one sentence about it that does the most work in the book: *observational data identifies a class of graphs, not a single graph.* No matter how much telemetry you have, the data can settle a causal structure only partway — it leaves edges it can never orient. That is not a data-quality problem more logging fixes; it is structural. Recognizing it is what separates a Rung-1 dashboard from a Rung-2 study, and it is why the reps' knowledge is not a nice-to-have but a mathematical necessity.

**The running thread.** Every chapter's exercises build one artifact: **The Causal Interview Bot** — a chatbot that talks to a rep in plain language ("walk me through the last time Dr. Okafor lingered on the safety slide") while the system listens like a causal scientist, extracting candidate confounders, susceptibility knowledge, and mediator weights, and refusing to record any edge the rep did not volunteer. By the final chapter you have built the bot, its annotated prior graph, and the governance brief that ships it. The bot is the book's argument made operational: the irreducibly human contribution is the structural knowledge the data cannot supply, and the bot exists to get it out before the rep changes territory and it walks out the door.

**How the book is organized.** Thirteen chapters in three acts.

*Act I — The Untapped Dataset (Chapters 1–4).*
- **Chapter 1, The Experiment Nobody Knows They're Running**, names the gap between a cool dataset and a causal one and turns a dashboard instinct into a do-question.
- **Chapter 2, What the Data Actually Is**, walks the schema — the passive telemetry, the rep-entered notes, the warehouse layer — and which fields are confounders, treatments, outcomes, and traps.
- **Chapter 3, Stuck on Rung 1**, teaches Pearl's ladder and shows why every deployed pharma analytic — content optimization, rep scorecards, next-best-action engines — answers a question one rung below the one the budget is asking.
- **Chapter 4, The Natural Experiment in Every Training Rollout**, develops the training-cohort instrument and its three conditions, falsification-first.

*Act II — Building the Causal Model (Chapters 5–9).*
- **Chapter 5, The Causal Graph and the Three Pathways**, draws the message-to-prescribing graph and separates the educational, relationship-maintenance, and reciprocity pathways with their different ethics.
- **Chapter 6, The Collider Trap (and the Consent Collider)**, shows why dwell time and reaction scores are post-treatment colliders you must not condition on — and previews consent as a selection collider.
- **Chapter 7, Markov Equivalence: Why the Data Can't Decide**, proves the data identifies an equivalence class and leaves edges undirected, motivating the interviews mathematically.
- **Chapter 8, The Causal Interview Bot**, builds the KEBN elicitation engine — the three-rung ladder, the evidence gate, the rejected-edges appendix — that orients those edges from rep knowledge.
- **Chapter 9, From Priors to a Living Model**, parameterizes the elicited graph into a structural causal model with provenance tags and sensitivity dimensions.

*Act III — Running It (Chapters 10–13).*
- **Chapter 10, Project 1: Staggered Difference-in-Differences on Message-Variant Transitions**, runs the most feasible study end to end with the modern DiD estimators.
- **Chapter 11, Projects 2 & 3: Meals (Causal Forest) and Slide Sequencing (Double Machine Learning)**, estimates heterogeneous effects on public meals data and the effect of slide ordering, colliders handled.
- **Chapter 12, From Propensity to Persuadables**, reframes targeting from sure-things to physicians whose prescribing the message actually moves, with the uplift and Qini machinery.
- **Chapter 13, Consent, Compliance, and the Handoff**, treats consent as a selection collider, routes the regulatory landscape to counsel, and writes the handoff brief that ships the prototype across the public-data/IP firewall.

**How to read it.** Front to back is the intended path; the argument compounds. But the chapters are written to be self-contained enough that a reader who knows DiD can start at Chapter 10 and backfill. Each chapter closes with the same furniture: a *What would change my mind* section that names the evidence that would overturn its claims, a *Still puzzling* section that admits what remains open, and a five-part exercise block built around the running Interview Bot project — when to use AI, when not to, a hands-on LLM build, a command-line build, and an AI-validation audit. Work the exercises on the synthetic dataset first; it has a known ground truth, so you can tell whether your method actually recovered the answer before you ever touch real public data.

**A note about AI.** This is a book about using AI to do causal science, and it has a strong, specific opinion about *how*. In this field, the failure mode is not that the model is wrong — it is that the model is *fluent and confident* about exactly the things it cannot know. Ask a language model whether the forty-seven seconds meant absorption or skepticism and it will give you a crisp, plausible answer. That answer is a hallucination wearing the clothes of inference, because the *why* is structural information the data does not contain. So the discipline of this book is: **LLMs are elicitation clerks, never structural authorities.** A model may transcribe a rep interview, propose candidate edges *strictly tied to a verbatim rep quote*, surface contradictions across many transcripts, draft a prompt, format a brief, collate artifacts. A model may *not* decide which causal story is true, certify that your do-question is the right one, judge whether an elicited edge had genuine rep support, or adjudicate a regulatory question. The rule that operationalizes this is the evidence gate: every edge in the model needs a quote the rep volunteered unprompted, or it goes in the rejected-edges appendix. The tell, throughout, is *verify-not-trust* — if you can independently check the output against a source that sits right next to it, the AI is a tool; if the model's confidence is your *reason* for believing something, it has fabricated evidence.

That discipline is also why this book can exist as a fully public artifact. Bear Brown, LLC and Humanitarians AI work as an external innovation lab: we scope the question, prototype on public and synthetic data, test whether it holds, and then work alongside a partner's own team to implement what proves promising — staffed, when a project needs extra hands, from a Fellows pool of one to two hundred people on a per-hour basis. The firewall that makes this safe is the same one the AI discipline enforces: Fellows touch only public Open Payments data and a synthetic rep-visit dataset; the partner replicates anything promising on proprietary CRM data behind the wall. You will build the whole pipeline on this side of that firewall. Treating redaction as a reflex, and the handoff as governance rather than a demo, is not bureaucratic overhead — it is the thing that lets you use AI freely on a sensitive problem without ever touching what you are not permitted to touch.

## The research projects across the two books

These two books are a matched pair. They share one operating model: Humanitarians AI Fellows working with a pharmaceutical marketing-technology partner, under a single discipline. Fellows work only on **public data** (CMS Open Payments, Medicare Part D, Medicaid drug-utilization data, ICER value reports) and on **synthetic stand-ins**; the partner reproduces anything promising on its own proprietary data, behind an **IP firewall**, so nothing confidential ever enters a public book or repository. Every project is graded on an **Evidence Ladder** from 0 to 6 — public data realistically reaches about Level 3, an honest offline test that beats a baseline, and the higher rungs need the partner's private data. Every project writes its **kill criterion before it sees the result**, so a disappointing finding is a result, not an embarrassment to be spun. The lab's product is not a literature review and not a relabeled vendor number; it is *reduced uncertainty about where the partner should spend real data and engineering.* One blocker sits over all of it — **Risk 1**: partner data access, how the collaboration is framed, and consent ethics — and until it is settled, the proprietary and adversarial work waits.

The pair divides the labor. **Book A — _AI-Driven Programmatic HCP Marketing_** is breadth: a thirteen-project portfolio across the whole marketing stack. **Book B — _Pharma Rep Visit Data as an Untapped Causal Dataset_** is depth: one dataset, one causal model, built end to end. You are holding **Book B**; the full map is below, so you can see where your book sits — and so a partner can see the two as one program.

### Book A — the thirteen-project portfolio (breadth: the marketing stack)

**Track A — Lift and attribution** asks whether the "script-lift" numbers that set budgets are real effects or artifacts.

- **A1 — Script-lift attribution audit.** Rebuild a vendor-style lift number from public data, then re-estimate it with an honest design, to show how much of the headline was just the targeting picking physicians who would have prescribed anyway.
- **A2 — EHR-lift design memo.** Write the experiment that *would* prove point-of-care advertising works, with a power analysis — and admit that on public data this is a design, not a result. Saying so plainly is the contribution.
- **A3 — Channel decomposition.** Split an observed change in prescribing across promotion channels, honest about what cannot be separated without knowing who was actually exposed.

**Track B — Brand and physician identity** asks whether promotion builds something durable or rents short-term attention.

- **B1 — Share-of-mind → share-of-market.** Test the industry's belief that mindshare today predicts market share later — never rigorously checked on linked data. An open question, not a fact.
- **B2 — Prescribing-habit persistence.** Measure how sticky prescribing is across drug classes, to find where brand loyalty is structurally real.
- **B3 — Brand equity vs. clinical value.** The uncomfortable test: does brand equity predict prescribing *after* you control for the drug's independent clinical value? An equity-not-value finding is a patient-welfare result.
- **B4 — Loss-of-exclusivity event study.** Use the date a generic enters as a natural shock to see how much branded share survives, and whether the survivors were the strong brands. The causal companion to B3.

**Track C — Model architecture and segmentation** checks the engineering claims behind the targeting.

- **C1 — Ensemble vs. routed model.** The worked example: a plain, well-tuned model against a fancy "Mixture-of-Experts" one on the same targeting task, judged on calibration, not just accuracy. In the companion book's run the plain model wins — and the "no" saves a build nobody needed.
- **C2 — Physician archetypes.** Ask whether physician "segments" are a real structure in the data or a round number chosen to fit a slide.
- **C3 — Vendor "MoE" audit.** A desk review of vendor "Mixture-of-Experts" claims against a short list of what genuine MoE actually requires.

**Track D — Privacy, fairness, and accountability** is the adversarial track, and the one that most needs partner sign-off.

- **D1 — Proxy-discrimination test.** Check whether targeting models quietly key on physician "susceptibility" or protected-group proxies instead of clinical need.
- **D2 — Co-pay-coupon generic suppression.** Measure how much co-pay coupons suppress cheaper generics and raise system cost — a well-precedented public-data design that tests a partner product line, so framing must be cleared first.
- **D3 — Accountability framework.** Write the rules a deployed targeting system should live under: what it monitors, what it discloses, what can be audited.

### Book B — one dataset, built into a causal model (depth: rep-visit telemetry — this book)

Three runnable studies, plus the machinery that ties them together — and the chapters of this book build them in order.

- **Project 1 — Staggered difference-in-differences (Chapter 10).** Reps are trained in waves, so the rollout calendar is a natural experiment; use it to measure what a *message* actually causes a physician to prescribe. The most feasible and highest-value study.
- **Project 2 — Meals causal forest (Chapter 11).** Use public Open Payments data to estimate how promotional meals affect prescribing for different kinds of physicians, separating genuine education from simple reciprocity. The one study you can run today, end to end, on public data alone.
- **Project 3 — Slide-sequence Double Machine Learning (Chapter 11).** Ask whether the *order* of slides changes prescribing — and learn the book's hardest lesson: the engagement signals you would naively control for are *caused* by the message, so conditioning on them quietly breaks the estimate.

And the architecture around the studies:

- **The Causal Interview Bot (Chapter 8, and the running project).** Let a rep talk like a rep while the system listens like a causal scientist, turning the rep's knowledge of *why* a physician behaves a certain way into a structured prior — necessary because the data alone can never decide the direction of certain arrows.
- **Persuadables, not sure-things (Chapter 12).** Flip targeting from "who is most likely to prescribe" (often people who would anyway) to "who would a message actually move."
- **Consent, compliance, and the handoff (Chapter 13).** Handle the ethics and the law — opt-out quietly biases the sample, only the educational pathway is legally drivable, regulatory questions go to counsel — and end with the deliverable that matters: a working model on synthetic/public data plus a brief telling the partner exactly what to build behind the firewall.

### Where to start

For a first cohort that needs a fast, defensible win: in the companion book, **C1** (a plain model beating a fancy one) is the cleanest starter. In *this* book, **Project 2, the meals causal forest, is the only fully-public _causal_ result across both books** — start there. **A1** (the script-lift audit, companion book) strikes at the field's central inflated claim. And **D2** (coupon suppression) is well-precedented but should wait for Risk-1 sign-off. Everything above Level 3 — the strong causal versions, the deployed Living Model — is the partner's to build; the portfolio's highest-value output, where the real uncertainty lives above the public-data ceiling, is *telling the partner exactly what proprietary work to fund.*

**Back to the iPad.** The reaction button is waiting to be tapped, and the rep is about to guess. By the end of this book you will not be in the business of guessing what the forty-seven seconds meant. You will be in the business of building a model that knows the difference between what it can identify and what it must elicit — and that says so, out loud, in every claim it makes. Start with Chapter 1, and bring the do-question with you.

---

*Tags: causal inference, pharmaceutical marketing, rep-visit telemetry, Pearl's ladder, do-operator, instrumental variables, difference-in-differences, Markov equivalence, expert elicitation, KEBN, causal forest, double machine learning, persuadables, consent selection bias, FCI, Living Model, Irreducibly Human, AI+1, Humanitarians AI.*
