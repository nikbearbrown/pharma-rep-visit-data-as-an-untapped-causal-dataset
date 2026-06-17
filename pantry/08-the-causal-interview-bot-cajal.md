# CAJAL Figure Report — Chapter 08: The Causal Interview Bot
_Mode: /scan silent · 2026-06-16 · source: chapters/08-the-causal-interview-bot.md_

## Density recommendation

For this text, I recommend **4 figures using Mechanistic density.** The chapter is procedural at its core: it builds an elicitation pipeline whose value depends on the reader holding several interdependent stages at once. Three MC concepts dominate — the knowledge-source-to-identification-role mapping (what telemetry, rep elicitation, and an RCT each contribute), the three-rung question ladder (association → intervention → counterfactual, each rung feeding the next), and the annotated-prior-DAG edge schema (evidence/confidence/contradictions, plus the rejected-edges integrity check). A fourth figure isolates the chapter's central discipline — the evidence gate that runs in one direction only (rep quote grounds the edge; model prior is rejected), which is a VG claim the prose makes repeatedly but never shows. The chapter places three author diagram comments and one table comment; CAJAL confirms all four and re-types the table content as a structured edge-schema figure. No PQ figures — the chapter's only numbers (47 seconds, "forty visits") are anecdotal anchors, not a distribution worth charting. The honesty caveat is load-bearing here: rep knowledge is necessary *in principle*, but elicitation *quality* (LLM suggestibility, hallucinated structure) is the open empirical question — Figure 4 must show the gate as a designed safeguard, not a solved problem.

## Figures

---

FIGURE 1 — What each knowledge source contributes to causal identification · Critical · Comparison panels · VG + MC

**BLOCK 1 — ILLUSTRAE PASTE**

Build a three-column contribution diagram. Each column is a vertical source box at top feeding a small graph fragment below it that shows what that source can and cannot resolve about a causal model. Column one (observational telemetry): box at top, below it a small graph whose edges are drawn as plain undirected line segments — the source identifies the structure but leaves edges unoriented. Column two (rep elicitation): box at top, below it the same edges now drawn as directed arrows with single arrowheads — the source supplies orientation. Column three (randomized experiment): box at top, below it two nodes joined by one bold directed arrow representing a total effect, with no internal pathway structure shown. Beneath the middle and outer columns, draw a horizontal brace spanning the gap between column one and column three to mark the region only elicitation fills. Keep the three source boxes uniform, the graph fragments small and aligned to a shared baseline, flat vector style, single stroke weight, white background. Leave everything unlabeled for later typesetting of source names and the "what elicitation fills" caption.

**BLOCK 2 — FULL SCOPE**

- **[S]** Single-column, 89mm width, 300 DPI, vector, textbook default.
- **[C]** Three chapter-confirmed knowledge sources and their causal-identification roles: (1) Observational data/telemetry — identifies the Markov-equivalence class, leaves edges undirected; (2) Rep elicitation/KEBN — orients edges, supplies susceptibility indices and mediator weights; (3) RCT/natural experiment — identifies total effect, not pathway structure. The gap between (1) and (3) is what elicitation fills.
- **[O]** Three columns, shared baseline; same edge set rendered three ways (undirected → directed → single total-effect arrow); a brace marking the elicitation-filled gap. → for orientation contributed.
- **[P]** Flat vector, white, 1pt strokes. Telemetry undirected edges neutral gray #999999; rep-oriented directed edges active Bluish Green #009E73; RCT total-effect arrow dominant Blue #0072B2; source boxes Sky Blue #56B4E9; brace black #000000. No baked text.
- **[E]** Exclude: source names and role text as baked labels; "susceptibility indices"/"mediator weights" annotations; the three-rung ladder; Pearl's-rungs naming; the M1/M2/M3 pathways; the iPad 47-second anecdote; CPT/noisy-OR content; legends.

**BLOCK 3 — NEGATIVE PROMPT**

source-name text, role labels, susceptibility/mediator annotations, rung ladder, pathway labels, anecdote elements, legends, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

FIGURE 2 — The three-rung question ladder · Critical · Process flowchart · MC

**BLOCK 1 — ILLUSTRAE PASTE**

Draw a vertical three-rung ladder ascending from bottom to top, each rung a horizontal band of equal height. The bottom rung (association) is the entry point; the middle rung (intervention) sits above it; the top rung (counterfactual) caps the climb. Within each rung, lay out three aligned slots left to right: a left slot for the question type, a center slot for what the system extracts, and a right slot for the causal role that extract plays. Connect the rungs with a single upward arrow on the left side showing the climb, and add two short feed-forward arrows on the right showing that the lower rung's output frames the rung above it — bottom rung output feeding the middle rung, middle rung output feeding the top. Render the three rungs in a graduated single-hue progression so the ascent reads as deepening causal commitment. Keep slot boxes uniform, arrows single-weight with standard arrowheads, flat vector, white background, generous spacing. Leave every slot blank for later typesetting of the prompts, extracts, and roles.

**BLOCK 2 — FULL SCOPE**

- **[S]** Single-column, 89mm width, 300 DPI, vector, textbook default.
- **[C]** Pearl's three rungs as implemented in the bot, chapter-confirmed: Rung 1 Association (open narration → extracts candidate confounders); Rung 2 Intervention (mental experiment → extracts susceptibility indices / heterogeneous-treatment-effect knowledge); Rung 3 Counterfactual (road-not-taken → extracts mediator weights across M1/M2/M3). Rung-1 output frames Rung-2; Rung-2 output frames Rung-3.
- **[O]** Vertical ladder, bottom-to-top ascent; three slots per rung (question / extract / causal role); one upward climb arrow plus two feed-forward arrows between rungs. → for ascent and feed-forward.
- **[P]** Flat vector, white, 1pt strokes. Three rungs in a single-hue progression using Sky Blue #56B4E9 (Rung 1) → Blue #0072B2 (Rung 3) to show deepening; slot boxes neutral gray #999999 outline; arrows black #000000. No baked text.
- **[E]** Exclude: the verbatim prompts as baked text; "Pearl's ladder, unspoken" caption; the words confounder/susceptibility/mediator; the dead-end leading-question example; the evidence gate (Figure 4); the annotated-DAG schema; M1/M2/M3 pathway diagram.

**BLOCK 3 — NEGATIVE PROMPT**

verbatim prompt text, ladder caption text, role-name labels, leading-question example, evidence-gate content, DAG schema, pathway diagram, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

FIGURE 3 — Annotated prior DAG edge schema · Important · Annotated example · VG + MC

**BLOCK 1 — ILLUSTRAE PASTE**

Create a structured-row schematic of how a single elicited edge is recorded. At the top, draw one edge as the subject: two nodes joined by a directed arrow with a single arrowhead. Below the edge, lay out a vertical stack of three uniform field rows, each a horizontal slot, representing the three annotations every edge carries: an evidence row, a confidence row, and a contradictions row. Beside the confidence row, draw a small three-segment vertical scale (low, medium, high) as a discrete stepped bar, not a continuous gradient, with one segment filled to indicate a confidence level. Below the three field rows, set off a separate boxed region representing the rejected-edges appendix — draw inside it one greyed-out edge with its arrow crossed by a short perpendicular block mark to indicate rejection. Keep field rows uniform, the accepted edge visually distinct from the rejected one, flat vector style, single stroke weight, white background. Leave all rows and the appendix box unlabeled for later typesetting of field names and example content.

**BLOCK 2 — FULL SCOPE**

- **[S]** Single-column, 89mm width, 300 DPI, vector, textbook default.
- **[C]** Chapter-confirmed annotated-prior-DAG edge format: each edge carries (a) Evidence — verbatim rep quote; (b) Confidence — high/med/low preserving the rep's hedge; (c) Contradictions — opposite orientations across reps/accounts. Plus the rejected-edges appendix: edges the bot proposed but the rep did not support, marked rejected. Confidence is discrete three-level.
- **[O]** One subject edge at top; three stacked uniform field rows below (evidence / confidence / contradictions); a discrete stepped low-med-high scale beside confidence; a separated appendix box holding one rejected edge marked with a block/stop mark. ⊣ for the rejection mark. → for the accepted edge.
- **[P]** Flat vector, white, 1pt strokes. Accepted edge active Bluish Green #009E73; confidence stepped scale Sky Blue #56B4E9; rejected edge neutral gray #999999 with blocking Vermillion #D55E00 stop-mark; field rows gray outline; nodes Sky Blue #56B4E9. No baked text.
- **[E]** Exclude: field names and example quotes as baked text; the full multi-edge DAG; the three-rung ladder; the LLM-role content; compliance/M3 discussion; continuous gradient for the confidence scale (must be discrete steps); JSON output structure.

**BLOCK 3 — NEGATIVE PROMPT**

field-name text, example quote text, full DAG, rung ladder, LLM-role content, compliance content, continuous confidence gradient, JSON structure, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

FIGURE 4 — The one-directional evidence gate · Critical · Process flowchart · VG

**BLOCK 1 — ILLUSTRAE PASTE**

Draw a one-directional gate diagram with two candidate inputs converging on a single gate, only one of which passes. On the left, stack two source boxes: an upper source representing the rep's volunteered observation, and a lower source representing the language model's training prior. Both send a horizontal arrow rightward toward a central gate element drawn as a narrow vertical barrier. The upper arrow (rep quote) passes cleanly through the gate and continues to the right into an accepted-edge node — two nodes joined by a directed arrow. The lower arrow (model prior) meets the gate and is stopped: terminate it at the barrier with a short perpendicular block mark and route a small return arrow downward into a separate rejected bin. The gate's asymmetry is the entire point — one path through, one path blocked. Keep the two source boxes uniform in size, the gate centered, arrows single-weight, flat vector style, white background, clear left-to-right flow. Leave all elements unlabeled for later typesetting of the source names and gate rule.

**BLOCK 2 — FULL SCOPE**

- **[S]** Single-column, 89mm width, 300 DPI, vector, textbook default.
- **[C]** Chapter-confirmed evidence gate: it runs in one direction only — a rep's volunteered, quoted observation grounds (passes) an edge into the model; the LLM's training-prior assertion is rejected (blocked) and sent to the rejected-edges bin. Necessary-in-principle vs quality-open caveat is reflected by showing the gate as a *designed safeguard* against model suggestibility, not an automatic guarantee [inferred framing — chapter states the gate's direction and the LLM hallucination risk; the "safeguard not guarantee" emphasis is the chapter's open-question caveat].
- **[O]** Left-to-right: two source boxes (rep quote upper, model prior lower) → central gate barrier → rep path passes to accepted-edge node; model path blocked, routed to rejected bin. → for passage, ⊣ for blockage.
- **[P]** Flat vector, white, 1pt strokes. Rep-quote source and accepted edge active Bluish Green #009E73; model-prior source and blocked path blocking Vermillion #D55E00; gate barrier dominant Blue #0072B2; rejected bin neutral gray #999999; arrows black #000000. No baked text.
- **[E]** Exclude: the gate rule and source names as baked text; the Shaposhnyk et al. citation; the three-rung ladder; the edge-schema fields; M3/reciprocity compliance content; the LLM's three good tasks (transcription etc.); any depiction implying the gate fully solves elicitation quality.

**BLOCK 3 — NEGATIVE PROMPT**

gate-rule text, source-name labels, citation text, rung ladder, edge-schema fields, compliance content, LLM-task list, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

## Video candidate pass

FIGURE 1 — knowledge-source contribution map
Status: STATIC SUFFICIENT
Criterion met: none — a comparison of what three fixed sources contribute.
Reason: The figure contrasts stable contributions; the reader needs to inspect the gap between sources, not watch a transition.

FIGURE 2 — three-rung question ladder
Status: VIDEO CANDIDATE
Criterion met: Criterion 2 (three or more sequential causal stages that build on each other) and Criterion 1 (the feed-forward — each rung's output framing the next — is the learning target).
Reason: The chapter's mechanism is that Rung-1 output *frames* Rung-2 and Rung-2 *frames* Rung-3; a static ladder shows the rungs but only approximates the directional dependency with arrows, whereas a step-through reveals how an open narration narrows into an intervention question and then a counterfactual. Static panels still let the reader self-pace, so this is a recommend-not-require candidate.

FIGURE 3 — annotated prior DAG edge schema
Status: STATIC SUFFICIENT
Criterion met: none — a record format, not a process.
Reason: The schema is a static template the reader inspects field by field; motion adds no instructional meaning.

FIGURE 4 — one-directional evidence gate
Status: STATIC SUFFICIENT
Criterion met: none — the gate's asymmetry is a single rule, fully shown by one pass-through and one blocked path.
Reason: Static arrows with a clear ⊣ block communicate the one-directional rule completely; animating the rejection adds drama, not learning.

Video candidates identified: 1. Recommended for production: Figure 2 — a short step-through (interactive or narrated) of the three-rung ladder, because the feed-forward dependency between rungs is a sequential mechanism a static figure can only approximate. The other three figures are well-served by static treatment — formats noted above.
