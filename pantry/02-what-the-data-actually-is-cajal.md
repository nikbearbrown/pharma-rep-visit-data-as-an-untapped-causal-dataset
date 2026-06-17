# CAJAL Figure Report — Chapter 02: What the Data Actually Is
_Mode: /scan silent · 2026-06-16 · source: chapters/02-what-the-data-actually-is.md_

## Density recommendation
Schema-heavy and structure-heavy — this chapter's value is in layouts and role classifications, not numbers. The two diagram comments and one table comment in the source are well-placed; figures should make verifiable the two-plane split, the consent gating, and the five-step assembly with an explicit time axis. No bar/forest charts warranted (no effect sizes here; the only numerics — 125+ Vault customers, Dec 2029 EOS, 80% — are dated `[verify]` facts, not chartable distributions). Target 4 figures: 2 Critical, 2 Important. One video candidate (assembly pipeline with time-index reveal).

## Figures

---

FIGURE 1 — Two-plane CLM schema converging via NPI into the physician-time panel · Priority: Critical · Type: layered block/architecture diagram · Heuristic: VG

BLOCK 1 — ILLUSTRAE PASTE BLOCK:
Render a layout diagram of how rep-visit data sources converge into one analysis panel. On the left, draw two stacked source boxes representing two structurally different planes: an upper box for passive, machine-logged telemetry and a lower box for rep-entered, interpretive post-call data; give each box a small set of internal field rows. Place a third source box on the right for warehouse data such as prescribing measures, payments, and cohort timing. Across the bottom, draw a horizontal gating band representing a consent layer that all source data must pass through. Draw connectors from all source boxes downward into the consent band, then from the consent band into a single wide target box on the lower right representing the physician-time panel, the convergence point. Label the join visually by routing every connector through one shared junction marker representing the NPI key. Make the two-plane left stack the conceptual anchor and the panel the destination. Use flat vector boxes, aligned rows, white background, one stroke weight, single-headed arrows, and color to distinguish passive from interpretive from warehouse from consent.

BLOCK 2 — FULL SCOPE:
[S]pecification: single-column 89mm, 300 DPI, vector, textbook.
[C]ontent: chapter-confirmed — Plane 1 passive telemetry (Duration_vod, Reaction_vod, Display_Order_vod, CLM_Presentation_vod, Multichannel_Activity_vod); Plane 2 rep-entered (Call2_Key_Message_vod, products detailed, free-text notes); Warehouse (NRx/TRx/NBRx, Open Payments, cohort timing); Consent layer gating NPI availability; all converge via NPI join → physician-time panel. Carry caveat: field names are dated mid-2026 examples (`*_vod`), and figure depicts public/synthetic structure only. Max 6 internal field rows per plane box (cap components).
[O]rganization: left = two stacked plane boxes (anchor); right = warehouse box; bottom = consent gating band all paths traverse; → all into single panel box; shared NPI junction marker on the joins.
[P]resentation: flat vector, white bg, Okabe-Ito — Plane 1 passive #56B4E9 (primary anchor), Plane 2 rep-entered #E69F00 (secondary), warehouse #CC79A7 (composite), consent band #D55E00 (gating/blocking), panel destination #0072B2 (dominant); connectors #000000 1pt single-headed.
[E]xclusions: no baked-in `*_vod` field text (render field rows as blank slots, not typeset names), no SQL, no ER-diagram crow's-foot notation, no real data values, no Salesforce/Vault logos, no more than 6 rows per box, no proprietary telemetry depiction.
[E] — highest leverage: keep field rows BLANK (no text) and keep the consent band as a single mandatory gate, not an optional side box — its gating role is the causal point.

BLOCK 3 — NEGATIVE PROMPT:
typeset field names, SQL text, crow's-foot ER notation, vendor logos, real data values, more than six rows per box, proprietary screenshots, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures (unless explicitly requested), visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

FIGURE 2 — Five-step panel assembly with an explicit time axis · Priority: Critical · Type: process pipeline with time axis · Heuristic: MC

BLOCK 1 — ILLUSTRAE PASTE BLOCK:
Render a left-to-right assembly pipeline that transforms raw logs into an analysis-ready panel. Begin with three small stacked input boxes on the far left representing three raw event logs: slide-view events, call records, and payment records. From them draw a single connector into a horizontal sequence of five numbered process nodes in order: aggregate to physician-week, define treatment from the message-content object, join the warehouse layer by key, lag the outcome forward in time, and quarantine the post-treatment block. Lead the pipeline into a final wide output box on the right representing the physician-time panel, whose interior is divided into column groups: treatment, lagged outcome, instrument, covariates, and a visually set-apart quarantined post-treatment group. Beneath the entire pipeline, draw one horizontal time axis arrow, with a clear marker dividing pre-treatment from post-treatment, and align the output column groups to their side of that divide. The time axis is the spine of the figure. Use flat vector shapes, uniform node sizing, white background, single stroke weight, single-headed arrows.

BLOCK 2 — FULL SCOPE:
[S]pecification: single-column 89mm, 300 DPI, vector, textbook.
[C]ontent: chapter-confirmed — three raw logs → (1) aggregate to NPI-week, (2) define treatment from Key_Message_vod/CLM_Presentation_vod, (3) join warehouse by NPI, (4) lag outcome 30/60/90d, (5) quarantine post-treatment block; output panel grouped treatment / lagged outcome / instrument / covariates / quarantined post-tx. Carry caveat: quarantine ≠ delete — those fields flip to treatment in the Ch.11 slide-sequencing estimand (depict as set-apart, not removed). Time axis must separate pre- from post-treatment columns.
[O]rganization: 3 inputs → 5 ordered nodes (→ progression) → 1 output box with 5 internal column groups; bottom = single time axis with a pre/post divider; quarantined group sits visually on the post side.
[P]resentation: flat vector, white bg, Okabe-Ito — input logs light gray (neutral), process nodes #0072B2 (dominant), instrument group #E69F00, covariate group #56B4E9, treatment group #009E73 (active), quarantined post-tx group #D55E00 (forbidden-for-this-estimand); time axis #000000 1pt single-headed arrow.
[E]xclusions: no code, no actual field-name text in column groups (blank labeled slots only), no numeric horizon values typeset (no "30/60/90"), no database icons, no more than 5 process nodes, no deletion/trash icon for quarantine (it is retained).
[E] — highest leverage: render quarantine as visually isolated-but-present (e.g., a bordered set-aside group), never as discarded; and the pre/post time divider must be unmistakable.

BLOCK 3 — NEGATIVE PROMPT:
code snippets, typeset field names, numeric horizon labels, trash/delete icons, database cylinder icons, more than five process nodes, SQL, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures (unless explicitly requested), visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

FIGURE 3 — Field-role classification on two independent axes (plane × causal role) · Priority: Important · Type: 2-axis classification matrix · Heuristic: VG

BLOCK 1 — ILLUSTRAE PASTE BLOCK:
Render a classification matrix establishing that a field's data plane and its causal role are independent axes. Set one axis for plane with four categories — passive telemetry, rep-entered, warehouse, consent — and a perpendicular axis for causal role with five categories — treatment, outcome, instrument, covariate, collider. Form a grid of cells from the crossing of the two axes. Place a small number of marker tokens into specific cells to show that fields scatter across the grid rather than aligning along a diagonal: for instance one token where passive telemetry meets collider, another where rep-entered meets treatment, others at warehouse-instrument, warehouse-covariate, warehouse-outcome, and consent-collider. The key visual message is that the tokens do NOT line up on a diagonal — plane does not predict role. Keep the grid uniform and aligned, distinguish the markers by the color of their causal role, white background, flat vector shapes, single stroke weight, no diagonal guide line. No category text, no field-name text.

BLOCK 2 — FULL SCOPE:
[S]pecification: single-column 89mm, 300 DPI, vector, textbook.
[C]ontent: chapter-confirmed — two independent axes: plane (passive telemetry / rep-entered / warehouse / consent) and role for the total message→NRx effect (treatment / outcome / instrument / covariate / collider). Confirmed placements: Reaction_vod = passive × collider; Call2_Key_Message_vod = rep-entered × treatment; cohort timing = warehouse × instrument; baseline NRx/specialty/geo = warehouse × covariate; NRx@horizon = warehouse × outcome; consent status = consent × (selection) collider; CLM_Presentation_vod/Key_Message_vod = treatment. Carry caveat: roles flip under a different estimand (Reaction/Duration become treatment in Ch.11) — show as a single estimand snapshot only. Keep markers ≤7.
[O]rganization: 4-column × 5-row grid; scattered (non-diagonal) marker placement is the whole point; no trend line.
[P]resentation: flat vector, white bg, Okabe-Ito role colors — treatment #009E73, outcome #0072B2, instrument #E69F00, covariate #56B4E9, collider #D55E00; grid lines light gray 0.5pt, marker strokes 1pt.
[E]xclusions: no axis category text, no field names, no diagonal reference line (would imply correlation between axes — false), no more than 7 markers, no heat shading of cells, no checkmarks/x-marks.
[E] — highest leverage: NO diagonal line and scatter the markers off-diagonal — the figure's thesis is axis independence.

BLOCK 3 — NEGATIVE PROMPT:
diagonal reference line, axis category text, field names, cell heat shading, checkmarks, x-marks, more than seven markers, trend lines, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures (unless explicitly requested), visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

FIGURE 4 — The consent collider: opt-out gating creates a selected sampling frame · Priority: Important · Type: small causal/selection graph · Heuristic: MC

BLOCK 1 — ILLUSTRAE PASTE BLOCK:
Render a compact causal-structure diagram showing how consent selection sits between two sets of influences. Place a central node representing whether a physician's row enters the panel — the inclusion/consent node. Draw an incoming arrow into this node from a treatment-side cluster on the left, representing factors such as engaged reps and high-contact physicians, and a second incoming arrow into the same node from an outcome-side cluster on the right, representing factors such as prescribing propensity and industry skepticism. Because two arrows point into the shared inclusion node, render that node distinctly as a collider where conditioning occurs — for example as a node with a small enclosing frame indicating the sample is selected on it. Then show that selecting on this node opens a spurious association by drawing a dashed connector arcing between the treatment-side cluster and the outcome-side cluster, marked as induced rather than causal. Keep the layout symmetric, left cluster and right cluster feeding one central selected node, white background, flat vector shapes, single stroke weight, standard single-headed arrows for causal edges and one clearly distinct dashed line for the induced path.

BLOCK 2 — FULL SCOPE:
[S]pecification: single-column 89mm, 300 DPI, vector, textbook.
[C]ontent: chapter-confirmed — consent governs whether a physician's row exists; consent plausibly correlated with treatment-side factors (engaged reps, high-contact physicians) AND outcome-side factors (prescribing propensity, industry skepticism); selecting on consent = conditioning on a collider → opens a spurious treatment–outcome path; no covariate adjustment fixes a sampling-frame selection. Carry caveat strongly: the correlation is "highly plausible but unproven" — mark the induced/dashed path as [inferred]/hypothesized, not established. Public/synthetic structure only.
[O]rganization: left cluster → central inclusion node ← right cluster; central node framed as selected; dashed induced edge arcs left-cluster ⟷ right-cluster (clearly non-causal styling).
[P]resentation: flat vector, white bg, Okabe-Ito — treatment-side cluster #009E73, outcome-side cluster #0072B2, central consent/collider node #D55E00 (the bias locus), causal arrows #000000 1pt solid single-headed, induced path a single dashed gray line.
[E]xclusions: no field names, no "collider" text, no probability notation, no more than 2 source clusters, no extra confounder nodes, no double-headed arrow for the induced path (use one dashed line, not a dual-head), no plate notation.
[E] — highest leverage: the induced path MUST be visually distinct from causal arrows (dashed, no full arrowhead) so readers do not misread it as a real cause; keep it marked hypothesized.

BLOCK 3 — NEGATIVE PROMPT:
field names, probability notation, extra confounder nodes, plate notation, dual-headed induced arrow, solid induced path, more than two clusters, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures (unless explicitly requested), visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

## Video candidate pass

VIDEO CANDIDATE — Figure 2 (five-step assembly with time axis)
- Status: Recommended
- Criterion met: ≥3 sequential causal/transformational stages where the transformation itself is the learning target — raw event logs are progressively reshaped into a time-indexed panel, and the *time-index reveal* (pre/post divider snapping into place) is exactly what protects the estimate.
- Reason: The chapter's thesis is "correctness is a property of the time index, not the column count." A staged build — logs aggregate, treatment defined, warehouse joins, outcome slides forward in time (lag), post-treatment block visibly peels off into quarantine — dramatizes cause-precedes-effect in a way the static figure can only imply. The lag step animating outcome moving rightward past the treatment marker is the payoff beat.
- Suggested format: 15–20s silent staged build; emphasize step 4 (lag) as motion along the time axis and step 5 (quarantine) as the post-tx group separating; no narration text required.

Figures 1, 3, 4 are static-appropriate (layout, classification snapshot, fixed graph structure). Target ~1 video/chapter met.
