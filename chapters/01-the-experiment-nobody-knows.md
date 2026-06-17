# Chapter 1: The Experiment Nobody Knows They're Running

## Overview

By the end of this chapter you will be able to look at a pharmaceutical rep-visit dataset and see two things at once: a dense behavioral record of what happened in tens of thousands of physician interactions, and the raw material of a causal study that nobody has run. You will be able to describe what the detailing iPad actually logs and why that logging is "causal-grade." You will be able to tell a *behavioral signal* (what a physician did) apart from a *causal claim* (what a message caused) — the single distinction this whole book is built on. And you will write down, in plain language, the do-question your own project will answer, a sentence you will carry all the way to the final handoff brief.

This is the altitude chapter. We stay at 30,000 feet on purpose. The field-by-field schema is Chapter 2. The formal ladder that separates "what happened" from "what was caused" is Chapter 3. The mechanism that turns rep-training schedules into a usable instrument is Chapter 4. Here, the job is to make you *see* the dataset differently — to stop reading it as a customer-relationship-management log and start reading it as an experiment that has been running, unanalyzed, for roughly fifteen years.

## Learning objectives

After this chapter, you should be able to:

- **(Understand)** Describe what the detailing iPad logs during a rep visit and explain why that telemetry is causal-grade rather than merely descriptive.
- **(Analyze)** Distinguish a behavioral signal in this data from a causal claim about it, and identify when a "rich data" argument is quietly substituting for an identification argument.
- **(Apply / own-thread)** State, in one sentence, the do-question your project will answer — your first named artifact, carried forward to Chapter 4 and the final brief.

## Opening case: the dashboard that felt like understanding

Start with a failure, because the failure is the reason this book exists.

A data team at a mid-size pharmaceutical company gets handed a few years of detailing telemetry — the per-slide records the field reps' iPads have been writing during sales calls. The data is gorgeous. For every visit, there is a row per slide: how many seconds the physician lingered, which slides got skipped, which got revisited, a little colored button the rep tapped to mark whether the doctor's reaction looked positive, neutral, or negative. Hundreds of thousands of these traces, timestamped, keyed to physician and territory and rep.

So the team builds a dashboard. Heatmaps of slide dwell time. Drop-off curves showing where attention dies. Reaction-score leaderboards by message. It is genuinely beautiful, and it gets shown to leadership, and someone says the words that doom the project: *"Now we understand what works."*

They do not understand what works. They understand what *happened*. The dashboard can tell you that physicians lingered on slide 7 and that lingering correlates with later prescribing. It cannot tell you whether showing slide 7 *caused* the prescribing, or whether the physicians who were already going to prescribe were also the ones who lingered. Those are different questions, and the richer the dashboard got, the more confident everyone became — and the more confidently wrong. A million high-resolution observations of a confounded process give you a precisely estimated biased number. The precision feels like rigor. It is not rigor.

That gap — between a *cool* dataset and a *causal* dataset — is the gap this entire book is built to close. This chapter names it. The rest of the book closes it.

(The same failure, in a generic enterprise setting, opens the companion *Living Models* book as "The Dashboard That Lied." This chapter is its pharma-specific instance: a model — or a dashboard — that summarizes the data beautifully and cannot answer a single do-question.)

## Core sections

### What the iPad actually logs (and why "CRM" is the wrong frame)

When a pharmaceutical sales representative — a "rep" — sits down with a physician, the centerpiece of the visit is a tablet running a digital sales-aid presentation. In industry terms, this is **closed-loop marketing (CLM)**: an interactive deck of approved slides, served by the company's customer-relationship-management (CRM) system. *CRM* is the category of software that companies use to track interactions with customers; in pharma, the physician is the customer and the rep is logging the call.

Here is the move that matters. While the rep flips through slides and talks, the software is silently writing a behavioral record. Per slide, it captures (we tease the field names now; the full dictionary is Chapter 2):

- how many seconds were spent on each slide (`Duration_vod`),
- the order slides were shown in — including skips, loops, and back-tracks (`Display_Order_vod`),
- a real-time reaction the rep taps for each slide: positive, neutral, or negative (`Reaction_vod`),
- the channel of the interaction — face-to-face, remote video, and so on (`Call_Channel_vod`),

all of it packaged at the end of the session into a clickstream record (`Call_Clickstream_vod`), timestamped and keyed to the rep, the physician's **National Provider Identifier** (NPI — the unique ID every U.S. prescriber carries), the territory, and the rep's training cohort.

(A naming caveat we will repeat: these `*_vod` field names are dated mid-2026 examples. Veeva, the dominant vendor here, is migrating from its Salesforce-based "Veeva CRM" to a proprietary "Vault CRM," and field names may be renamed or relocated. Treat every field name in this book as an example of a *kind* of field, not a guarantee of a current API name. Chapter 2 handles this in detail.)

The crucial property: **the rep does nothing to trigger this logging.** It is passive telemetry, a byproduct of normal selling. The rep is trying to make a sale, not produce a dataset. That is exactly why the data is both rich and unexamined — it accrues as exhaust.

Now the reframe. Calling this "a CRM" makes you treat it like a record-keeping log: contact stamps, call notes, an activity history. But a CRM *schema* is merely the container. What actually accrues inside it is slide-level behavioral telemetry across hundreds of thousands of interactions a year — structurally far closer to a website clickstream or a session-analytics dataset than to a contact log. The CRM label is the reason nobody treats it as analyzable behavior. The label hides the dataset.

One visit produces one micro-behavioral trace: slide 3 viewed 47 seconds, slide 4 skipped, slide 3 revisited, "neutral" tapped on the safety slide. Stack that trace across one physician over months and you have a longitudinal behavioral series. Stack it across all physicians and all reps and you have something close to a census of how pharmaceutical promotion actually lands.

### Behavioral signal is not a causal claim

This is the spine distinction of the book, so we plant it carefully and we do not resolve it (Chapter 3 formalizes it with Pearl's ladder; Chapter 7 shows why the data alone *can't* resolve it).

The data is behaviorally dense: it records, in fine grain, what happened. But a behavioral signal is not a causal claim. Consider two sentences:

- *"Physicians who dwelled longer on the safety slide prescribed more."* This is an **association** — a statement about what co-occurs. It is true or false as a matter of counting, and the telemetry can settle it.
- *"Delivering the safety-first message caused more prescribing."* This is a **causal claim** — a statement about what would change if you intervened. The raw telemetry alone *cannot* license it, no matter how much of it you have.

The trap is believing that granularity buys you the second sentence. It does not. Granularity buys resolution, not identification. **Identification** is the question of whether your data and assumptions are even capable of pinning down the causal quantity you want; resolution is just how finely you can measure. You can have exquisite resolution on a hopelessly confounded comparison. The dashboard team had exactly that.

Here is the puzzle that makes the distinction concrete — keep it in your head; we will return to it in Chapter 3 and again in Chapter 7. A physician spends 47 seconds on the safety slide. What does that datum mean?

1. **Compelled.** She is reading carefully, genuinely absorbing the safety data. A positive educational signal.
2. **Skeptical.** She is hunting for the flaw, re-reading because she distrusts it. A negative signal — and one the rep, watching her frown in concentration, might easily tap as "neutral."
3. **Polite while distracted.** She got a text, her attention drifted, the slide stayed up. Pure noise.

Identical behavioral datum. Three incompatible causal stories. The telemetry records *what happened* — 47 seconds — and is mute on *why*. No amount of additional 47-second observations resolves which story is true for this physician on this day. That silence is not a data-quality problem you can fix with more logging. It is structural. It is the seed of this book's central argument, and we will not pretend to resolve it here.

### The fifteen-year unrun experiment

Now the framing that gives the book its title.

Pharma has logged this telemetry at scale for roughly fifteen years — the iPad-detailing era plus the rise of a dominant CRM platform, broadly the 2010s onward. (Treat "fifteen years" as approximate; the notes flag it as `[verify]` against a pinned start date rather than a round number, and so do we.) Across those years, the actual clinical messages reps deliver have *changed*. A brand revises its deck — say, from leading with efficacy data to leading with safety data. New decks roll out not all at once but in waves: by region, by geography, by the calendar on which reps are scheduled for training.

That rollout pattern generates **quasi-experimental variation** — variation in *which message reached which physician when* that was driven by administrative logistics rather than by anything about the physician. A physician did not choose to hear the new safety-first message; she heard it because her rep happened to be in the training cohort that got certified first. Another comparable physician, across town, kept hearing the old efficacy-first deck that same month, because *his* rep trained later.

That is the structure of a natural experiment. The variation in treatment is, plausibly, as-good-as-random with respect to the physicians. And nobody has run the analysis on it.

We assert this here as the motivating claim. The *mechanism* — how training-cohort timing functions as an **instrument**, and the three conditions it must satisfy — is Chapter 4's entire job. For now: notice that the variation exists, that it was generated for reasons unrelated to the physicians, and that it has been accumulating, unexamined, for over a decade.

Why has nobody run it? Not for lack of effort — and this is the credibility-protecting point. The data lives in commercial and CRM silos, where it is analyzed constantly: for engagement reporting, for rep-performance management, for next-best-action recommendations. But all of that analysis lives on the lowest rung of the causal ladder (Chapter 3). The people who *have* the data are organized around engagement metrics and lack the causal frame; the people who *have* the causal frame — econometricians, epidemiologists — have never had access to this data. That gap is not a scandal. It is an opportunity, and it is yours.

### Scale: why this dataset is large and uniform

One more reason this dataset is special. The dominant pharma field-sales CRM standardizes the schema across most of the industry. The source essay puts Veeva's share above 80% of pharma field sales — a figure we flag as `[verify]` (it needs a current market citation; soften to "the dominant pharma CRM" if you cannot source it). Whatever the exact number, the consequence holds: because one platform's object model (`Call_Clickstream_vod`, `Reaction_vod`, and the rest) is shared infrastructure across many firms, the telemetry is both vast *and comparable*. The content differs brand to brand; the *shape* is common.

That uniformity is what makes a large-panel — even a cross-firm — causal study conceivable rather than fantastical. A method that works on one brand's panel transfers, schema-wise, to another's. (Note carefully what this does *not* solve: the obstacle to cross-firm work is data *ownership*, not data *shape*. That is the IP-firewall problem, and it is Chapter 13's. The schema is shared; the data is owned.)

## Worked example: turning a dashboard instinct into a do-question

Let me walk through the process a Fellow actually goes through on first contact with this data — including the dead end, because the dead end is the lesson.

**The starting instinct (the dead end).** You get the telemetry. Your first move, almost reflexively, is to find what correlates with prescribing. You pull NPI-level prescribing volume, join it to slide-level engagement, and look for signal. You find some: physicians with higher average dwell on the efficacy slides prescribe more. Encouraging. You imagine the recommendation: *"Push the efficacy deck; it drives prescribing."*

**Why it's a dead end.** Stop and ask the behavioral-vs-causal question. Did the efficacy deck *cause* the prescribing, or did reps push the efficacy deck to physicians who were already leaning toward prescribing — because those are exactly the physicians who engage, who take the meeting, who lean in? You cannot tell from this comparison. The physicians who got more efficacy detailing are not exchangeable with those who got less; the rep chose where to spend effort, and that choice is tangled with prescribing propensity. Your encouraging correlation is consistent with the deck doing everything, nothing, or anything in between. You have a precisely estimated number with no causal interpretation. This is the dashboard team's mistake wearing a regression's clothes.

**The lesson.** The fix is not a better model on the same comparison. It is a *different comparison* — one where the message a physician received was determined by something unrelated to that physician's prescribing propensity. That "something" is the training-cohort rollout (Chapter 4). The instinct to find correlations was not wrong as exploration; it was wrong as *evidence*. Exploration tells you what to look at. Identification tells you what you are allowed to conclude.

**The artifact this produces: your do-question.** The output of this whole exercise is not a number. It is a *question*, written precisely enough to be answerable. Instead of "does engagement predict prescribing," you write a do-question:

> *What is the effect on a physician's new prescriptions of delivering the safety-first message, versus the efficacy-first message — `P(NRx | do(message = safety-first))` vs `P(NRx | do(message = efficacy-first))`?*

You do not yet know how to estimate this. (That is Chapters 4 through 11.) But naming it precisely is the single most important move in the book, because it forces every later choice. The notation `do(...)` — read it as "set the message by intervention, not by observation" — is formalized in Chapter 3; for now, treat it as a flag that says *this is a causal question, not a correlation*.

**The limit.** Be honest about what naming the question does and does not buy. It does not tell you the answer is identifiable in your data; that is a separate argument (Chapter 4's). It does not tell you the effect is the same for every physician; it almost certainly is not (Chapter 11). And it does not tell you whether the effect runs through education, through reciprocity, or through relationship — distinct pathways with very different ethics (Chapter 5). Naming the do-question is the first step, not the last. But you cannot take any of the later steps until you have taken it.

## Common misconceptions

**"It's a CRM, so it's a record-keeping log."** A CRM schema *hosts* the data; what accrues is slide-level behavioral telemetry on hundreds of thousands of interactions — a clickstream, not a contact list. The CRM framing is precisely why the dataset goes unexamined as behavior.

**"Rich, granular, timestamped data is causal data."** Granularity buys resolution, not identification. A million high-resolution observations of a confounded process yield a precisely estimated biased number.

**"If there were a usable experiment in this data, someone would have found it."** The data sits in silos analyzed for engagement and rep performance, not causal identification. The people with the data lack the frame; the people with the frame lack the data.

**"Nobody is using this data, so the field is incompetent."** Wrong and unfair. The field analyzes this data intensively — just on the wrong rung. The claim is about the *question being asked*, not the *effort being spent*. (We hold this line hard for both accuracy and partnership reasons.)

## Evidence check

**Independent vs. vendor vs. hypothetical vs. contested.**

- *Independent / settled:* That the CLM media player logs slide-level dwell, navigation, reaction, and channel is documented in Veeva's own CRM Help (independent of any claim in this book). That HCP-directed promotion is a large spend category is supported by public sources (CBO; MM+M trend reporting).
- *Vendor:* The "above 80% market share" figure originates from the source essay and is not yet pinned to an independent market report — carried as `[verify]`.
- *Hypothetical:* The opening dashboard team and the specific physician scenarios are illustrative composites, not reports of a named company's data.
- *Contested / positioning:* "Causally unanalyzed" is a *positioning* claim. Vendors run heavy analytics on this data. The defensible version is narrow: *no deployed engine answers the do-question* — it is about the rung, not the effort. Chapter 3 substantiates this against vendors' own self-descriptions.

**Consent / compliance & patient-welfare check.** Even at altitude, flag the thing that becomes load-bearing later: whether a physician's telemetry is logged at all, and whether it carries an NPI, is governed by *consent* settings (Chapter 2 maps them; Chapter 6 shows why they create a selection bias; Chapter 13 makes them a deployment constraint). Nothing in this chapter touches patients directly, but the downstream purpose — influencing what gets prescribed — is a patient-welfare matter, and the only regulatorily defensible pathway to optimize is the *educational* one (Chapter 5, Chapter 13). We name that now so it is never an afterthought.

## Named Fellow artifact: state your do-question

Your artifact for this chapter is one sentence. Write the do-question your project will answer, in this form:

> *What is the effect on `[outcome — e.g., new prescriptions, NRx]` of delivering `[treatment — e.g., message variant A]` versus `[comparison — e.g., message variant B]`, for `[population — e.g., a given specialty]`?*

Then write two more lines: (1) the engagement metric a current tool would optimize *instead* of your outcome, and (2) one sentence on why that metric is not your outcome. You will revise this by Chapter 4 once you understand identification. That is expected. Writing it badly now is how you write it well later. Keep it; it is the pebble dropped in the pond, and every later artifact is a ripple from it.

## Research finding for Fellows

The headline finding of this chapter is a finding about *open territory*: the entire class of pharma rep-visit telemetry is, as far as public evidence shows, causally unanalyzed at the level of the do-operator. Not unanalyzed — heavily analyzed, on Rung 1. But the interventional question, the one the budget actually turns on, has no deployed answer. For a Fellow, that is not a gap to lament; it is a frontier. The methods exist (econometrics, epidemiology, causal ML). The data exists (fifteen years of it). They have never been introduced. Introducing them is the research program.

## Exercises

1. **(Understand)** In your own words, explain why "we have millions of high-resolution slide-view records" is not, by itself, an argument that you can estimate a causal effect. Use the 47-second slide to illustrate.

2. **(Analyze)** Take five claims a rep-visit dashboard might display (e.g., "the safety slide has the highest dwell time," "high-reaction visits are followed by more prescribing," "Territory 4's reps get more positive reactions"). For each, label it *behavioral signal* or *causal claim*, and for any you labeled causal, name one alternative non-causal explanation for the same pattern.

3. **(Apply — produces an artifact)** Write your do-question (the named artifact above), plus the two follow-on lines. Then write a short paragraph: if a colleague handed you a model that predicts your outcome with 95% accuracy, would that answer your do-question? Why or why not?

4. **(Analyze, optional stretch)** Find one public description of a pharma next-best-action or engagement tool (vendor page, case study, blog). Identify the *target* it says it optimizes. Is that target your outcome, or a proxy? Save your note; you will reuse it in Chapter 3.

## Five-part AI block

**When to use AI here.** Use an LLM as a sparring partner for sharpening your do-question and for stress-testing whether a claim is behavioral or causal. It is good at generating alternative non-causal explanations for a correlation — exactly the muscle this chapter trains. Use it to brainstorm the *space* of explanations, then you adjudicate.

**When NOT to use AI here.** Do not let an LLM tell you that your dataset "supports causal inference" or assert an effect exists. It has no access to your identification strategy and will happily produce confident, ungrounded causal language — the precise failure mode this chapter warns against. Causal *licensing* is your job, not the model's.

**LLM exercise.** Paste this prompt into your LLM of choice:

```
I have pharmaceutical sales-rep visit telemetry: per-slide dwell time, slide
navigation order, a rep-tapped reaction (positive/neutral/negative), channel,
all keyed to physician NPI and training cohort, plus downstream prescribing
counts per physician.

Here is a claim I might make from it: "Physicians who spent more time on the
safety slide prescribed more, so the safety message drives prescribing."

Do NOT tell me whether this is true. Instead:
1. List every distinct NON-causal explanation for this correlation you can think of.
2. For each, say what about the data-generating process would have to be true.
3. Tell me what kind of variation in the data would let me distinguish the
   causal story from the non-causal ones — without asserting that my data has it.
```

**CLI exercise.** In a terminal with Claude Code (or similar), ask it to scaffold — *not* analyze — a project: create a `do-question.md` file containing your do-question template, a `pantry/`-style notes stub listing the four causal ingredients you will need (treatment, outcome, instrument, covariates), and a `.gitignore` with a `private/` entry (the firewall habit, started early). Review every line it writes.

**AI validation.** Check the LLM's output the way this chapter checks a dashboard: did it slip from listing explanations into *asserting* which one is right? Did any "non-causal explanation" actually smuggle in an unstated causal assumption? Cross-check at least one of its proposed distinguishing variations against what you actually know is in your data — if it proposes "randomized message assignment" and your data has none, that is the model telling you what you'd *need*, not what you *have*.

## AI Use Disclosure

This chapter was drafted by a human author working from research notes; an AI assistant was used to help organize prose and tighten phrasing, and every causal claim, field name, and source was checked against the notes and flagged `[verify]` where the notes flagged it.

## What would change my mind

The chapter's core claim is that rep-visit telemetry contains causal-grade variation that is, in deployment, unanalyzed at the do-operator level. I would revise this if: (1) a vendor published a method that explicitly estimates `P(NRx | do(message))` from this data with a stated identification strategy — not an engagement proxy (Chapter 3 looks for exactly this and does not find it); (2) the training-cohort rollout turned out, on inspection, to be systematically targeted by physician characteristics, destroying the natural-experiment claim (Chapter 4's falsification test is built to catch this); or (3) the telemetry turned out to be far sparser, noisier, or more consent-suppressed than described — enough that no usable panel can be assembled (Chapter 2 and Chapter 6 examine this).

## Still puzzling

A few questions this chapter raises and does not answer. How much of the slide-level telemetry survives consent opt-out, and is the survivors' data even representative? (Chapter 6, Chapter 13.) The "fifteen years" figure is asserted, not pinned — when, precisely, did causal-grade variation start accruing? (Open `[verify]`.) And the deepest one: even granting the variation exists, is the *why* — which of the three 47-second stories is true — ever recoverable from data alone, or does it always require something the database cannot hold? The book's answer is the latter, and it takes until Chapter 7 to earn it.

## Summary

Rep-visit telemetry is not a CRM log; it is a dense behavioral dataset — slide-level dwell, navigation, reaction, channel — logged passively across hundreds of thousands of physician interactions. That density makes it *cool*. It does not, by itself, make it *causal*: a behavioral signal is not a causal claim, and granularity buys resolution, not identification. What makes the data causal-grade is something else — the quasi-experimental variation generated when message decks roll out by training cohort, an experiment that has been running for roughly fifteen years and that nobody has analyzed at the level of the do-operator. The first move of the book, and your first artifact, is to stop hunting correlations and instead *name the do-question* your project will answer.

## Key terms

- **Closed-loop marketing (CLM):** the interactive, approved digital sales-aid presentation a rep runs on a tablet during a visit; the software that logs the telemetry.
- **CRM (customer relationship management):** the software category that hosts the rep-visit data; the schema that contains the telemetry, not the telemetry itself.
- **Passive telemetry:** behavioral data logged automatically as a byproduct of normal activity, without any deliberate data-entry act.
- **NPI (National Provider Identifier):** the unique identifier every U.S. prescriber carries; the join key linking telemetry to prescribing and payment data.
- **Behavioral signal vs. causal claim:** a statement about what co-occurred (association) vs. a statement about what an intervention would change (causation).
- **Identification:** whether your data plus assumptions can, in principle, pin down the causal quantity you want — distinct from resolution (how finely you can measure).
- **Natural experiment:** variation in treatment generated by something unrelated to the units, which can stand in for a deliberate randomized assignment.
- **do-question / do-operator:** a question about the effect of *setting* a variable by intervention, written `do(X = x)`; formalized in Chapter 3.

## Bridge

To use the dataset, you have to be precise about what it actually contains — field by field, plane by plane. Chapter 2 opens the `Call_Clickstream_vod` payload and reads it line by line, then assembles the physician-by-time panel and classifies every field as treatment, outcome, instrument, covariate, or collider.

## Further reading

- Pearl, J., & Mackenzie, D. (2018). *The Book of Why: The New Science of Cause and Effect.* (Chapter 1, "The Ladder of Causation" — the source of the behavioral-vs-causal distinction this chapter plants and Chapter 3 formalizes.)
- Veeva CRM Help, "Tracking CLM Call Clickstream Activity" and "Capturing Reactions to Presentations." (`crmhelp.veeva.com` — the primary documentation that the telemetry described here is real; note the ongoing Veeva CRM → Vault CRM migration.)
- Congressional Budget Office, "Promotional Spending for Prescription Drugs." (`cbo.gov/publication/25006` — public grounding for pharma promotional spend; HCP-directed promotion as the majority share.)
