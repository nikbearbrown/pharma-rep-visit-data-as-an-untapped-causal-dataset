# CAJAL Figure Report — Chapter 01: The Experiment Nobody Knows They're Running
_Mode: /scan silent · 2026-06-16 · source: chapters/01-the-experiment-nobody-knows.md_

## Density recommendation
Conceptually dense, light on hard numbers — this chapter argues four ideas (the cool-vs-causal gap, single-trace telemetry, the 47-second ambiguity, the cohort natural experiment) and asserts only two soft quantities (~80% Veeva share, "fifteen years"). Favor four mechanism/verification figures over any quantitative chart; the two numbers are flagged `[verify]` in-text and should NOT be rendered as charts. Target 4 figures: 2 Critical, 2 Important. One video candidate (the 47-second branching).

## Figures

---

FIGURE 1 — Single rep-visit trace as a behavioral timeline, then stacked longitudinally · Priority: Critical · Type: timeline / small-multiple stack · Heuristic: VG

BLOCK 1 — ILLUSTRAE PASTE BLOCK:
Render a horizontal timeline representing one pharmaceutical sales-call session. Lay out a left-to-right row of contiguous rectangular blocks, each block one slide viewed, the width of each block proportional to dwell seconds so some blocks are wide and some narrow. Color each block by a three-state reaction reading: positive, neutral, negative. Show one slide skipped as a gap or thin marker, and show one back-track as a single curved connector returning to an earlier block. Beneath this single row, stack five or six more rows of the same form, each row a separate month for the same physician, vertically aligned on a shared left edge to read as one longitudinal series for one person. Keep all rows the same height and aligned to a common time origin on the left. Use flat vector rectangles, a white background, one stroke weight, and a small palette mapping the three reaction states plus a neutral gray for skipped content. Make the top single trace visually primary and the stacked block secondary in weight. No numeric axis ticks, no slide names, no legend text.

BLOCK 2 — FULL SCOPE:
[S]pecification: single-column 89mm, 300 DPI, vector, textbook figure.
[C]ontent: chapter-confirmed only — one session = a sequence of slide views; width = dwell seconds; color = rep-tapped reaction (positive/neutral/negative); skips and back-tracks present; stacking the same physician across ~six months yields a longitudinal series. The "census across all physicians and reps" extension is mentioned in text but [inferred] as a third panel — omit to stay honest; depict only one-visit and six-month layers.
[O]rganization: top panel = single trace (anchor); below = 5–6 stacked traces sharing a left time origin → vertical progression reads month-over-month. No interactivity.
[P]resentation: flat vector, white bg, Okabe-Ito — positive #009E73, neutral light gray, negative #D55E00, primary trace outline #0072B2; 1pt strokes. Back-track connector a single standard curved arrow.
[E]xclusions: no numeric tick labels, no slide titles, no legend, no axis numbers, no second/minute marks, no realistic iPad device, no UI chrome, no heatmap coloring, no dual-headed arrows, no "census" third panel, no rep or physician avatars.

BLOCK 3 — NEGATIVE PROMPT:
heatmap gradients, iPad device rendering, UI screenshot chrome, numeric tick marks, legend boxes, dual-headed arrows, third census panel, human avatars, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, hand-drawn styles, sketch lines, human figures (unless explicitly requested), visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

FIGURE 2 — One datum, three incompatible causal stories (the 47-second slide) · Priority: Critical · Type: branching interpretation tree · Heuristic: MC

BLOCK 1 — ILLUSTRAE PASTE BLOCK:
Render a branching diagram that begins from a single source node on the left representing one identical observation. From that single node draw three diverging branches to three terminal nodes on the right. The three terminals represent three mutually incompatible interpretations of the same observation: careful absorption read as a positive educational signal; skeptical scrutiny read as a negative signal; and drifted attention read as pure noise. Make the single left node visually dominant to emphasize that all three stories share one origin. Render the three branches as clean diverging connectors of equal weight so no interpretation is privileged. Differentiate the three terminal nodes by color to mark positive, negative, and neutral character. Keep the geometry symmetric, one source fanning to three sinks, on a white background with flat vector shapes and a single stroke weight. Convey that the branching happens after the data, in interpretation, not in the recorded number itself. No text inside nodes, no captions, no numeric values shown.

BLOCK 2 — FULL SCOPE:
[S]pecification: single-column 89mm, 300 DPI, vector, textbook.
[C]ontent: chapter-confirmed — identical datum (47s on safety slide) → three incompatible stories: compelled/careful reading = positive; skeptical re-reading = negative; distracted = noise. The text stresses these are equally consistent and undistinguishable from data; depict equal-weight branches.
[O]rganization: one source node (left, anchor) → three diverging branches → three terminal nodes (right). Strict 1-to-3 fan. Branch order top-to-bottom: positive, negative, neutral.
[P]resentation: flat vector, white bg, Okabe-Ito — source node #0072B2 (dominant conceptual), positive terminal #009E73, negative terminal #D55E00, neutral terminal light gray; connectors #000000 1pt, single standard arrowheads.
[E]xclusions: no text in nodes, no "47" numeral, no probability weights on branches, no rep/physician figures, no thought bubbles, no facial expressions, no slide imagery, no fourth branch, no unequal branch weighting, no convergence back to a single answer.

BLOCK 3 — NEGATIVE PROMPT:
probability weights, branch thickness variation, thought bubbles, facial expressions, slide imagery, fourth branch, convergence node, numerals, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures (unless explicitly requested), visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

FIGURE 3 — The unrun experiment: cohort × rollout checkerboard · Priority: Important · Type: grid / checkerboard matrix · Heuristic: VG

BLOCK 1 — ILLUSTRAE PASTE BLOCK:
Render a rectangular grid representing time on the horizontal axis and rep training cohorts on the vertical axis. Each row is one training cohort; each column is one time period across roughly a decade. Fill each cell in one of two distinct colors marking whether that cohort was delivering the old message deck or the new message deck in that period. Arrange the fills so the new-message color appears at different horizontal positions for different rows — earlier for cohorts certified sooner, later for cohorts certified later — producing a staggered, checkerboard-like front that sweeps across the grid rather than switching all at once. Overlay a small number of vertical guide lines marking distinct rollout waves. The visual point is that the boundary between old and new is staggered across cohorts, created by administrative training schedules and not by anything about the physicians. Keep cells uniform in size and aligned, white background, flat fills, one stroke weight. No axis numbers, no year labels, no cohort names.

BLOCK 2 — FULL SCOPE:
[S]pecification: single-column 89mm, 300 DPI, vector, textbook.
[C]ontent: chapter-confirmed — rows = rep cohorts, columns = time (~2010 to present, treat as approximate; "fifteen years" is `[verify]`/asserted, so show an unlabeled span not dated ticks), two states per cell (old deck / new deck), staggered adoption front driven by training-certification timing. Carry the honesty caveat: variation is "plausibly" as-good-as-random — this is a motivating claim, not yet identified; do not imply proven randomization.
[O]rganization: matrix; staggered diagonal-ish front of new-message cells → reads as wave rollout; 3–4 vertical wave guides.
[P]resentation: flat vector, white bg, Okabe-Ito — old deck light gray (neutral), new deck #E69F00 (secondary), wave guide lines #000000 thin 0.5pt; cell strokes 1pt. Avoid implying treatment-positive green so the "not yet identified" caveat holds.
[E]xclusions: no year numbers, no cohort ID text, no axis titles, no physician markers inside cells, no arrows asserting causation, no legend text, no map/geography rendering, no smooth gradient sweep (must be discrete cells).
[E] — highest leverage: the staggering must be irregular/administrative-looking, NOT a clean diagonal that implies a designed experiment.

BLOCK 3 — NEGATIVE PROMPT:
year numbers, axis titles, cohort labels, smooth gradient sweep, clean perfect diagonal, geographic map, causation arrows, legend boxes, physician icons, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures (unless explicitly requested), visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

FIGURE 4 — Dashboard instinct to do-question: the dead-end and the fix · Priority: Important · Type: process flow with a blocked path · Heuristic: MC

BLOCK 1 — ILLUSTRAE PASTE BLOCK:
Render a process diagram of an analyst's first encounter with the telemetry. Begin at a left start node representing the raw telemetry. Draw a path to a node representing the reflexive move: correlate engagement with prescribing. From there draw a path to a finding node representing an encouraging correlation, and then to a terminal node representing a recommendation. Mark this entire path as a dead end by drawing a clear blocking bar across the connector leading out of the recommendation node, signaling it is not valid evidence. Then, from an earlier branch point near the raw-telemetry node, draw a second, separate path that detours through a node representing a different comparison driven by training-cohort timing, and lead that path to a distinct terminal node representing a precisely written do-question. Make the second path visually the resolved/active route and the first path the blocked route. Keep two clearly parallel horizontal lanes, white background, flat vector nodes of uniform shape, single stroke weight, standard single-headed arrows only.

BLOCK 2 — FULL SCOPE:
[S]pecification: single-column 89mm, 300 DPI, vector, textbook.
[C]ontent: chapter-confirmed — starting instinct (correlate dwell/engagement with prescribing) → encouraging correlation → recommendation, which is a dead end because the comparison is confounded (rep chose where to detail). The fix = a different comparison using training-cohort rollout → the do-question P(NRx | do(message)). The blocking bar depicts "wrong as evidence." Mark the do-question terminal as the chapter's named artifact.
[O]rganization: two horizontal lanes from a shared origin. Top lane = dead end, terminated by a ⊣ blocking bar. Bottom lane = fix, → progression to do-question node. Branch split point explicit at origin.
[P]resentation: flat vector, white bg, Okabe-Ito — start/raw node #56B4E9 (primary anchor), dead-end path nodes light gray, blocking bar #D55E00 (blocking/negative), fix-path nodes #009E73 (active/positive), do-question terminal #0072B2 (dominant); arrows #000000 1pt single-headed.
[E]xclusions: no code or regression formula text, no numeric correlation values, no do() notation typeset (depict the artifact as a distinct node, not as words), no scatterplot inset, no rep/physician figures, no third lane.

BLOCK 3 — NEGATIVE PROMPT:
regression formulas, scatterplot inset, correlation numbers, typeset do() notation, third lane, code snippets, human figures, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

## Video candidate pass

VIDEO CANDIDATE — Figure 2 (47-second slide → three causal stories)
- Status: Recommended
- Criterion met: sub-observable transformation — the learning target is that one fixed observation underdetermines three mechanisms; the *act of branching from a single datum into incompatible interpretations* is the mechanism being taught.
- Reason: A short build animation (single datum lands → fans into three stories → none can be eliminated by more data) makes the underdetermination viscerally clear in a way a static fan cannot; the "you cannot collapse these back" beat is the payload and recurs as the book's emblem (reappears Ch.3).
- Suggested format: 10–15s silent build/morph animation, three branches appearing sequentially then a brief "no path collapses" hold; no narration text needed.

Other concepts (timeline trace, checkerboard, dead-end flow) are static-appropriate: they show structure/before-after, not an essential transition mechanism. Target of ~1 video/chapter met.
