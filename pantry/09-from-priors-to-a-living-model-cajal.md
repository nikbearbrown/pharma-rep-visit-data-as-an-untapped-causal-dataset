# CAJAL Figure Report — Chapter 09: From Priors to a Living Model
_Mode: /scan silent · 2026-06-16 · source: chapters/09-from-priors-to-a-living-model.md_

## Density recommendation

For this text, I recommend **5 figures using Mechanistic density.** This is the synthesis chapter and the most diagram-dependent of the three: it assembles three previously separate inputs into a structural causal model, runs a three-step counterfactual engine on it, and wraps both in a drift monitor. Four MC concepts carry the chapter — the three-ingredient assembly (structure + magnitude + history → SCM), the abduction→action→prediction procedure (the heart of the chapter, an essential sequential mechanism), the three granularities of inference (population ACE / subgroup CATE / individual counterfactual), and the Living Model architecture (SCM → engine → drift monitor with feedback). A fifth figure carries the edge-provenance scheme (data/rep/literature/assumed and what a breach triggers), a VG claim the prose asserts as the model's auditable underbelly. The chapter places five author diagram/table comments; CAJAL confirms all five and assigns types. No standalone PQ figure: the chapter's numbers (+4 prescriptions, α/β, 10-percentage-point threshold) are illustrative parameters embedded in the worked SCM, not a dataset to chart — forcing a bar chart would fabricate a distribution. The honesty caveat governs Figures 2 and 4: the counterfactual is abduction→action→prediction and can be only *partially identified* (bounds, not points); the drift monitor's signature target is opt-out-rate shift driving selection-collider bias.

## Figures

---

FIGURE 1 — Three-ingredient assembly into the SCM · Critical · Systems diagram · MC

**BLOCK 1 — ILLUSTRAE PASTE**

Build a convergence-and-fan-out systems diagram. On the left, stack three uniform input boxes representing the three ingredients: a top box (elicited structure), a middle box (identified magnitudes), and a bottom box (physician history). Each sends one horizontal arrow rightward into a single large central box representing the assembled model. From that central box, fan two arrows out to the right into two output boxes stacked vertically: an upper output (the counterfactual engine) and a lower output (the drift monitor). Keep the three input boxes identical in size and the central box visibly larger to read as the meeting point. Use single-weight arrows with standard arrowheads, flat vector rectangles with squared corners, white background, generous spacing, clean left-to-center-to-right reading order with no crossing lines. Leave every box unlabeled for later typesetting of ingredient names, the SCM label, and the two output names.

**BLOCK 2 — FULL SCOPE**

- **[S]** Single-column, 89mm width, 300 DPI, vector, textbook default.
- **[C]** Chapter-confirmed assembly: three inputs — Structure (elicited prior DAG: oriented edges, confidence tags), Magnitude (identified quasi-experimental effects: IV/DiD/forest estimates), History (physician-by-time panel: visits, NRx, reactions) — converge into a Structural Causal Model (DAG + structural equations + provenance tags), which feeds two outputs: the Counterfactual Engine and the Drift Monitor.
- **[O]** Left-to-center convergence (three inputs → SCM), then center-to-right fan-out (SCM → engine, SCM → monitor). → for contribution and output flow. Central SCM box enlarged.
- **[P]** Flat vector, white, 1pt strokes. Three input boxes secondary Orange #E69F00; central SCM box composite Reddish Purple #CC79A7 (it is the composite object); two output boxes Sky Blue #56B4E9; arrows black #000000. No baked text.
- **[E]** Exclude: ingredient/output names and the structural-equation formula as baked text; chapter cross-references (Ch 8, 4, 10, 11, 2); the abduction-action-prediction internals (Figure 2); the provenance table; the drift-monitor sub-jobs; any depiction of the engine's three steps inside this figure.

**BLOCK 3 — NEGATIVE PROMPT**

ingredient-name text, output-name text, equation text, chapter cross-references, engine internals, provenance table, monitor sub-jobs, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

FIGURE 2 — Abduction → Action → Prediction · Critical · Mechanism cross-section · MC

**BLOCK 1 — ILLUSTRAE PASTE**

Draw three horizontal panels left to right, each showing the same small three-node causal graph in a different state of the counterfactual procedure. Panel one (abduction): the graph with its observed nodes highlighted and a curved inference arrow pointing back from the observed nodes toward two small noise-term symbols attached to two of the nodes, depicting recovery of the unit's specific noise from evidence. Panel two (action): the same graph, but the top input node's incoming mechanism is replaced — draw a distinct intervention box sitting on that one node, severing its usual upstream arrow, while every other edge in the graph stays intact. Panel three (prediction): the same graph running forward in the normal arrow direction, now carrying the two recovered noise symbols through to a final output node, with a short ranked list shape beside it indicating ranked candidate outputs. Keep the graph identical across panels so the reader tracks one model through three operations. Use flat vector circles, single-weight arrows, white background, equal panel gutters. Leave unlabeled for later typesetting of step names, node names, and noise symbols.

**BLOCK 2 — FULL SCOPE**

- **[S]** Single-column, 89mm width, 300 DPI, vector, textbook default.
- **[C]** Pearl's chapter-confirmed three-step counterfactual procedure on one physician: Step 1 Abduction (recover exogenous noise U from observed evidence); Step 2 Action (apply do(message=m'), replacing only the message equation, leaving all other mechanisms intact); Step 3 Prediction (run modified model forward carrying recovered U, rank candidate messages). Same SCM across all three panels. Partial-identification caveat reflected by keeping the prediction output as a ranking/range shape, not a single point [inferred visual choice — chapter states output is a ranking and may be bounded].
- **[O]** Three side-by-side panels, shared graph; Panel 1 back-inference arrow to noise terms; Panel 2 intervention box severing one upstream edge, others intact; Panel 3 forward run carrying noise to ranked output. → for forward mechanism, do-intervention shown as equation replacement.
- **[P]** Flat vector, white, 1pt strokes. Observed/evidence nodes in Panel 1 Sky Blue #56B4E9; recovered noise symbols composite Reddish Purple #CC79A7; intervention box in Panel 2 active Bluish Green #009E73 with the severed edge stub neutral gray #999999; forward arrows and final output Blue #0072B2; arrows black #000000. No baked text.
- **[E]** Exclude: step names, node names, the structural-equation formulas, "do(...)" notation as baked text; the population/subgroup/individual granularity ladder (Figure 3); the weather-model analogy; the formulary-blocked worked numbers; the three-ingredient assembly; the drift monitor.

**BLOCK 3 — NEGATIVE PROMPT**

step-name text, node-name text, equation text, do-operator notation, granularity ladder, analogy imagery, worked numbers, assembly diagram, drift monitor, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

FIGURE 3 — Three granularities of causal inference · Important · Hierarchy / progression · VG + MC

**BLOCK 1 — ILLUSTRAE PASTE**

Draw a three-step resolution staircase ascending left to right, each step narrowing from a broad population to a single unit. Step one (population): a wide block containing many identical small dots grouped as one undivided mass, representing the average effect over everyone. Step two (subgroup): a block of the same width but partitioned into a few distinct cells, each cell holding a cluster of dots, representing effects estimated within subgroups. Step three (individual): a single isolated dot, set apart and enlarged, representing one specific unit whose own state is read. Connect the three steps with two rightward arrows showing increasing resolution, and let each step sit slightly higher than the last so the figure reads as an ascent in specificity. Keep dot size uniform within and across steps, cell divisions clean and equal, flat vector style, single stroke weight, white background, ample spacing. Leave unlabeled for later typesetting of the three level names and the rung annotations.

**BLOCK 2 — FULL SCOPE**

- **[S]** Single-column, 89mm width, 300 DPI, vector, textbook default.
- **[C]** Three chapter-confirmed granularities: Population ACE (average causal effect, no abduction, Rung-2); Subgroup CATE (conditional average treatment effect, splits by observed characteristics, Rung-2 with resolution); Individual counterfactual (conditions on one physician's history, recovers her noise, Rung-3). Each step is strictly more resolved than the last.
- **[O]** Left-to-right ascending staircase: undivided population mass → partitioned subgroup cells → single isolated unit; two increasing-resolution arrows. → for increasing specificity.
- **[P]** Flat vector, white, 1pt strokes. Population mass neutral gray #999999; subgroup cells secondary Orange #E69F00; single individual dot dominant Blue #0072B2 (the chapter's payoff object); dividers and arrows black #000000. No baked text.
- **[E]** Exclude: level names and rung labels as baked text; effect-size numbers (the +4); the abduction-action-prediction panels; the NBA-engine comparison; specialty/region example text; any continuous gradient between steps (keep discrete).

**BLOCK 3 — NEGATIVE PROMPT**

level-name text, rung labels, effect-size numbers, procedure panels, NBA comparison, example text, continuous gradient, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

FIGURE 4 — Living Model architecture (SCM · engine · drift monitor) · Critical · Systems diagram · MC

**BLOCK 1 — ILLUSTRAE PASTE**

Build a horizontal three-component architecture with a feedback return path. Place three boxes in a row: a left box (the parameterized model), a center box (the counterfactual engine), and a right box (the drift monitor). Draw a forward arrow from the left box to the center box, showing the model feeding its equations to the engine. From the right monitor box, draw a return arrow looping back to the left model box, showing that detected drift triggers model updates. Inside the right monitor box, show three small stacked sub-elements as distinct horizontal bands representing its three watch-jobs, with the bottom band carrying a small alarm marker to single out the signature alarm. Keep the three main boxes uniform in height, the forward and return arrows clearly distinct in their paths so the loop is unambiguous, flat vector style, single stroke weight, white background. Leave all boxes and bands unlabeled for later typesetting of component names and the three monitor jobs.

**BLOCK 2 — FULL SCOPE**

- **[S]** Single-column, 89mm width, 300 DPI, vector, textbook default.
- **[C]** Chapter-confirmed Living Model architecture: (1) Parameterized SCM (DAG + structural equations + provenance tags) → feeds (2) Counterfactual Engine (abduction→action→prediction, individual Rung-3 recommendations); (3) Drift Monitor with three jobs — parameter drift (→ refit), structural drift (→ re-elicit / FCI re-spec), opt-out-rate alarm (→ selection-structure review, the signature alarm). Monitor returns a trigger to the SCM.
- **[O]** Horizontal chain SCM → engine; monitor on the right with a return/feedback arrow to the SCM; three stacked bands inside the monitor, the opt-out band flagged. → for forward feed, return arrow for the update trigger.
- **[P]** Flat vector, white, 1pt strokes. SCM box composite Reddish Purple #CC79A7; engine box Sky Blue #56B4E9; monitor box dominant Blue #0072B2; the opt-out alarm band/marker blocking Vermillion #D55E00; forward arrows and return arrow black #000000. No baked text.
- **[E]** Exclude: component names and the three monitor-job labels as baked text; the abduction-action-prediction internals (Figure 2); the consent-collider graph; the 10-percentage-point threshold number; the educational-pathway compliance constraint; the assembly inputs (Figure 1).

**BLOCK 3 — NEGATIVE PROMPT**

component-name text, monitor-job labels, engine internals, consent-collider graph, threshold numbers, compliance text, assembly inputs, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

FIGURE 5 — Edge provenance scheme · Supplementary · Annotated example · VG

**BLOCK 1 — ILLUSTRAE PASTE**

Create a four-row provenance key, each row a uniform horizontal band showing one kind of edge by how it is sourced. Each band contains, on the left, one short edge sample — two nodes joined by a directed arrow — drawn in a distinct fill so the four rows are visually separable; and on the right, a small marker region reserved for what a breach of that edge triggers. Order the four bands top to bottom from most grounded to least grounded: a data-estimated band, a rep-elicited band, a literature-supported band, and an assumed band at the bottom. Render the bottom assumed band as the most visually exposed — the lightest fill or an open outline — to mark it as the model's weakest, most challengeable layer. Keep band heights equal, edge samples identical in geometry across rows so only color distinguishes them, flat vector style, single stroke weight, white background. Leave every band and marker region unlabeled for later typesetting of provenance-type names and breach-response text.

**BLOCK 2 — FULL SCOPE**

- **[S]** Single-column, 89mm width, 300 DPI, vector, textbook default.
- **[C]** Four chapter-confirmed edge provenance types, ordered grounded→exposed: data-estimated (identified quasi-experimental effect), rep-elicited (named rep observation at a confidence level), literature-supported (published causal estimate), assumed (no source — the exposed underbelly, gets sensitivity sweep / partner review). Each has an associated breach response.
- **[O]** Four stacked uniform bands top-to-bottom; left side one edge sample per band; right side a reserved breach-response marker; assumed band rendered as most visually exposed. → for the edge samples.
- **[P]** Flat vector, white, 1pt strokes. data-estimated Blue #0072B2; rep-elicited Bluish Green #009E73; literature-supported Sky Blue #56B4E9; assumed band open outline / light gray #CCCCCC to mark exposure. Arrows black #000000. No baked text.
- **[E]** Exclude: provenance-type names and breach-response text as baked labels; the full SCM graph; the sensitivity-sweep numbers; the abduction engine; the drift monitor; confidence-interval values; any continuous spectrum (keep four discrete bands).

**BLOCK 3 — NEGATIVE PROMPT**

provenance-name text, breach-response text, full SCM graph, sweep numbers, abduction engine, drift monitor, interval values, continuous spectrum, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

## Video candidate pass

FIGURE 1 — three-ingredient assembly into the SCM
Status: STATIC SUFFICIENT
Criterion met: none — a fixed convergence/fan-out structure.
Reason: The reader needs to see where the three inputs meet and what the SCM feeds; nothing transforms over time.

FIGURE 2 — abduction → action → prediction
Status: VIDEO CANDIDATE
Criterion met: Criterion 1 (transition mechanism is the learning target) and Criterion 4 (the recovery of below-observation noise terms is a transformation the reader must otherwise mentally simulate).
Reason: This is the chapter's explicit "heart" — the whole point is *how* the same model is transformed across three operations (noise recovered, one equation surgically replaced while others persist, then run forward carrying that noise). A static triptych can only approximate the do-operator's surgical nature and the carry-through of recovered noise; an animation of one model morphing through abduction→action→prediction shows the mechanism the prose calls the categorical difference from an NBA engine. Strongest video case of the three chapters.

FIGURE 3 — three granularities of inference
Status: STATIC SUFFICIENT
Criterion met: none — a resolution comparison across three fixed levels.
Reason: The staircase from population to individual is inspected, not played; static panels let the reader hold all three resolutions at once.

FIGURE 4 — Living Model architecture
Status: STATIC SUFFICIENT (cyclical feedback present but not the learning target)
Criterion met: arguably Criterion 3 (a feedback loop), but the loop is a single trigger-and-update relationship, not a multi-stage cycle whose unfolding is the lesson.
Reason: The return arrow communicates the feedback adequately as a static path; the instructional content is the three components and their roles, which a snapshot conveys. Animating the loop would add motion without added learning.

FIGURE 5 — edge provenance scheme
Status: STATIC SUFFICIENT
Criterion met: none — a four-category key.
Reason: A provenance key is a reference table; motion is irrelevant.

Video candidates identified: 2 (Figures 2 and 4). Recommended for production: Figure 2 — abduction→action→prediction, a short looping or step-through animation, because the carry-through of recovered noise and the surgical single-equation replacement of the do-operator are transformation mechanisms that static panels can only approximate, and this is the chapter's declared central concept. Figure 4's feedback loop is well-served by a static return arrow — suggested format noted above.
