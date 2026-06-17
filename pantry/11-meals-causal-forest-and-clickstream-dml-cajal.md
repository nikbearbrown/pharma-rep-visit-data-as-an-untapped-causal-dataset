# CAJAL Figure Report — Chapter 11: Meals (Causal Forest) and Slide Sequencing (Double Machine Learning)
_Mode: /scan silent · 2026-06-16 · source: chapters/11-meals-causal-forest-and-clickstream-dml.md_

## Density recommendation

This chapter runs two parallel projects that the prose explicitly frames as "one family," and it carries three inline `[DIAGRAM]`/`[CHART]` markers of its own: predictive-vs-causal forest, the DML three-box residual flow, and the collider-contamination comparison chart. Those three are the chapter's load-bearing exhibits and are all Critical. The chapter's deepest single idea — post-treatment conditioning (mediator vs collider) corrupting the estimate — deserves one dedicated structural figure beyond the three-box flow, because the chapter returns to it four times and it is the "central misconception." One Important figure should make the public/proprietary firewall and the "same family" unification visible, since the chapter argues both explicitly. Recommended density: **5 figures** — 3 Critical (the marked exhibits), 1 Important (the mediator-vs-collider SCM contrast), 1 Supplementary (the firewall / two-project map). Do not over-figure the heterogeneity readout — it has no planted magnitudes (Project 2 has no ground truth) and would produce a speculative bar chart that violates the no-fabrication rule.

## Figures

---

FIGURE 1 — Predictive forest vs causal forest · Critical · Comparison diagram · MC + VG

BLOCK 1 ILLUSTRAE PASTE:
Render a flat vector two-column comparison diagram contrasting a standard predictive random forest against a causal forest. In the left column, depict the predictive forest: a single data block flowing into a tree-ensemble glyph that uses all of the data for both splitting and leaf estimation, terminating in a single predicted-outcome node rendered in secondary orange. In the right column, depict the causal forest: a data block that splits into two disjoint subsamples — one subsample routed to a "decide where to split" node, the disjoint other routed to an "estimate the effect inside each leaf" node — these being the honest-splitting discipline, converging into a tree-ensemble glyph that terminates in a per-unit treatment-effect node carrying a confidence-interval whisker, rendered in active green. Place three small contrast markers along the centerline aligning the two columns on the dimensions that differ: estimand, splitting discipline, and inference validity. Use orthogonal single-weight arrows, anchor-blue for the honest-split routing, white background, exact column alignment, no baked text.

BLOCK 2 FULL SCOPE:
- [S] Single-column 89mm, 300 DPI, vector, textbook.
- [C] Left: predictive forest learns E[Y|X], all data for split + estimate, output = predicted outcome. Right: causal forest learns τ(x)=E[Y(1)−Y(0)|X], honest splitting on disjoint subsamples (one for split location, one for leaf effect), output = per-unit effect + pointwise CI. Three contrast dimensions: estimand, splitting discipline, inference validity. All chapter-confirmed (Wager & Athey 2018).
- [O] Two parallel columns. Left: data → ensemble → outcome node. Right: data → fork into two disjoint subsamples → ensemble → effect node + CI whisker. Centerline contrast markers aligning the two on three dimensions.
- [P] Flat vector, white bg, Okabe-Ito: predictive output #E69F00 (secondary), causal output #009E73 (active), honest-split routing #56B4E9 (anchor), neutral gray for shared structure, 1pt strokes.
- [E] Exclude: actual decision-tree node text, mathematical estimand notation as rendered glyphs, more than three contrast dimensions, realistic foliage/tree imagery (use abstract ensemble glyph, not a literal tree). Do not draw splits as a botanical tree.

BLOCK 3 NEGATIVE PROMPT:
botanical tree imagery, leaf foliage, estimand equations, decision-tree node text, fourth contrast dimension, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

FIGURE 2 — DML three-box residual-on-residual flow · Critical · Process flow · MC

BLOCK 1 ILLUSTRAE PASTE:
Render a flat vector horizontal three-box process flow showing the Double Machine Learning residual-on-residual mechanics. Box one: regress the outcome Y on the controls Z using machine learning, emitting an outcome-residual output. Box two: regress the treatment D on the same controls Z using machine learning, emitting a treatment-residual output. Box three: regress the outcome-residual on the treatment-residual, emitting the causal parameter estimate, rendered as the dominant-blue terminal node. Draw the two residual outputs from boxes one and two converging by orthogonal arrows into box three. Below the flow, attach a discipline annotation band split into two roles using a thin divider: the diagram-chooses-Z role on one side (anchor blue) and the ML-adjusts-for-Z role on the other (neutral gray), making explicit that these are different jobs for different actors. Add a single blocking-vermillion warning marker, a blocked terminator, attached to the controls input Z, flagging that a post-treatment variable entering Z causes structural bias that cross-fitting cannot repair. Single-weight strokes, white background, exact alignment, no baked text.

BLOCK 2 FULL SCOPE:
- [S] Single-column 89mm, 300 DPI, vector, textbook.
- [C] Three boxes, chapter-confirmed: (1) Y~Z via ML → Ỹ; (2) D~Z via ML → D̃; (3) Ỹ~D̃ → θ̂. Two residuals converge into box 3. Discipline split: "diagram chooses Z" vs "ML adjusts for Z." Warning: post-treatment variable in Z → structural bias, cross-fitting does not save you (chapter's explicit red annotation).
- [O] Horizontal: Box1 + Box2 (parallel) → residual outputs converge → Box3 → θ̂ terminal. Z input feeding boxes 1 and 2. Two-role annotation band beneath. Blocked terminator (⊣) warning on Z.
- [P] Flat vector, white bg, Okabe-Ito: θ̂ terminal #0072B2 (dominant), diagram-role marker #56B4E9 (anchor), ML-role marker neutral gray, warning terminator #D55E00 (blocking), residual paths #000000, 1pt strokes.
- [E] Exclude: the residual equations as rendered math, cross-fitting fold subscripts as text, ML model names (GBM/RF) as text, more than three boxes. Keep the warning to a single marker — do not clutter with multiple red flags.

BLOCK 3 NEGATIVE PROMPT:
residual equations, fold subscripts, ML model names, fourth box, multiple warning flags, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

FIGURE 3 — Collider-contamination comparison (correct vs contaminated DML) · Critical · Effect-estimate comparison chart · PQ + VG

BLOCK 1 ILLUSTRAE PASTE:
Render a flat vector estimate-comparison chart showing three effect-estimate states against a known planted ground truth. Draw a horizontal ground-truth reference line spanning the panel at the true sequence-effect level, in neutral gray. Plot two point estimates with confidence-interval whiskers as vertical elements. The first, the correctly-specified DML using pre-treatment controls only, is an active-green point sitting on or very close to the ground-truth line, with an honest, moderately wide confidence whisker that overlaps the truth line. The second, the collider-contaminated DML, is a blocking-vermillion point displaced clearly away from the ground-truth line, carrying a deceptively narrower confidence whisker that does not overlap the truth line — the tight-interval-around-a-biased-number failure. Anchor the vertical effect axis at a visible zero baseline so the displacement reads honestly, and keep the ground-truth reference visually dominant as the comparison datum. The narrower whisker on the wrong estimate must be visibly tighter than the correct one. Single-weight strokes, white background, no baked text.

BLOCK 2 FULL SCOPE:
- [S] Single-column 89mm, 300 DPI, vector, textbook.
- [C] Three states, chapter-confirmed: true effect (horizontal reference); correct DML (CI overlaps truth, honest width); collider-contaminated DML (point displaced off truth, CI narrower AND not overlapping truth). The narrower-CI-on-wrong-answer is the chapter's load-bearing point. Magnitudes [inferred — synthetic, "to be generated"; render relative displacement and relative CI width only].
- [O] Vertical effect axis with visible zero. Horizontal ground-truth reference line. Two point-estimate columns with CI whiskers: correct (on truth, wider whisker) and contaminated (off truth, tighter whisker).
- [P] Flat vector, white bg, Okabe-Ito: correct estimate #009E73 (active), contaminated estimate #D55E00 (blocking), ground-truth reference neutral gray, zero baseline #000000, 1pt strokes, whiskers thin.
- [E] Exclude: numeric effect values (unconfirmed magnitudes — relative only), more than two estimates, shaded CI bands (use whiskers), bar fills below points (this is a point-and-whisker comparison, not a bar chart). Mark magnitudes "[inferred]." Do not invent a specific drift number.

BLOCK 3 NEGATIVE PROMPT:
numeric effect values, third estimate, shaded CI bands, bar fills, legend text, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

FIGURE 4 — Mediator vs collider: why post-treatment conditioning corrupts · Important · Structural causal-graph contrast · VG + MC

BLOCK 1 ILLUSTRAE PASTE:
Render a flat vector two-panel structural causal graph contrasting the two ways an in-visit signal corrupts the estimate when placed in the control set. Both panels share the same backbone nodes: treatment (slide sequence) on the left, outcome (prescribing) on the right, connected by the causal path under study. In the left panel, depict the mediator case: the in-visit signal node sits on the path from treatment to outcome, with treatment pointing into it and it pointing into outcome; mark a conditioning box around the mediator and show the through-path being severed with a blocking terminator, illustrating that residualizing on it strips out part of the effect (controlled direct effect, not total effect). In the right panel, depict the collider case: the in-visit signal node is jointly caused by treatment and by an unobserved physician characteristic that also affects outcome; mark a conditioning box around the collider and show a new spurious path opening between treatment and outcome as a composite reddish-purple non-causal connection. Use directed single-weight arrows throughout, anchor-blue for the legitimate causal backbone, vermillion for the blocked/severed path, composite reddish-purple for the opened spurious path, white background, no baked text.

BLOCK 2 FULL SCOPE:
- [S] Single-column 89mm, 300 DPI, vector, textbook.
- [C] Two panels, chapter-confirmed: (left) mediator — treatment→signal→outcome, conditioning blocks part of the true path = controlled direct effect not total; (right) collider — signal ← treatment and signal ← physician characteristic → outcome, conditioning opens spurious path = bias where none existed. Shared treatment→outcome backbone. In-visit signals = dwell/reaction (downstream of treatment per Ch5 SCM).
- [O] Two side-by-side DAG panels sharing treatment (left node) and outcome (right node). Left: signal on-path, conditioning box, severed through-path (⊣). Right: signal as common effect, conditioning box, opened spurious path.
- [P] Flat vector, white bg, Okabe-Ito: causal backbone #56B4E9 (anchor), severed/blocked path #D55E00 (blocking), opened spurious path #CC79A7 (composite), unobserved characteristic neutral gray, directed 1pt arrows.
- [E] Exclude: node text labels, more than the necessary 4–5 nodes per panel, probability notation, a third failure case (only mediator and collider). Keep each panel ≤5 nodes. Do not label nodes — rely on position and shared backbone.

BLOCK 3 NEGATIVE PROMPT:
node text labels, probability notation, third panel, more than five nodes per panel, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

FIGURE 5 — Public/proprietary firewall and the one-family map · Supplementary · Schematic map · MC

BLOCK 1 ILLUSTRAE PASTE:
Render a flat vector schematic map placing the chapter's two projects on either side of a public/proprietary data firewall while showing they share one methodological backbone. Draw a single central vertical divider as the firewall. On the public side, place Project 2 (meals, causal forest): a data node fed by two public sources, CMS Open Payments and Medicare Part D, flowing into a heterogeneous-effect estimator glyph that emits a per-physician effect output, rendered in active green to denote fully runnable. On the proprietary side, place Project 3 (slide sequencing, DML): a data node whose real partner sources are shown blocked by a vermillion blocked terminator, with a synthetic-data substitute node carrying a planted-ground-truth marker routed in instead, flowing into an average-parameter estimator glyph that emits a single causal-parameter output, rendered in dominant blue. Beneath both, draw a shared-backbone band spanning the firewall labeled by position as the common orthogonalized residual-on-residual foundation, in neutral gray, with two small tributary markers — cross-fitting and honest splitting — feeding it from the two sides. Orthogonal single-weight arrows, white background, no baked text.

BLOCK 2 FULL SCOPE:
- [S] Single-column 89mm (flag full-width if crowded), 300 DPI, vector, textbook.
- [C] Chapter-confirmed: Project 2 public (Open Payments + Part D → causal forest → per-physician CATE, fully runnable); Project 3 proprietary (partner clickstream blocked → synthetic substitute with planted truth → DML → average θ); shared orthogonalized residual-on-residual backbone; cross-fitting (DML) and honest splitting (forest) as the two bias-removal devices feeding the common backbone. Central firewall divider.
- [O] Central vertical firewall. Left (public/green): two source nodes → forest → CATE output. Right (proprietary/blue): blocked partner source (⊣) + synthetic substitute (truth marker) → DML → θ output. Bottom shared-backbone band spanning both, with cross-fitting + honest-splitting tributaries.
- [P] Flat vector, white bg, Okabe-Ito: Project 2 runnable #009E73 (active), Project 3 estimator #0072B2 (dominant), blocked partner source #D55E00 (blocking), shared backbone neutral gray, synthetic-truth marker #56B4E9 (anchor), 1pt strokes.
- [E] Exclude: dataset filenames/field names as text (Veeva fields dated, no baked text), more than two public sources, citation text, the systematic-review names. Keep ≤6–8 components total. Do not render data sources as realistic database cylinders with texture.

BLOCK 3 NEGATIVE PROMPT:
dataset filenames, Veeva field names, database cylinder textures, citation text, third project, more than eight components, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

## Video candidate pass

**FIGURE 4 — Mediator vs collider structural contrast**
- Status: RECOMMENDED (primary candidate, ~1/chapter)
- Criterion: sub-observable transformation is the learning target — the *act of conditioning* changes the graph's open/closed paths, and that state-change (a path being severed vs a spurious path opening) is precisely what cannot be observed in data and is the chapter's "invisible failure."
- Reason: The chapter's central claim is that the corruption is structurally invisible — "nothing on the dashboard says you conditioned on a collider." A short animation that drops a conditioning box onto the in-visit signal node and shows, in real time, the through-path severing (mediator case) or the spurious path lighting up (collider case) makes the otherwise-abstract do-calculus consequence legible. The transition — clean graph → conditioned graph → corrupted estimand — is the mechanism, not just the endpoints.
- Suggested format: 20–30s silent vector animation, two scenes sharing the treatment→outcome backbone: Scene A (mediator) drops the conditioning box, the on-path link dims/severs in vermillion, a side caption-free indicator shows total→direct-effect collapse; Scene B (collider) drops the conditioning box, a new composite reddish-purple spurious arc animates into existence between treatment and outcome. End on both estimates drifting off a ground-truth line. No narration text; rely on path-state color transitions.

**FIGURE 2 — DML three-box flow**
- Status: NOT recommended (build static only)
- Criterion: fails — it is a fixed pipeline of three steps with no sub-observable transformation; the sequence is fully legible statically and is reference material, not a mechanism whose *transition* is the lesson.
- Reason: A learner reads the three boxes once and has it. Animation adds nothing the arrows do not already carry.

No other figure meets the video bar. One video candidate flagged.
