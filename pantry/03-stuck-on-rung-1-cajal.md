# CAJAL Figure Report — Chapter 03: Stuck on Rung 1
_Mode: /scan silent · 2026-06-16 · source: chapters/03-stuck-on-rung-1.md_

## Density recommendation
The chapter's conceptual core is Pearl's three-rung taxonomy and the do-operator's arrow-severing mechanism — both inherently diagrammatic. There is one chartable quantity worth honoring (the three-practice classification, and the soft "+25% campaign engagement" / "100M+ field suggestions" figures, both `[verify]`-flagged and NOT to be rendered as bars — they are anecdotal, not a distribution). Favor mechanism/verification figures. Target 4 figures: 2 Critical, 1 Important, 1 Supplementary. One video candidate (the do-operator severing arrows).

## Figures

---

FIGURE 1 — Pearl's Ladder of Causation, three sealed rungs · Priority: Critical · Type: vertical ladder / hierarchy · Heuristic: VG

BLOCK 1 — ILLUSTRAE PASTE BLOCK:
Render a vertical three-rung ladder establishing a strict hierarchy of question types. Draw two upright rails crossed by three horizontal rungs evenly stacked. Assign the bottom rung to association (seeing), the middle rung to intervention (doing), and the top rung to counterfactual (imagining), with the bottom visually the broadest/foundational and each higher rung marked as strictly more powerful. Along the left rail, between the bottom and middle rungs, draw a distinct horizontal barrier symbol — a sealing band — indicating that data resting on the lowest rung cannot, by itself, reach the rung above it. Along the right side, attach one small marker per rung representing a pharma example: an engagement-proxy marker at the bottom, a deliberate-message-assignment marker in the middle, and an individual-counterfactual marker at the top. Keep the three rungs equal in spacing, the sealing band the most salient single element, white background, flat vector shapes, single stroke weight, no decorative ladder texture. No rung-name text, no equations, no captions.

BLOCK 2 — FULL SCOPE:
[S]pecification: single-column 89mm, 300 DPI, vector, textbook.
[C]ontent: chapter-confirmed — Rung 1 association P(Y|X) "seeing"; Rung 2 intervention P(Y|do(X)) "doing"; Rung 3 counterfactual "imagining"; each strictly more powerful; the rungs are SEALED — Rung-1 data alone cannot answer Rung-2 questions (depict the seal between rung 1 and 2). Pharma examples per rung: email open rate (R1), deliberate message assignment→NRx (R2), for-this-physician which message would have changed prescribing (R3).
[O]rganization: vertical ladder, R1 bottom → R3 top (ascending power); prominent sealing band between R1 and R2 on the left; three example markers on the right, one per rung.
[P]resentation: flat vector, white bg, Okabe-Ito — R1 #56B4E9 (primary/foundational anchor), R2 #0072B2 (dominant — the target rung), R3 #CC79A7 (transitional/highest), sealing band #D55E00 (the blocking boundary), rails/rungs #000000 1pt.
[E]xclusions: no typeset P(Y|X) or do() equations, no rung-name words, no human climber figure, no owl illustration, no realistic wood/metal texture, no fourth rung, no upward arrow implying easy ascent across the seal.
[E] — highest leverage: the seal between R1 and R2 must read as impassable-by-data, not as a step; do not draw an arrow crossing it.

BLOCK 3 — NEGATIVE PROMPT:
typeset equations, rung-name words, owl illustration, human climber, wood texture, metal texture, fourth rung, arrow crossing the seal, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures (unless explicitly requested), visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

FIGURE 2 — The do-operator severs incoming arrows · Priority: Critical · Type: paired before/after causal graphs · Heuristic: MC

BLOCK 1 — ILLUSTRAE PASTE BLOCK:
Render two small causal graphs side by side comparing observation to intervention. In the left graph, draw two upstream source nodes — one for physician receptivity, one for rep strategy — each with an arrow pointing into a central node representing the message received, and a further arrow from the message node into a downstream node representing prescribing. This left graph shows the confounded observational structure with both upstream arrows intact. In the right graph, reproduce the same four nodes in the same positions, but cut the two arrows that ran from the upstream sources into the message node — render those two arrows as clearly severed or crossed out — leaving only the single arrow from message to prescribing intact. The contrast between the two graphs is the entire point: intervention removes the incoming arrows that observation leaves in place. Keep the two graphs identical in node layout so the only visible difference is the severed arrows, white background, flat vector nodes, single stroke weight, standard single-headed arrows, and a distinct severing mark on the two cut edges in the right graph.

BLOCK 2 — FULL SCOPE:
[S]pecification: single-column 89mm, 300 DPI, vector, textbook.
[C]ontent: chapter-confirmed — left (observational): physician receptivity → message received, rep strategy → message received, message → NRx; confounding paths visible. Right (interventional, do(message)): incoming arrows to message severed, only message → NRx remains. The do-operator both replaces message's equation with a constant and severs all incoming arrows. Carry caveat: this severance is what randomization does physically and a causal model does mathematically — depict the structure, not a proof of identifiability.
[O]rganization: two side-by-side panels, identical node geometry; left = intact edges; right = ⊣ two incoming edges cut; left-to-right reads observation → intervention.
[P]resentation: flat vector, white bg, Okabe-Ito — message node #0072B2 (dominant/intervened), receptivity & rep-strategy nodes #E69F00 (secondary confounders), NRx node #009E73 (outcome/positive), intact arrows #000000 1pt single-headed, severed arrows marked with #D55E00 cut strokes.
[E]xclusions: no node-name text, no do() notation typeset, no probability expressions, no scissors clipart, no extra confounders, no bidirected/dual-headed arrows, no third graph, no curved confounding back-door arc (keep the explicit fork structure).
[E] — highest leverage: the two graphs must be geometrically identical except for the cut edges; the severing mark must be unambiguous and applied only to the two incoming edges, never to message→NRx.

BLOCK 3 — NEGATIVE PROMPT:
node-name text, typeset do() notation, probability expressions, scissors clipart, extra confounder nodes, bidirected arrows, third graph, curved back-door arc, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures (unless explicitly requested), visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

FIGURE 3 — Three deployed pharma practices, all stuck on Rung 1 · Priority: Important · Type: classification matrix (practice × query level) · Heuristic: VG

BLOCK 1 — ILLUSTRAE PASTE BLOCK:
Render a three-row classification matrix comparing what pharma analytics practices actually answer against what the budget actually needs. Create three rows, one per practice: content optimization, rep performance management, and next-best-action engines. Give the matrix three columns: the stated optimization target, the actual query the practice computes, and the Rung-2 question it cannot answer. Visually mark all three middle-column cells as belonging to the same lowest tier — a uniform Rung-1 band running down the matrix — to show that every practice, despite differing surfaces, sits on the same rung. Mark the right-column cells as belonging to the higher unmet tier, in a distinct color, to show the common gap. The visual message is a shared floor: three different practices, one identical rung, one identical unanswered question type above them. Keep rows aligned and equal height, white background, flat vector cells, single stroke weight, no arrows. No cell text, no field names.

BLOCK 2 — FULL SCOPE:
[S]pecification: single-column 89mm, 300 DPI, vector, textbook.
[C]ontent: chapter-confirmed — three practices: content optimization (target: dwell/reaction; computes P(engagement|message,physician)); rep performance mgmt (target: call attainment vs quota; computes P(high conversion|rep territory)); NBA engine (target: open rate/CTR/visit acceptance; computes P(open|physician,message)). All three = Rung 1. The unmet Rung-2 questions: P(NRx|do(message)), P(NRx|do(rep assigned)), P(NRx|do(message=X) vs do(message=Y)). Carry caveat: "no major vendor publicly claims to estimate the do-expression" is a sourced inference from public self-descriptions, not proof about proprietary internals — the figure depicts the public-pattern claim only [inferred re: internals].
[O]rganization: 3 rows × 3 columns; uniform Rung-1 band down the middle column; distinct unmet-tier color down the right column; rows equal height.
[P]resentation: flat vector, white bg, Okabe-Ito — practice/left column light gray (neutral), middle Rung-1 band #56B4E9 (the shared actual rung), right unmet-Rung-2 column #D55E00 (the gap); grid lines #000000 0.5pt.
[E]xclusions: no vendor names (Aktana/IQVIA/Indegene), no typeset probability expressions, no percentage figures (+25%, 100M), no arrows, no checkmarks, no logos, no more than 3 rows.
[E] — highest leverage: do NOT name vendors and do NOT render the unverified engagement-uplift percentages; the figure is about rung structure, not vendor scorekeeping.

BLOCK 3 — NEGATIVE PROMPT:
vendor names, vendor logos, percentage figures, typeset probability expressions, arrows, checkmarks, more than three rows, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures (unless explicitly requested), visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

FIGURE 4 — Prediction ≠ intervention: high predicted propensity vs. incremental value · Priority: Supplementary · Type: 2×2 conceptual quadrant · Heuristic: VG

BLOCK 1 — ILLUSTRAE PASTE BLOCK:
Render a two-by-two quadrant that separates predicted prescribing propensity from incremental effect of the message. Set the horizontal axis as predicted likelihood of prescribing, from low on the left to high on the right, and the vertical axis as how much the message actually changes prescribing, from low at the bottom to high at the top. This yields four quadrants. Emphasize two of them in tension: the lower-right quadrant, high predicted propensity but near-zero incremental effect, is where a prediction-and-personalization engine concentrates its spend — mark it as the misdirected-budget region. The upper region, where incremental effect is genuinely high, is the region actually worth spending on regardless of predicted propensity — mark it as the value region. The visual lesson is that the engine optimizes along the horizontal axis while the budget should care about the vertical axis, and the two come apart. Keep axes clean with directional arrows but no numeric ticks, white background, flat vector quadrants distinguished by fill, single stroke weight. No quadrant text, no data points, no captions.

BLOCK 2 — FULL SCOPE:
[S]pecification: single-column 89mm, 300 DPI, vector, textbook.
[C]ontent: chapter-confirmed — a high predicted-propensity physician may prescribe regardless (zero incremental value); the better the predictor, the more confidently it points to people least worth spending on; prediction (Rung 1) and intervention/incremental value (Rung 2) are different axes that diverge. Lower-right = where prediction+personalization spends (misdirected); high-incremental band = where budget should go. Mark this as a conceptual schematic, no real data.
[O]rganization: x-axis = predicted propensity (low→high), y-axis = incremental effect (low→high); four quadrants; highlight lower-right (misdirected spend) and the high-incremental band (true value); axis arrows only, no ticks.
[P]resentation: flat vector, white bg, Okabe-Ito — misdirected-spend quadrant (high propensity / low incremental) #D55E00, true-value region (high incremental) #009E73, remaining quadrants light gray (neutral); axes #000000 1pt single-headed, no tick marks.
[E]xclusions: no numeric axis ticks, no scatter points, no quadrant labels, no curve/line fit, no physician icons, no percentages, no third axis.
[E] — highest leverage: no data points — this is conceptual; and the two emphasized regions must read as on different axes (one horizontal-defined, one vertical-defined) to make "different questions" visible.

BLOCK 3 — NEGATIVE PROMPT:
numeric axis ticks, scatter points, quadrant labels, fitted curve, physician icons, percentages, third axis, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures (unless explicitly requested), visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

## Video candidate pass

VIDEO CANDIDATE — Figure 2 (do-operator severs incoming arrows)
- Status: Recommended
- Criterion met: sub-observable transformation — the learning target is the operation itself, the do-operator structurally cutting incoming arrows to convert an observational graph into an interventional one. The transition IS the concept.
- Reason: Animating the two confounding arrows being severed (and the observed conditional collapsing to the clean message→NRx path) makes "seeing is not doing" kinetic; the moment of severance is precisely what randomization performs physically and what the static side-by-side can only assert. This is the chapter's central mechanism and recurs as the engine of the whole book.
- Suggested format: 8–12s silent transform: start on intact observational graph, two incoming edges cut on a beat, residual single causal edge highlighted; no narration text required.

Note — Figure 1 (the ladder) is a strong secondary candidate (the "sealed boundary" could animate a Rung-1 data blob failing to cross), but to hold ~1 video/chapter, prefer Figure 2; the ladder reads fully as a static hierarchy. Figures 3 and 4 are static-appropriate.
