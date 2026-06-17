# Chapter 1 — The Experiment Nobody Knows They're Running
*Fifteen years of behavioral data, one unanswered question, and the distinction that makes all the difference.*

Start with a failure, because the failure is the reason this book exists.

A data team at a mid-size pharmaceutical company gets handed a few years of detailing telemetry — the per-slide records the field reps' iPads have been writing during sales calls. The data is gorgeous. For every visit, there is a row per slide: how many seconds the physician lingered, which slides got skipped, which got revisited, a little colored button the rep tapped to mark whether the doctor's reaction looked positive, neutral, or negative. Hundreds of thousands of these traces, timestamped, keyed to physician and territory and rep.

So the team builds a dashboard. Heatmaps of slide dwell time. Drop-off curves showing where attention dies. Reaction-score leaderboards by message. It is genuinely beautiful, and it gets shown to leadership, and someone says the words that doom the project: *"Now we understand what works."*

They do not understand what works. They understand what *happened*.

The dashboard can tell you that physicians lingered on slide 7 and that lingering correlates with later prescribing. It cannot tell you whether showing slide 7 *caused* the prescribing, or whether the physicians who were already going to prescribe were also the ones who lingered. Those are different questions. And the richer the dashboard got, the more confident everyone became — and the more confidently wrong. A million high-resolution observations of a confounded process give you a precisely estimated biased number. The precision feels like rigor. It is not rigor.

That gap — between a *cool* dataset and a *causal* dataset — is the gap this book is built to close. This chapter names it. The rest of the book closes it.

---

## What the iPad actually logs

When a pharmaceutical sales representative sits down with a physician, the centerpiece of the visit is a tablet running a digital sales-aid presentation. In industry terms this is **closed-loop marketing**, or CLM: an interactive deck of approved slides, served by the company's customer-relationship-management system. The CRM is the software category that hosts the data; what it actually contains is something quite different from a contact log.

Here is the move that matters. While the rep flips through slides and talks, the software is silently writing a behavioral record. Per slide, per visit, it captures how many seconds were spent on each slide; the order slides were shown, including skips, loops, and back-tracks; a real-time reaction the rep taps for each slide — positive, neutral, or negative; and the channel of the interaction, whether face-to-face or remote video. All of it gets packaged into a clickstream record at the end of the session, timestamped and keyed to the rep, the physician's National Provider Identifier, the territory, and the rep's training cohort.

A naming note worth carrying forward: specific field names in Veeva's CRM — the dominant platform, used by an estimated 80-plus percent of global pharmaceutical sales representatives by 2022, up from roughly a third in 2013 (industry-analyst estimates; Veeva serves 19 of the top 20 life-sciences companies) — are in transition as the company migrates from a Salesforce-based architecture to its proprietary Vault CRM. Every field name in this book is an example of a *kind* of field, not a guarantee of a current API name. Chapter 2 handles the schema in detail. Here, the names are incidental; the structure is the point.

The crucial property of this logging is that the rep does nothing to trigger it. It is **passive telemetry** — a byproduct of normal selling. The rep is trying to make a sale, not produce a dataset. That is exactly why the data is both rich and unexamined. It accrues as exhaust.

Now the reframe. Calling this a CRM makes you treat it like a record-keeping log: contact stamps, call notes, an activity history. But a CRM schema is merely the container. What actually accrues inside it is slide-level behavioral telemetry across hundreds of thousands of interactions a year — structurally far closer to a website clickstream or a session-analytics dataset than to a contact log. The CRM label is the reason nobody treats it as analyzable behavior. The label hides the dataset.

One visit produces one micro-behavioral trace: slide 3 viewed 47 seconds, slide 4 skipped, slide 3 revisited, "neutral" tapped on the safety slide. Stack that trace across one physician over months and you have a longitudinal behavioral series. Stack it across all physicians and all reps and you have something close to a census of how pharmaceutical promotion actually lands on the people it is aimed at.

![Single rep-visit trace as a horizontal timeline — each slide a block sized by dwell time and colored by reaction, with a skip and a back-track marked — then the same physician's trace stacked across six months as one longitudinal series.](images/01-the-experiment-nobody-knows-fig-01.png)
*Figure 1.1 — One visit, one trace; six months, a series*

<!-- → [DIAGRAM: Single rep-visit trace as a horizontal timeline — each slide as a block, width proportional to dwell time, color-coded by reaction, with skips and back-tracks shown as arrows. Below it, the same physician's trace stacked across six months. Caption: "One visit: one behavioral trace. Six months: a longitudinal series. The CRM logs both; nobody has treated either as data."] -->

---

## Behavioral signal is not a causal claim

This is the spine distinction of the book, and we plant it carefully.

The data is behaviorally dense: it records, in fine grain, what happened. But a behavioral signal is not a causal claim. Consider two sentences about the same dataset.

*"Physicians who dwelled longer on the safety slide prescribed more."* This is an **association** — a statement about what co-occurs. It is true or false as a matter of counting, and the telemetry can settle it in an afternoon.

*"Delivering the safety-first message caused more prescribing."* This is a **causal claim** — a statement about what would change if you intervened. The raw telemetry alone cannot license it, no matter how much of it you have.

The trap is believing that granularity buys you the second sentence. It does not. Granularity buys **resolution**, not **identification**. Identification is the question of whether your data and assumptions are even capable of pinning down the causal quantity you want. Resolution is how finely you can measure. You can have exquisite resolution on a hopelessly confounded comparison. The dashboard team had exactly that.

Here is the puzzle that makes the distinction concrete. A physician spends 47 seconds on the safety slide. What does that datum mean?

She might be reading carefully, genuinely absorbing the safety data — a positive educational signal. She might be hunting for the flaw, re-reading because she distrusts it — a negative signal, one the rep watching her frown in concentration might easily tap as "neutral." Or she got a text, her attention drifted, the slide stayed up — pure noise.

Identical behavioral datum. Three incompatible causal stories. The telemetry records *what happened* — 47 seconds — and is mute on *why*. No amount of additional 47-second observations resolves which story is true for this physician on this day. That silence is not a data-quality problem you can fix with more logging. It is structural. It is the seed of this book's central argument.

![Three branching interpretations of the single datum "47 seconds on the safety slide" — careful absorption read positive, skeptical scrutiny read negative, drifted attention read as noise — each an equally consistent causal story the data cannot choose among.](images/01-the-experiment-nobody-knows-fig-02.png)
*Figure 1.2 — One observation, three incompatible stories*

<!-- → [DIAGRAM: Three branching interpretations of the single datum "47 seconds on the safety slide" — compelled / skeptical / distracted — each leading to a different causal story with different implications for message strategy. Caption: "Identical observation, three incompatible interpretations. The data records what happened; the why requires something the database cannot hold."] -->

The misconception worth killing immediately: if there were a usable causal experiment in this data, someone would have found it. This is wrong, and it is wrong for a structural reason rather than a competence reason. The data lives in commercial and CRM silos, where it is analyzed constantly — for engagement reporting, for rep-performance management, for next-best-action recommendations. But all of that analysis operates on the first rung of the causal ladder: association. The people who have the data are organized around engagement metrics and lack the causal frame. The people who have the causal frame — econometricians, epidemiologists — have never had access to this data. That gap is not incompetence. It is an opportunity.

---

## The fifteen-year unrun experiment

Pharma has logged this telemetry at scale for roughly fifteen years — the iPad-detailing era (the first iPad shipped in 2010) plus the rise of the dominant CRM platform through the 2010s, when Veeva went from roughly a third of pharma reps in 2013 to 80-plus percent by 2022. The exact start date of *causal-grade* variation is not pinned; treat "fifteen years" as an approximate, defensible anchor rather than a precise figure. Across those years, the actual clinical messages reps deliver have changed. A brand revises its deck — from leading with efficacy data to leading with safety data, say. New decks roll out not all at once but in waves: by region, by geography, by the calendar on which reps are scheduled for training.

That rollout pattern generates something important: **quasi-experimental variation** — variation in which message reached which physician when, driven by administrative logistics rather than by anything about the physician. A physician did not choose to hear the new safety-first message. She heard it because her rep happened to be in the training cohort that got certified first. Another comparable physician across town kept hearing the old efficacy-first deck that same month, because his rep trained later.

That is the structure of a natural experiment. The variation in treatment is, plausibly, as-good-as-random with respect to the physicians. And nobody has run the analysis on it.

We assert this here as the motivating claim. The mechanism — how training-cohort timing functions as an **instrument**, and the three conditions it must satisfy — is Chapter 4's entire job. For now, notice three things. The variation exists. It was generated for reasons unrelated to the physicians. And it has been accumulating, unexamined, for over a decade.

The dominant pharma CRM standardizes the schema across most of the industry. Because one platform's object model is shared infrastructure across many firms, the telemetry is both vast and *comparable* — the content differs brand to brand; the shape is common. That uniformity is what makes a large-panel causal study conceivable rather than fantastical. A method that works on one brand's panel transfers, schema-wise, to another's. What does not transfer is ownership: the data is shared in shape and siloed in custody. That is the IP-firewall problem, and it is Chapter 13's. Here, we note only that the schema is the unlocking condition.

![A grid with rep training cohorts on the vertical axis and calendar quarters across the horizontal axis; each cell shaded for the old efficacy-first deck or the new safety-first deck, the staggered certification dates forming a checkerboard of message exposure driven by training logistics rather than physician choice.](images/01-the-experiment-nobody-knows-fig-03.png)
*Figure 1.3 — The unrun experiment: a rollout checkerboard*

<!-- → [DIAGRAM: Timeline from ~2010 to present — rep cohorts on the y-axis, message-deck rollout dates as vertical lines creating a checkerboard of "old message" and "new message" cells. Physicians assigned to cohorts by geography, not by choice. Caption: "The unrun experiment: administrative training schedules created quasi-random variation in which physician heard which message when. The telemetry recorded it all."] -->

---

## Turning a dashboard instinct into a do-question

Let me walk through the process a Fellow actually goes through on first contact with this data — including the dead end, because the dead end is the lesson.

**The starting instinct.** You get the telemetry. Your first move, almost reflexively, is to find what correlates with prescribing. You pull NPI-level prescribing volume, join it to slide-level engagement, and look for signal. You find some: physicians with higher average dwell on the efficacy slides prescribe more. Encouraging. You imagine the recommendation: *push the efficacy deck; it drives prescribing.*

**Why it's a dead end.** Stop and ask the behavioral-versus-causal question. Did the efficacy deck cause the prescribing, or did reps push the efficacy deck to physicians who were already leaning toward prescribing — because those are exactly the physicians who engage, who take the meeting, who lean in? You cannot tell from this comparison. The physicians who got more efficacy detailing are not exchangeable with those who got less; the rep chose where to spend effort, and that choice is tangled up with prescribing propensity. Your encouraging correlation is consistent with the deck doing everything, nothing, or anything in between. You have a precisely estimated number with no causal interpretation. This is the dashboard team's mistake wearing a regression's clothes.

**The fix.** The fix is not a better model on the same comparison. It is a *different comparison* — one where the message a physician received was determined by something unrelated to that physician's prescribing propensity. That "something" is the training-cohort rollout. The instinct to find correlations was not wrong as exploration; it was wrong as evidence. Exploration tells you what to look at. Identification tells you what you are allowed to conclude.

**The artifact: your do-question.** The output of this whole exercise is not a number. It is a question, written precisely enough to be answerable:

> *What is the effect on a physician's new prescriptions of delivering the safety-first message versus the efficacy-first message?*

In causal notation, this is written as comparing `P(NRx | do(message = safety-first))` against `P(NRx | do(message = efficacy-first))`. The notation `do(...)` — read it as "set the message by intervention, not by observation" — is formalized in Chapter 3. For now, treat it as a flag that says: *this is a causal question, not a correlation*.

You do not yet know how to estimate this. That is Chapters 4 through 11. But naming it precisely is the single most important move in the book, because it forces every later choice. The wrong prior question produces the wrong method, the wrong identification strategy, the wrong comparison group, the wrong result. You cannot take any of the later steps until you have taken this one.

**What naming the question does not buy.** It does not tell you the effect is identifiable in your data — that is a separate argument, Chapter 4's. It does not tell you the effect is the same for every physician — it almost certainly is not, and Chapter 11 addresses that. It does not tell you whether the effect runs through education, through reciprocity, or through relationship — distinct pathways with very different ethics, addressed in Chapter 5. Naming the do-question is the first step. But it is the step nobody takes.

---

## What would change my mind

The chapter's core claim is that rep-visit telemetry contains causal-grade variation that is, in deployment, unanalyzed at the level of the do-operator. Three things would require revision.

First, if a vendor published a method that explicitly estimates `P(NRx | do(message))` from this data with a stated identification strategy — not an engagement proxy — the claim that the question is unanalyzed would need to be retracted or narrowed. Chapter 3 looks for exactly this and does not find it; if that search is wrong, the chapter's framing changes substantially.

Second, if the training-cohort rollout turned out, on inspection, to be systematically targeted by physician characteristics — better reps assigned to better physicians first, for example — the natural-experiment claim collapses, and Chapter 4's falsification test is built to catch exactly that.

Third, if the telemetry turns out to be far sparser, noisier, or more consent-suppressed than described — enough that no usable panel can be assembled — the whole premise dissolves into a design exercise rather than a runnable study. Chapter 2 and Chapter 6 examine this honestly.

## Still puzzling

How much of the slide-level telemetry survives consent opt-out, and is the survivors' data even representative of the full physician population? A consent-mediated selection bias could be as serious as any confounding in the main analysis.

When precisely did causal-grade variation start accruing? "Fifteen years" is asserted, not pinned. The answer matters for statistical power and for how much pre-period data is available for falsification.

And the deepest one: even granting the variation exists, is the *why* — which of the three 47-second stories is true — ever recoverable from data alone? The book's working answer is no, and it takes until Chapter 7 to earn that answer properly. For now, the puzzle stays open.

---

## Exercises

**Warm-up**

1. *(Recall — tests the core distinction)* In your own words, explain why "we have millions of high-resolution slide-view records" is not, by itself, an argument that you can estimate a causal effect. Use the 47-second slide to illustrate — specifically, give two non-causal explanations for the same observation. *What this tests: whether you can state the behavioral-vs-causal distinction without jargon.*

2. *(Recall — tests the reframe)* What is the difference between a CRM schema and the behavioral data it contains? Why does the CRM label cause the dataset to go unexamined as behavior? *What this tests: whether you understand the labeling problem that hides the dataset.*

3. *(Recall — tests the natural experiment)* Explain, in plain language, why a rep-training rollout schedule might generate variation in physician message exposure that is "as-good-as-random." Then name one thing that would have to be true for that claim to hold, and one thing that would falsify it. *What this tests: whether you can describe the natural-experiment structure without asserting the identification claim is already established.*

**Application**

4. *(Apply — behavioral vs. causal classification)* Take five claims a rep-visit dashboard might display: "the safety slide has the highest dwell time"; "high-reaction visits are followed by more prescribing"; "Territory 4's reps get more positive reactions"; "physicians who saw the new deck in Q1 prescribed more in Q2"; "slide 3 has a 40% skip rate." For each, label it a behavioral signal or a causal claim, and for any labeled causal, name one alternative non-causal explanation for the same pattern. *What this tests: whether you can apply the distinction reliably to realistic examples.*

5. *(Apply — produces the named artifact)* Write your do-question in the form: "What is the effect on [outcome] of delivering [treatment] versus [comparison], for [population]?" Then write two additional lines: (1) the engagement metric a current tool would optimize instead of your outcome, and (2) one sentence on why that metric is not your outcome. *What this tests: whether you can name the causal question you will spend the book answering — badly at first, which is expected.*

6. *(Apply — dashboard critique)* Find one public description of a pharma next-best-action or engagement optimization tool — a vendor page, a case study, a blog post. Identify the target it says it optimizes. Is that target your outcome (prescribing caused by the message), or a proxy (engagement)? If it is a proxy, name the additional assumption that would have to hold for optimizing the proxy to be equivalent to optimizing your outcome. *What this tests: whether you can apply the behavioral-vs-causal lens to a real, public commercial claim.*

**Synthesis**

7. *(Synthesize — dataset and identification)* Connect the two arguments of this chapter: (1) the CRM contains dense behavioral telemetry, and (2) the training-rollout generates quasi-experimental variation. Explain why each argument, on its own, is insufficient to support a causal study — and what the two arguments together make possible that neither does alone. *What this tests: whether you understand that data richness and identification strategy are separate contributions that must both be present.*

8. *(Synthesize — the opening failure)* Return to the dashboard team in the opening case. They have a million observations and a genuinely beautiful visualization. Write the two-paragraph explanation you would give a non-technical colleague for why the dashboard answers the wrong question — and what question the right analysis would answer instead. Your explanation must not use the word "regression" or "causation." *What this tests: whether you can translate the book's central distinction into language a skeptical practitioner would accept.*

**Challenge**

9. *(Challenge — falsification design)* The chapter claims that training-cohort rollout timing is "as-good-as-random" with respect to physician prescribing propensity. Design a test that would falsify this claim: what data would you look at, what pattern would constitute a falsification, and how bad would the falsification have to be to make the natural-experiment argument untenable? You do not need the data to do this — specify the test precisely enough that someone with the data could run it. *What this tests: whether you can think adversarially about the book's central identification claim before you have been trained to defend it.*

---

## Prompts

### Figure 1.1 — One visit, one trace; six months, a series

Build a single self-contained HTML file (inline CSS; D3 7.9.0 from the cdnjs CDN) rendering a two-panel behavioral timeline of one physician's rep visits. Top panel: a single session as a left-to-right row of rectangular slide blocks, x position by sequence, block width proportional to dwell seconds (no zero-baseline; this is a duration-ribbon, not a bar chart), color encoding the rep-tapped reaction — red for positive (the one highlighted state), mid-gray neutral, dark ink negative, light gray skipped. Mark one skipped slide as a thin gap and one back-track. Bottom panel: small-multiple stack of five more months for the same physician, same encoding, thinner rows, sorted top-to-bottom by month. A within-call time arrow spans the bottom; a four-state legend sits beneath. Hover/focus any block for a tooltip with slide label, dwell seconds, and reaction. Deliverable: 700x420 viewBox, Brutalist palette via CSS variables with a dark-mode media query, EB Garamond title / Inter body / JetBrains Mono axis. Caption must note the reaction is a rep reading, not a measured cognitive response; data is synthetic/structural only.

### Figure 1.2 — One observation, three incompatible stories

Build a single self-contained HTML file (inline CSS; D3 7.9.0 from the cdnjs CDN) rendering a branching interpretation tree. One source node on the left ("47 seconds on the safety slide") fans via three equal-weight curved branches into three terminal nodes on the right: careful absorption (positive, red fill), skeptical scrutiny (negative, dark ink fill), drifted attention (noise, light gray fill). Branches must be equal stroke weight — no probabilities, no thicker "likely" path. Each terminal node is a labeled rectangle; hover/focus dims the other two and shows a tooltip with the full causal story. Deliverable: 700x420 viewBox, arrowheads from a single defs marker, Brutalist palette via CSS variables with dark-mode media query, EB Garamond title / Inter body. Annotate that the branch happens in interpretation, not in the recorded number, and that more telemetry cannot collapse the three to one. Synthetic/structural only.

### Figure 1.3 — The unrun experiment: a rollout checkerboard

Build a single self-contained HTML file (inline CSS; D3 7.9.0 from the cdnjs CDN) rendering a staggered-rollout matrix. Chart type: categorical heatmap / checkerboard. Y axis: four training cohorts (A–D) sorted by certification order, top = earliest. X axis: four calendar quarters left to right. Each cell is colored by which message deck the cohort delivered that quarter — light gray for the old efficacy-first deck, red for the new safety-first deck (post-certification). Staggering produces a lower-triangular checkerboard. A time arrow runs under the plot; a two-item legend sits below. Hover/focus any cell for a tooltip naming cohort, quarter, and deck. Deliverable: 700x420 viewBox, plot region filled F5F5F5 (never white), Brutalist palette via CSS variables with dark-mode media query, EB Garamond title / Inter body / JetBrains Mono tick labels. Caption must flag that cohort timing as an instrument is a testable claim (a cohort instrument is testable, not assumed) and that the as-good-as-random property is earned by a later falsification test, not asserted here; data synthetic/illustrative only.

---

## Chapter 1 Exercises: The Experiment Nobody Knows They're Running

**Project:** The Causal Interview Bot
**This chapter adds:** the elicitation spec — what the bot must pull out of a rep that telemetry can never see, anchored on the 47-second-slide ambiguity (the *why* behind a recorded behavior).

### Exercise 1 — When to Use AI

Three tasks in this chapter where reaching for an LLM is the right call.

First, **drafting the bot's opening interview stems.** You need a dozen plain-English questions a rep would actually answer — "walk me through the last time Dr. Okafor lingered on the safety slide" — phrased with zero causal jargon. *Why AI works here:* this is drafting in a constrained register, an option-generation task where the model proposes and you cull. **The tell:** you can read every stem aloud and judge whether a real rep would find it natural — you don't need the model to be right, only fluent enough to give you raw material.

Second, **reformatting a messy visit recollection into a clean transcript.** Hand the model a rep's stream-of-consciousness paragraph and ask it to segment by physician, visit, and slide. *Why AI works here:* it is mechanical structure-imposition on text you already have, and any error is visible against the source. **The tell:** the source text sits right next to the output and you can diff them.

Third, **generating the menu of non-causal stories for one observation.** Give the model "47 seconds on the safety slide" and ask for every reading — careful absorption, skeptical scrutiny, distraction — to seed your bot's disambiguation branches. *Why AI works here:* breadth-of-options is exactly what LLMs are good at, and you are the one who decides which readings are real. **The tell:** you can independently evaluate each story against what you know about how visits go.

### Exercise 2 — When NOT to Use AI

Two tasks where the model's confidence is a liability, not an asset.

First, **deciding whether the 47 seconds means absorption or skepticism for a specific physician on a specific day.** *Why AI fails here:* there is no ground truth in the data, and the model will manufacture a plausible single answer — exactly the false resolution the chapter warns against. The *why* is structural information the database does not contain; an LLM inventing it is hallucination dressed as inference. **The tell:** if you cite the model's interpretation as your *reason* for orienting a future causal edge, you have let it fabricate evidence; if you use it only to generate candidate questions that elicit the real answer from the rep, you have used it as a tool.

Second, **certifying that your do-question is the right causal question for the brand's decision.** *Why AI fails here:* naming the estimand is a values-and-stakes judgment — what outcome matters, to whom — that depends on context the model cannot access and should not arbitrate. **The tell:** the model can format your do-question; it cannot decide it deserves to be the one you spend the book answering. **Series connection:** this is a **T5 (Causal)** task — the irreducible human contribution is the structural claim the data cannot supply, which is the entire reason the interview bot exists rather than another classifier.

### Exercise 3 — LLM Exercise

**What you're building this chapter:** the bot's *elicitation spec* — a one-page document naming exactly what the Causal Interview Bot must extract that telemetry cannot, plus a first set of opening interview stems.

**Tool:** Claude, set up as a **Claude Project** named "Causal Interview Bot." Use a Project rather than a one-off chat because the bot's spec and system prompt will grow across all five chapters; the Project's persistent knowledge files become the bot's evolving memory, so Chapter 2 can add field-disambiguation questions on top of what you write here without re-explaining the dataset every time.

**The Prompt:**

```
You are helping me design "The Causal Interview Bot" — a chatbot that interviews
pharmaceutical sales reps to capture causal knowledge their iPad telemetry cannot.

Context the bot operates in: rep-visit telemetry logs per-slide dwell time, slide
navigation order, a rep-tapped reaction (positive/neutral/negative), channel, all
keyed to physician NPI and training cohort, plus downstream prescribing counts.
The telemetry records WHAT happened, never WHY. Anchor example: Dr. Alvarez, an
endocrinologist, spent 47 seconds on the safety slide. That single number is
consistent with at least three incompatible stories: she read it carefully
(positive education signal), she distrusted it and hunted for the flaw (negative),
or she got a text and the slide just stayed up (noise).

Produce two artifacts:

1. AN ELICITATION SPEC. List the specific kinds of knowledge the bot must extract
   from the rep that the telemetry cannot contain. For each, write one sentence on
   why the database structurally cannot hold it. Cover at minimum: the meaning
   behind dwell-time ambiguity, the reason a slide was skipped or revisited, and
   the rep's read on whether a reaction reflects the physician or the room.

2. SIX OPENING INTERVIEW STEMS in plain English a real rep would answer, using the
   Dr. Alvarez 47-second visit as the concrete scenario. NO causal jargon may
   appear in any stem — the rep must never hear "confounder," "mediator," or
   "counterfactual." After each stem, in a separate bracketed note to ME (not the
   rep), name what causal information that stem is trying to surface.

Do not assert which of the three 47-second stories is true. Your job is to design
questions that get the rep to tell me, not to guess the answer yourself.
```

**What this produces:** an elicitation spec listing the telemetry's blind spots and six rep-natural opening stems, each annotated with its hidden causal target — the seed document for the bot's system prompt.

**How to adapt:** *For your dataset:* replace Dr. Alvarez and the 47-second safety slide with a real ambiguous datum from your own panel (a long dwell, an odd skip), keeping the "three incompatible stories" frame. *For ChatGPT/Gemini:* paste the same prompt into a single chat, but save the output to a file you re-paste at the start of each later chapter's session, since those tools lack persistent Project memory. *For a Claude Project:* put the first paragraph (the bot's role and dataset context) in the Project's **system prompt / custom instructions**, and send the two-artifact request as a **message** — that way the role persists and only the per-chapter ask changes.

**Connection to previous chapters:** this is Chapter 1, so it is the foundation — every later block builds on this spec.

**Preview of next chapter:** Chapter 2 takes these blind spots and maps them onto the actual Veeva data fields, so the bot's questions can disambiguate the specific collider-prone signals (`Duration_vod`, `Reaction_vod`) rather than the abstract idea of "ambiguity."

### Exercise 4 — CLI Exercise

**What you're building:** the bot's repository scaffold — the files that will hold the elicitation spec, the growing interview stems, and the firewall rules — created with a tool that writes to disk.

**Tool:** Claude Code — because the artifact here is a *file structure* on your machine, not chat text, and the firewall discipline (synthetic/public only) is best enforced as an actual `.gitignore` and `CLAUDE.md` rather than as a good intention. **Skill level:** Beginner.

**Setup:**
1. Prereq artifact: the elicitation spec and six stems from Exercise 3 (paste-ready).
2. Tool: Claude Code (or Cowork) open in an empty directory.
3. CLAUDE.md rule to establish: "All interview content and data in this repo are synthetic or public only; never write real physician identifiers, real rep names, or proprietary telemetry."

**The Task:**

```
Scaffold a project for "The Causal Interview Bot." Create exactly these files and
nothing else:

- spec/elicitation-spec.md  — paste in the content I provide after you confirm the
  structure; until then, leave a single heading "# Elicitation Spec (Chapter 1)".
- spec/interview-stems.md   — a heading "# Interview Stems" plus an empty
  "## Chapter 1 — Opening (Rung-agnostic)" subsection for me to fill.
- bot/system-prompt.md      — a stub heading "# Causal Interview Bot — System Prompt".
- .gitignore               — ignore a private/ directory and any *.real.* files.
- CLAUDE.md                — write the synthetic-only rule I will give you, verbatim.

Read nothing else on my system. Do not invent spec content — only create the
file skeletons above. Stop after the files exist and print a tree of what you made.

Verification step: after creating the files, run `ls -R` (or the tree) and confirm
.gitignore contains a private/ entry and CLAUDE.md contains the word "synthetic".
```

**Expected output:** five files in a clean tree, with `.gitignore` and `CLAUDE.md` populated and the three Markdown stubs empty but correctly headed.

**What to inspect:** open `.gitignore` and `CLAUDE.md` yourself — confirm the `private/` ignore and the synthetic-only rule are actually there, not just claimed. Confirm the agent did not fabricate spec content you never gave it.

**If it goes wrong:** the common failure is the agent helpfully filling `elicitation-spec.md` with invented content. Recovery: delete the file, re-run with the explicit "do not invent spec content — only skeletons" line, and paste your real Exercise 3 output in a second step.

**CLAUDE.md note:** keep the synthetic-only rule at the top of `CLAUDE.md` for every later chapter — it is the firewall that lets you use AI freely on this project without ever touching proprietary data.

### Exercise 5 — AI Validation Exercise

**What you're validating:** the six opening interview stems from Exercise 3 — specifically, whether they elicit the rep's knowledge or smuggle in your own answer.

**Validation type:** Reasoning · **Risk level:** High — because a leading stem at the very start of the interview contaminates every downstream causal claim the bot builds, and the failure is invisible if you only read the stems for fluency.

**Setup:** use your Exercise 3 stems. If they all read clean, deliberately inject one flawed stem to test your own checklist — e.g., "So the safety slide is what convinced Dr. Alvarez to prescribe, right?" — and confirm your checklist catches it.

**The Validation Task:**

```
Validation Checklist — Chapter 1 (Opening Interview Stems)

For each interview stem, mark Pass / Fail / Cannot-determine on:

1. Correctness — does the stem ask about something the telemetry genuinely cannot
   capture (the WHY), rather than re-asking a number already in the data?
2. Completeness — across the six stems, are all three 47-second stories
   (absorption / skepticism / distraction) reachable, or do the stems only invite
   the flattering one?
3. Scope — does the stem stay in rep-natural language with NO causal jargon?
4. Chapter-specific: Open-endedness — does the stem leave the physician's reaction
   genuinely open, or does it presuppose a reaction?
5. Chapter-specific: Behavioral-vs-causal — does the stem ask the rep to REPORT
   what she observed, not to ASSERT a cause?
6. Failure-mode check — leading-the-witness: does any stem name the conclusion it
   hopes to hear ("the meal tipped her," "the safety data convinced her")? A stem
   can be fluent and grammatical and still lead the witness. Flag any stem whose
   answer is baked into the question.
```

**What to do with findings:** if all pass, lock the stems into `interview-stems.md`. One fail — rewrite that single stem open-ended and re-run the checklist. Multiple uncertain — the stem set is leaning; regenerate from Exercise 3 with an explicit instruction to never name a hoped-for answer.

**AI Use Disclosure prompt:** *Write two sentences naming exactly what an AI tool did in your Chapter 1 work and the one judgment it could not make. The judgment most specific to this chapter: whether a stem leads the witness — because the model that drafted the stems is the same kind of system that will cheerfully phrase a leading question, and only you can decide whether a real rep's answer would be free or steered.*

**Series connection:** the failure mode is **leading-the-witness** — the interviewer importing assumptions instead of eliciting them — which maps to **T5 (Causal)**: the human owns the structural claim, and the first place that ownership is won or lost is the wording of the very first question.
