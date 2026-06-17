# CAJAL Figure Report — Chapter 04: The Natural Experiment in Every Training Rollout
_Mode: /scan silent · 2026-06-16 · source: chapters/04-the-natural-experiment-in-training-rollouts.md_

## Density recommendation

This chapter carries four genuinely figure-worthy concepts and one borderline candidate. The spine is an instrumental-variables identification argument: a structural DAG (IV three-node), a multi-step process (the falsification-first design walked in order), a structural before/after (timeline of staggered cohort exposure), and one quantitative claim that the chapter explicitly elevates as a misconception-correction (the F-statistic / test-size relationship). The chapter already embeds four `<!-- → -->` figure stubs, three of which are sound; the F-statistic stub is the one PQ figure and is the most pedagogically load-bearing.

Recommended density: **4 figures** — 2 Critical (IV DAG; F vs. test-size chart), 2 Important (cohort timeline; falsification-first process), plus the staggered-TWFE forbidden-comparison structure as 1 Supplementary if space allows (5 total ceiling). This is a methods-dense chapter; over-figuring risks turning a reasoning argument into a slide deck. Keep the DAG and the F-chart; treat the rest as support. One video candidate (the falsification-first order-of-operations) is recommended but not auto-selected.

## Figures

---

FIGURE 1 — IV three-node identification structure (Z→D→Y with confounder) · Critical · DAG · MC + VG

BLOCK 1 ILLUSTRAE PASTE:
Render a flat vector causal directed-acyclic-graph with three primary nodes arranged left to right along a single horizontal axis: an upstream instrument node, a central treatment node, and a rightmost outcome node. Draw one solid single-headed arrow from the instrument node to the treatment node, and one solid single-headed arrow from the treatment node to the outcome node, forming an unbroken left-to-right chain. Place a fourth node above and between the treatment and outcome nodes, representing an unobserved confounder, and draw two dashed single-headed arrows descending from it, one into the treatment node and one into the outcome node. Critically, draw no arrow of any kind directly from the instrument node to the outcome node, and leave that absent path visibly empty so the eye registers that the instrument reaches the outcome only by passing through treatment. Use a circular outline for the confounder to distinguish it from the three solid square observed nodes. Keep all four nodes evenly sized, aligned on a clean grid, with generous whitespace. Leave every node blank for caption labeling. Flat vector, white background, one-point strokes, no shading.

BLOCK 2 FULL SCOPE:
[S] Single-column 89mm, 300 DPI, vector, textbook line figure.
[C] Chapter-confirmed: instrument Z = training-cohort assignment; treatment D = message-variant exposure; outcome Y = NRx; the unobserved confounder corrupting the D→Y comparison (chapter: "physician's openness drives both"); the deliberate absence of a Z→Y direct path (the exclusion restriction made visual). Confounder rendered as a single latent node [inferred — chapter describes it but never names a single variable].
[O] Linear left-to-right node chain Z → D → Y as primary spine; confounder node positioned above, with two dashed arrows ⟶ descending into D and into Y; the Z→Y path deliberately absent (⊣ shown as empty space, not a blocked arrow). Square nodes = observed; circle node = latent.
[P] Flat vector, white background, Okabe-Ito. Z (anchor/instrument) #56B4E9; D (treatment) #0072B2; Y (outcome) neutral gray #000000 outline; confounder node + its two dashed arrows #D55E00 (the threat). Solid 1pt strokes for the identified Z→D→Y chain; dashed 1pt for confounder paths.
[E] No Z→Y arrow. No bidirectional/dual-headed arrows (render the confounder as two separate single-headed dashed arrows, not one curved double-head). No labels baked in. No additional mediator nodes (those belong to Chapter 5). No first-stage/reduced-form annotations inside the graphic.

BLOCK 3 NEGATIVE PROMPT:
double-headed confounder arrow, Z-to-Y direct arrow, baked-in node labels, mediator nodes, first-stage equations, more than four nodes, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

FIGURE 2 — First-stage F vs. actual test size (the F>10 myth) · Critical · Line chart · PQ + MC

BLOCK 1 ILLUSTRAE PASTE:
Render a flat vector single-panel line chart. The horizontal axis runs from a low value near one to a high value near two hundred, representing a first-stage strength statistic; the vertical axis runs from zero up to a moderate ceiling, representing the actual size of a nominal five-percent statistical test, with the vertical axis beginning at zero. Draw one smooth monotonically descending curve: high on the left where the statistic is small, falling steeply, then flattening as it approaches the right. Draw one horizontal reference line at the five-percent level near the bottom. The descending curve should meet that horizontal reference line only far to the right, at roughly the one-hundred-five position on the horizontal axis. Draw one short vertical reference line at the value ten on the horizontal axis, and mark clearly where the descending curve sits directly above that vertical line — visibly well above the five-percent horizontal reference. Draw a second short vertical reference line at the one-hundred-five position where the curve finally touches five percent. Use distinct line weights so the data curve reads as primary and the two reference lines and the five-percent line read as secondary guides. Leave all axes and points unlabeled for later captioning. White background, flat vector, no shading.

BLOCK 2 FULL SCOPE:
[S] Single-column 89mm, 300 DPI, vector. Bar/line y-axis from zero (chapter: a size value; zero baseline mandatory).
[C] Chapter-confirmed: x = first-stage F from 1 to 200; y = actual size of nominal 5% t-test; horizontal line at 5%; curve descends to 5% only near F ≈ 104.7 (Lee, McCrary, Moreira & Porter 2022); vertical marker at F = 10 where actual size is well above 5% (the legacy threshold's failure). Curve shape (smooth monotone descent) [inferred — chapter states the two anchor points 10 and ~104.7 and the direction; exact intermediate curvature is illustrative].
[O] Cartesian chart, y from zero; one descending data curve as dominant; horizontal 5% guide and two vertical guides (at 10 and ~104.7) as secondary; emphasis dot where curve crosses each vertical guide.
[P] Flat vector, white background, Okabe-Ito. Data curve dominant #0072B2; 5% horizontal guide neutral gray; F=10 marker (the wrong threshold) #D55E00; F≈104.7 marker (the correct threshold) #009E73; markers 1pt.
[E] No values baked onto axes. No tF-correction second curve (keep one curve to avoid clutter — name in caption). No log axis distortion. Y-axis must start at zero. No annotation callouts inside plot area.

BLOCK 3 NEGATIVE PROMPT:
truncated y-axis, second overlaid curve, baked axis numbers, log-scale distortion, annotation callout boxes, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

FIGURE 3 — Staggered cohort exposure timeline · Important · Timeline / structure · VG

BLOCK 1 ILLUSTRAE PASTE:
Render two horizontal parallel timeline bars stacked vertically, each representing one training cohort and spanning the same left-to-right time range. Divide each bar into two contiguous segments: an earlier segment in one fill representing the old message deck, and a later segment in a contrasting fill representing the new message deck. On the upper bar (the early cohort) the switch from old to new fill occurs near the left, early in the time range. On the lower bar (the late cohort) the switch occurs much later, near the right. Add a thin shared time ruler beneath both bars with evenly spaced tick marks. Draw one short single-headed arrow descending from each bar's mid-period region down to a single small node beneath both timelines, representing a comparable physician, so the two arrows converge to show that during the same calendar window the two cohorts were delivering different decks. Keep the two bars perfectly aligned on the same time axis, equal in length and height, with generous spacing. Leave all segments, ticks, and the node unlabeled. Flat vector, white background, one-point strokes, no shading or gradients.

BLOCK 2 FULL SCOPE:
[S] Single-column 89mm, 300 DPI, vector.
[C] Chapter-confirmed: two cohorts (A early, B late) on a shared timeline; cohort A switches to new deck in month 1, cohort B in month 3 (chapter stub); same-window comparison of otherwise-comparable physicians; the timing variation IS the instrument. Convergence-to-one-physician device taken from the chapter's own diagram stub.
[O] Two stacked horizontal bars on a shared time ruler; old-deck → new-deck segment transition on each (→ progression along time); early switch (top) vs. late switch (bottom) is the before/after structural claim; two arrows ⟶ converging on a single physician node.
[P] Flat vector, white background, Okabe-Ito. Old-deck segment secondary #E69F00; new-deck segment active #009E73; physician node + convergence arrows anchor #56B4E9; time ruler neutral gray; 1pt strokes.
[E] No month numbers or labels baked in. No more than two cohorts (chapter's example uses two; more would clutter). No probability/percent annotations. No third "control" bar. Keep arrows single-headed.

BLOCK 3 NEGATIVE PROMPT:
baked month labels, third cohort bar, percentage annotations, more than two timelines, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

FIGURE 4 — Falsification-first design (order of operations) · Important · Process flow · MC

BLOCK 1 ILLUSTRAE PASTE:
Render a vertical flowchart of five stacked stages connected top to bottom by single-headed arrows. Stage one is a rounded rectangle representing building the linked panel. Stage two is a diamond decision node representing the falsification test, with two exits: one exit arrow pointing sideways to a terminal node representing "stop and report failure," and one exit continuing downward representing "proceed." Stage three, reached only on the downward proceed path, is a rounded rectangle representing the robust difference-in-differences estimate that replaces a crossed-out naive specification — show the naive specification as a small node with a single diagonal strike line through it, bypassed by the flow. Stage four is a rounded rectangle representing the instrumental-variables estimate with honest inference. Stage five is a terminal rounded rectangle representing naming the local complier estimate as the limit. The decision diamond must be the visual pivot: the sideways "stop" branch and the downward "proceed" branch should be equally weighted so the gate reads as a true fork, not a formality. Align all stages on a single vertical centerline with the stop-branch offset to one side. Leave every node blank. Flat vector, white background, one-point strokes.

BLOCK 2 FULL SCOPE:
[S] Single-column 89mm, 300 DPI, vector, tall aspect.
[C] Chapter-confirmed five-step sequence from "Running the design, with the dead ends": (1) build the panel; (2) run falsification test first — gate; (3) walk through/abandon the TWFE dead end, swap in Callaway–Sant'Anna; (4) estimate with honest inference (effective F, tF, Anderson–Rubin); (5) name the limit (population LATE for compliers). The kill-criterion fork (significant at 5% → stop) is chapter-explicit (§8 "kill criterion").
[O] Top-to-bottom process spine with → arrows; one diamond decision gate with a ⊣ blocking branch (stop/report failure) splitting from the proceed branch; the naive TWFE node struck through (⊣) and bypassed; terminal node at base.
[P] Flat vector, white background, Okabe-Ito. Process stages neutral gray outline; the falsification gate (the load-bearing step) #56B4E9; the stop/fail branch #D55E00; the robust-estimator swap #009E73; struck-out naive node neutral gray with #D55E00 strike. Arrows 1pt single-headed.
[E] Max 6 components — exactly: build, gate, robust-estimate (with struck naive), IV-estimate, name-limit, stop-terminal. No inline statistic names baked as text. No second decision diamond. Do not render the TWFE dead end as a full parallel branch — a single struck node bypassed by flow is sufficient.

BLOCK 3 NEGATIVE PROMPT:
second decision diamond, parallel TWFE branch, baked statistic names, more than six nodes, swimlanes, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

FIGURE 5 — Staggered-TWFE forbidden comparison (negative weights) · Supplementary · Structural diagram · VG

BLOCK 1 ILLUSTRAE PASTE:
Render a compact structural diagram contrasting a permitted comparison with a forbidden one. Show three short horizontal cohort bars stacked vertically, each split into an untreated segment on the left and a treated segment on the right, with the treatment-start point progressively later from the top bar to the bottom bar, so the staggered onset is visible as a descending staircase of switch points. Draw one solid single-headed arrow representing a permitted comparison: from a later-treated cohort to a not-yet-treated cohort serving as a clean control. Draw one second single-headed arrow representing the forbidden comparison: from a later-treated cohort back to an already-treated earlier cohort being misused as a control, and mark this second arrow with a single small strike or annotation point indicating it carries a negative weight. The two arrow types must be visually distinct in color and the forbidden one clearly flagged. Keep the three cohort bars aligned on a shared horizontal time axis with equal heights. Leave all segments and arrows unlabeled. Flat vector, white background, one-point strokes, no shading.

BLOCK 2 FULL SCOPE:
[S] Single-column 89mm, 300 DPI, vector.
[C] Chapter-confirmed (Goodman-Bacon 2021): staggered TWFE = weighted average of 2×2 DiD comparisons; some use already-treated early cohorts as controls (forbidden); these enter with negative weights and can sign-flip the estimate; the fix uses not-yet-treated cohorts. Three-cohort staircase [inferred — minimal illustration of "staggered timing"; chapter does not fix a count].
[O] Three staggered cohort bars (untreated → treated segments, descending switch points); one permitted-comparison arrow ⟶ (to not-yet-treated); one forbidden-comparison arrow ⟶ (to already-treated) flagged as negative-weight.
[P] Flat vector, white background, Okabe-Ito. Untreated segment neutral gray; treated segment secondary #E69F00; permitted comparison arrow #009E73; forbidden/negative-weight arrow #D55E00 with strike marker. 1pt strokes.
[E] Max 3 cohort bars + 2 arrows. No weight numbers baked in. No equation. Do not render every pairwise 2×2 comparison — show one permitted and one forbidden only, named in caption. Single-headed arrows only.

BLOCK 3 NEGATIVE PROMPT:
all pairwise comparison arrows, baked weight values, regression equations, more than three cohort bars, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

## Video candidate pass

**FIGURE 4 — Falsification-first design (order of operations)**
- **Status:** Recommended (not auto-selected).
- **Criterion met:** ≥3 sequential causal stages where the *order* is the learning target; the chapter explicitly states grading emphasis is order of operations ("A design that shows an effect estimate before establishing that the falsification test passes is marked incomplete, by construction").
- **Reason:** The pedagogical payload is the *gate* — the estimate cannot run until the falsification test passes. A static flowchart shows the structure; a short animated build (panel → gate fires → fork: stop vs. proceed → robust swap → IV → LATE) makes the blocking semantics of the kill criterion legible in a way a still cannot. The "stop and report failure" branch lighting up is the moment worth animating.
- **Suggested format:** 20–30s vector build animation, stage-by-stage reveal, with the decision diamond animating both exits (the stop branch flashing #D55E00, the proceed branch continuing) before resolving to the proceed path. No narration baked into the asset.

Honesty note carried into Figures 1 and 4: the cohort instrument's independence is **testable, not assumed** — the falsification regression must falsify independence first; Figure 4's gate is the visual encoding of exactly that. The weak-IV F>10 rule depicted in Figure 2 is **legacy**; the figure deliberately shows the modern ~104.7 threshold as the real bar. Exclusion is threatened if early training improves general selling skill — not directly depicted (untestable channel), correctly left out of the DAG rather than drawn as a fake measurable edge.
