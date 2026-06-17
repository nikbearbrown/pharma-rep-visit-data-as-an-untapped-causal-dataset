# CAJAL Figure Report — Chapter 10: Staggered Difference-in-Differences on Message-Variant Transitions
_Mode: /scan silent · 2026-06-16 · source: chapters/10-staggered-did-message-variants.md_

## Density recommendation

This is the flagship project chapter, and its pedagogy is built on three load-bearing visual moments the prose explicitly gestures at with inline `[DIAGRAM]`/`[TABLE]` markers: the Goodman-Bacon decomposition (why TWFE breaks), the estimator-choice table, and the two-estimator event-study overlay (the punchline). The chapter is conceptually dense but reuses one core mechanism (the 2×2 building block) repeatedly, so it does not need a figure per section. Recommended density: **5 figures** — 3 Critical (the chapter's own marked exhibits), 1 Important (the falsification-first sequence, which is the chapter's stated "discipline" and currently carries no visual), 1 Supplementary (the ATT(g,t) → aggregation grid). This matches the chapter's rhythm: one figure to show the bug, one to choose the fix, one to prove the fix recovered truth, plus the discipline scaffold. Resist adding more — the parallel-trends and estimand concepts are well-served by prose and would produce thin, text-dependent figures that violate the no-baked-text rule.

## Figures

---

FIGURE 1 — Goodman-Bacon decomposition of TWFE into 2×2 comparisons · Critical · Process/comparison diagram · MC + VG

BLOCK 1 ILLUSTRAE PASTE:
Render a flat vector diagram that decomposes a single staggered two-way-fixed-effects estimate into its three families of underlying two-by-two difference-in-differences comparisons. Lay out three horizontal comparison panels stacked vertically inside one frame. The top panel shows a newly-treated cohort compared against a not-yet-treated cohort; mark this comparison as valid, carrying positive weight, using the active green. The middle panel shows an earlier-treated cohort compared against a later-treated cohort during the pre-treatment window of the later cohort; mark valid, positive weight, green. The bottom panel shows a later-treated cohort compared against an already-treated earlier cohort; mark this the forbidden comparison, using the blocking vermillion, with a small blocked terminator on the path from the already-treated control. Below the three panels, draw a summation node where the three weighted panels converge into one combined estimate box, the combined box rendered in composite reddish-purple to signal a contaminated mixture. Use timeline bands per cohort showing pre-period and post-period as distinct horizontal segments. Keep all paths orthogonal, single-weight one-point strokes, white background, no baked text.

BLOCK 2 FULL SCOPE:
- [S] Single-column 89mm, 300 DPI, vector, textbook.
- [C] Three comparison types from Goodman-Bacon (2021) as named in chapter: (1) newly-treated vs not-yet-treated [valid, +weight]; (2) earlier vs later treated during later's pre-window [valid, +weight]; (3) later vs already-treated earlier [forbidden, may carry negative weight]; weights sum to the TWFE coefficient. Cohort timelines with pre/post segments [inferred as visual device; the comparison families are chapter-confirmed].
- [O] Three stacked horizontal comparison panels → converging summation node → single combined-estimate box. Treatment-onset markers per cohort. Blocked terminator (⊣) on the forbidden control path.
- [P] Flat vector, white bg, Okabe-Ito: active/valid #009E73, blocking/forbidden #D55E00, composite/contaminated total #CC79A7, anchor timelines #56B4E9, neutral gray for inactive segments, 1pt strokes.
- [E] Exclude: numeric weight values, any equation, axis numbers, the word "negative," real prescribing curves, more than three comparison panels, any fourth estimator. Do not draw the magnitude of bias as a quantified bar — this figure is structural, not quantitative.

BLOCK 3 NEGATIVE PROMPT:
numeric weights, equations, axis tick numbers, fourth comparison panel, regression output tables, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

FIGURE 2 — Estimator selection table (four estimators side by side) · Critical · Comparison matrix · MC

BLOCK 1 ILLUSTRAE PASTE:
Render a flat vector comparison matrix laying four staggered-DiD estimators across columns and their distinguishing properties down rows. Reserve the leftmost column as a labeled row stub for property categories: intended use, key assumption, handling of non-absorbing (turn-on-and-off) treatment, and role as diagnostic versus estimation strategy. Across the four data columns place the Callaway–Sant'Anna estimator, the Sun–Abraham interaction-weighted estimator, the de Chaisemartin–D'Haultfœuille multiple-groups family, and the Goodman-Bacon decomposition. Visually distinguish the single diagnostic column (Goodman-Bacon) from the three estimation-strategy columns by tinting its header band a neutral gray while the three estimation strategies carry the dominant blue header band. Use filled and open cell glyphs — a solid mark for "handles non-absorbing treatment," an open or blocked mark for "does not" — rather than written words, with the blocking vermillion reserved for the blocked capability cells. Keep gridlines thin, one-point, evenly aligned, white background, generous cell padding, no baked text inside cells.

BLOCK 2 FULL SCOPE:
- [S] Single-column 89mm (may flag for full-width if four columns crowd), 300 DPI, vector, textbook.
- [C] Four estimators and their roles, all chapter-confirmed: Callaway–Sant'Anna (workhorse, doubly-robust, absorbing); Sun–Abraham (interaction-weighted event study); de Chaisemartin–D'Haultfœuille (non-absorbing/turn-on-and-off); Goodman-Bacon (diagnostic only). Property rows: use case, key assumption, handles non-absorbing treatment, diagnostic-vs-estimation [rows confirmed; R-package row from chapter optional, excluded to avoid baked text].
- [O] 4 estimator columns × 4 property rows + row-stub column. Header band tint distinguishes diagnostic (gray) from estimation (blue). Capability cells use solid/blocked glyphs.
- [P] Flat vector, white bg, Okabe-Ito: estimation-strategy headers #0072B2 (dominant), diagnostic header neutral gray, capability-present #009E73 solid glyph, capability-absent #D55E00 blocked glyph, 1pt gridlines.
- [E] Exclude: any cell prose, R package names as text, author publication years, more than four estimators, checkmark-style hand-drawn ticks. Do not encode "key assumption" as text — use this figure only for the binary/categorical glyph cells; assumption text stays in prose.

BLOCK 3 NEGATIVE PROMPT:
cell prose, package names, publication years, fifth estimator column, hand-drawn checkmarks, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

FIGURE 3 — Two-estimator event-study overlay (TWFE vs Callaway–Sant'Anna) · Critical · Event-study plot · VG + PQ

BLOCK 1 ILLUSTRAE PASTE:
Render a flat vector event-study coefficient plot with relative event time on the horizontal axis, spanning several pre-treatment periods left of a vertical zero reference line and several post-treatment periods right of it. Plot two coefficient paths. The first path, drawn as a dashed dominant-blue line with point markers, is the naive two-way-fixed-effects estimator: show its pre-treatment leads sloping spuriously away from zero and its early post-treatment lags pulled artificially below the zero baseline before partly recovering. The second path, drawn as a solid active-green line with point markers, is the Callaway–Sant'Anna clean estimator: show its pre-treatment leads sitting flat along zero and its post-treatment lags rising in a positive, growing staircase. Add a horizontal neutral-gray reference line marking the known synthetic ground-truth effect level. Draw a vertical anchor line at relative time zero. Include thin pointwise confidence whiskers on each marker. Horizontal axis crosses at the coefficient zero line so sign is unambiguous; do not start the vertical axis arbitrarily — keep zero visible and centered.

BLOCK 2 FULL SCOPE:
- [S] Single-column 89mm, 300 DPI, vector, textbook.
- [C] Two estimator paths over relative event time, chapter-confirmed: TWFE (dashed) shows spurious pre-trend leads + artificially negative early lags; Callaway–Sant'Anna (solid) shows flat leads + positive growing lags recovering planted ground truth. Vertical line at relative time 0; horizontal ground-truth reference line. Pointwise CIs [magnitudes inferred — chapter marks effect values "to be generated on synthetic data"; render shape only, mark "[inferred]" magnitudes].
- [O] x = relative event time (pre left, post right); y = ATT coefficient with zero line visible. Two overlaid paths (dashed TWFE, solid CS) + point markers + CI whiskers. Vertical onset line at 0. Horizontal ground-truth reference.
- [P] Flat vector, white bg, Okabe-Ito: TWFE path #0072B2 dashed, CS path #009E73 solid, ground-truth reference neutral gray, onset line #56B4E9 (anchor), 1pt strokes, thin whiskers.
- [E] Exclude: numeric coefficient values on axis (magnitudes unconfirmed — shape only), real NPI curves, more than two estimator paths, shaded CI ribbons (use whiskers not bands to keep flat), legend text. Mark all magnitudes "[inferred]" — do not imply a specific effect size the chapter has not generated.

BLOCK 3 NEGATIVE PROMPT:
numeric coefficient values, third estimator path, shaded CI ribbon bands, legend text, regression tables, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

FIGURE 4 — Falsification-first discipline sequence · Important · Process/decision flow · MC

BLOCK 1 ILLUSTRAE PASTE:
Render a flat vector vertical decision-flow showing the chapter's ordered falsification discipline that must run before any treatment effect is believed. Draw four sequential stage nodes connected top to bottom by single-weight orthogonal arrows: Test 1 independence (regress cohort assignment on pre-rollout prescribing and growth), Test 2 pre-trend leads (clean-estimator event-study leads near zero), Test 3 honest sensitivity (bound plausible post-period violation against observed pre-period violation), and a final estimate-and-readout node. From each of the first three test nodes, branch a side path terminated with a blocking vermillion stop glyph labeled by position as the kill outcome — "design dead, report the failure." The pass path continues downward in active green to the next test. The terminal estimate node, reached only after all three pass, is rendered in dominant blue. Place an exclusion-control side annotation (rep tenure, baseline performance) feeding the estimate node as a neutral-gray input. Keep the main spine vertical and straight, branches orthogonal, white background, no baked text.

BLOCK 2 FULL SCOPE:
- [S] Single-column 89mm, 300 DPI, vector, textbook.
- [C] Three ordered falsification tests + estimate stage, chapter-confirmed: Test 1 independence (cohort-on-pre-prescribing); Test 2 pre-trend leads; Test 3 honest sensitivity (Rambachan–Roth bounds); then estimate/readout. Each test has a kill branch ("stop, report failure"). Exclusion controls (rep tenure, baseline rep performance) feed the estimate. Sequence ordering is load-bearing and chapter-stated.
- [O] Vertical spine: Test1 → Test2 → Test3 → Estimate. Each test forks: pass (green, down) / kill (vermillion, ⊣ stop glyph to the side). Exclusion-control input arrow into estimate node (gray).
- [P] Flat vector, white bg, Okabe-Ito: pass paths #009E73, kill terminators #D55E00, estimate node #0072B2, control input neutral gray, 1pt strokes.
- [E] Exclude: more than four spine stages, equations, p-value thresholds as numbers, the Honest-DiD M-values as numbers, citation years. Do not merge the three tests into one box — the ordering is the teaching point. Keep ≤6 visible nodes plus branches.

BLOCK 3 NEGATIVE PROMPT:
p-value numbers, M-bar values, equations, citation years, fifth spine stage, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

FIGURE 5 — Group-time ATT(g,t) grid and aggregation · Supplementary · Matrix-to-summary diagram · MC + VG

BLOCK 1 ILLUSTRAE PASTE:
Render a flat vector diagram showing how Callaway–Sant'Anna group-time average treatment effects are built as a grid and then aggregated into summaries. On the left, draw a matrix whose rows are treatment cohorts (groups first treated at successive times g) and whose columns are calendar time periods t; shade the lower-left pre-treatment cells of each row in neutral gray (leads, expected near zero) and the post-treatment cells from each cohort's onset rightward in active green with increasing saturation to suggest growing effect. Mark each cohort's treatment-onset cell with an anchor-blue boundary so the staircase of onsets is visible across rows. On the right, draw two aggregation outputs fed by orthogonal arrows from the grid: a dynamic event-study summary (a small staircase line by exposure length) and an overall treatment-on-the-treated summary (a single dot with a confidence whisker). Each grid cell is a clean square building block, never reusing already-treated cells as controls. Keep alignment exact, single-weight strokes, white background, no baked text.

BLOCK 2 FULL SCOPE:
- [S] Single-column 89mm, 300 DPI, vector, textbook.
- [C] ATT(g,t) grid: rows = cohorts by onset g, columns = time t; pre-onset cells = leads (near zero), post-onset cells = effects; staggered onset staircase; aggregation to dynamic event study + overall ToT, all chapter-confirmed. Increasing post-onset saturation = growing effect [inferred visual encoding of chapter's "small at 30, larger at 60/90"].
- [O] Left: cohort × time grid with onset-boundary staircase, pre cells gray, post cells green. Right: two aggregation outputs (dynamic staircase, single ToT dot+whisker) via arrows from grid.
- [P] Flat vector, white bg, Okabe-Ito: post-onset effect cells #009E73 (saturation gradient by step, not continuous fill), pre/lead cells neutral gray, onset boundary #56B4E9 (anchor), aggregation outputs #0072B2 (dominant), 1pt strokes.
- [E] Exclude: numeric ATT values in cells, continuous heatmap gradient (use stepped saturation tiers, not a rainbow ramp), more than ~4 cohort rows, equation for aggregation weights. Do not render as a photographic heatmap.

BLOCK 3 NEGATIVE PROMPT:
numeric cell values, continuous heatmap gradient, rainbow color scales, aggregation equations, more than four cohort rows, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, 3D perspective distortion

---

## Video candidate pass

**FIGURE 1 — Goodman-Bacon decomposition**
- Status: RECOMMENDED (primary candidate, ~1/chapter)
- Criterion: ≥3 sequential causal stages AND sub-observable transformation — the transition mechanism (how a forbidden already-treated-as-control comparison enters with a negative weight and drags a positive total negative) is itself the learning target.
- Reason: The bias is invisible in static form because the *temporal* overlap of one cohort's still-growing post-period with another cohort's comparison window is what creates the sign flip. A short animation can sweep the comparison window across the staggered timeline and show the weight changing sign as the already-treated control's effect keeps rising — the single hardest intuition in the chapter, and the one the opening case turns on. Static Figure 1 carries the structure; a video would carry the *mechanism*.
- Suggested format: 20–30s silent vector animation: (1) show three cohorts adopting on a timeline; (2) sweep a comparison highlight, accumulating valid green 2×2s; (3) reach the forbidden comparison, show the already-treated control still climbing, flip its contribution to vermillion negative; (4) collapse all contributions into the contaminated composite total that lands below zero. No narration text; rely on color-state transitions.

**FIGURE 3 — Two-estimator event-study overlay**
- Status: NOT recommended (build static only)
- Criterion: fails — no transition mechanism is the learning target; the comparison is between two final states, fully legible side by side in one static frame.
- Reason: A before/after or two-line static plot already delivers the punchline. Animation would add motion without adding understanding.

No other figure meets the video bar. One video candidate flagged.
