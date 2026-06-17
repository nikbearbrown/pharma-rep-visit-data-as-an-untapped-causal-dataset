# CAJAL Figure Report — Chapter 05: The Causal Graph and the Three Pathways
_Mode: /scan silent · 2026-06-16 · source: chapters/05-the-causal-graph-and-three-pathways.md_

## Density recommendation

This chapter's spine is a mediation argument with three correlated pathways, and its central honesty point is that the per-pathway split is generally **not point-identified**. The figure-worthy concepts are: the three-pathway mediation DAG (the chapter draws it twice, as ASCII and as a stub); the counterfactual estimand vocabulary (TE/NDE/NIE/CDE) which is a comparison table, not a picture; the assumption-hierarchy stack (what quasi-randomization buys at each tier); the non-identification phenomenon (order-dependence of the education share); and the regulatory color map that overlays the pathway graph.

Recommended density: **4 figures** — 1 Critical (three-pathway mediation graph with regulatory color), 1 Critical (the estimand comparison table), 1 Important (assumption hierarchy stack), 1 Important (non-identification / order-dependence). The regulatory-color overlay should be folded *into* the mediation graph rather than drawn separately — they are the same nodes. Avoid drawing the counterfactual contrasts (Y(1,M(0)) etc.) as a figure; they are notation best left as a table. This chapter is conceptually dense but visually it needs the graph and the table to do most of the work. One video candidate (the non-identification order-swap) is recommended.

A caution this report enforces throughout: do NOT render any figure that implies a clean three-way percentage split (e.g., a stacked bar "60% M1 / 25% M2 / 15% M3"). The chapter exists to deny that such a figure is honest. The non-identification figure must show *disagreement across orderings*, not a settled share.

## Figures

---

FIGURE 1 — Three-pathway mediation graph with regulatory color · Critical · DAG · MC + VG

BLOCK 1 ILLUSTRAE PASTE:
Render a flat vector causal graph. On the far left place an instrument node; an arrow leads right from it to a treatment node. From the treatment node, three solid single-headed arrows fan out rightward to three stacked mediator nodes arranged vertically: an upper, a middle, and a lower mediator. From each of the three mediator nodes a solid single-headed arrow converges rightward onto a single outcome node on the far right. Add one additional source node at the lower left, separate from the instrument, with a single-headed arrow feeding into the lower mediator only, representing an external public-data source entering the reciprocity channel. Color the three mediator nodes and their arrows distinctly by tier: the upper mediator and its arrows in a permissive tier color, the middle mediator in a cautionary tier color, the lower mediator in a high-scrutiny tier color — but do not use any red-and-green pairing; use the specified colorblind-safe substitutes. Keep the fan-out and fan-in symmetric, the three mediators evenly spaced, and the whole graph on a clean left-to-right reading axis. Leave every node and arrow unlabeled for captioning. Flat vector, white background, one-point strokes, no shading.

BLOCK 2 FULL SCOPE:
[S] Single-column 89mm, 300 DPI, vector. (Wide aspect acceptable; six nodes plus external source.)
[C] Chapter-confirmed nodes: Z (cohort timing, instrument), D (message variant), M1 (educational attitude), M2 (samples/co-pay/follow-up), M3 (reciprocity/sponsored meals), Y (NRx). Arrows: Z→D; D→M1; D→M2; M3→Y with meals entering M3 from Open Payments public data; all three M→Y. Regulatory tiers chapter-explicit: M1 green/permissive, M2 amber/cautionary, M3 red/scrutinized. NOTE: chapter's stub draws D→M1 and D→M2 but the reciprocity meals enter M3 from the external Open Payments source rather than from D — preserve that asymmetry faithfully.
[O] Left-to-right: Z → D, then D fans to M1/M2/M3 (→ progression), three M nodes fan-in to Y; external Open Payments source ⟶ M3 from below-left.
[P] Flat vector, white background, Okabe-Ito mapped to the chapter's regulatory semantics WITHOUT red-green: M1 (permissive) #009E73; M2 (cautionary) #E69F00; M3 (scrutinized) #D55E00; Z anchor #56B4E9; D #0072B2; Y neutral gray; external Open Payments source #CC79A7. 1pt single-headed arrows.
[E] Do NOT draw D→M3 as the meals path (meals enter M3 from Open Payments, not from D — chapter is specific). No measurement-confidence text baked in (caption work). No percentage shares on arrows. Max 7 nodes (Z, D, M1, M2, M3, Y, Open-Payments) — at ceiling; do not add latent-confounder nodes here (those are Chapter 6's territory). No red-green: the green/amber/red regulatory language is realized via #009E73 / #E69F00 / #D55E00.

BLOCK 3 NEGATIVE PROMPT:
D-to-M3 meals arrow, percentage shares on arrows, baked measurement-confidence labels, latent confounder nodes, more than seven nodes, red-green tier coloring, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

FIGURE 2 — Estimand comparison table (TE / NDE / NIE / CDE / IIE) · Critical · Comparison matrix · MC

BLOCK 1 ILLUSTRAE PASTE:
Render a flat vector comparison matrix as a clean grid with five rows and five columns. The five rows correspond to five distinct effect estimands; the five columns correspond to attributes: what question it answers, the key identifying assumptions, whether the cross-world assumption is required, whether an exact three-mediator decomposition is achieved, and a suitability verdict. Render the table as a minimal ruled grid: thin horizontal separators between rows, a slightly heavier rule beneath the header row, no vertical rules or only the faintest. In the cross-world column and the exact-decomposition column, use small geometric status glyphs rather than words — a filled circle for "required/yes," an open circle for "not required/no" — so the binary distinctions read at a glance. Keep cell padding generous and consistent, all rows equal height, the header row visually distinct by weight only. Leave all text cells blank as placeholder rectangles of consistent height for later typesetting; render only the structural grid and the status glyphs. Flat vector, white background, one-point rules, no shading or zebra striping.

BLOCK 2 FULL SCOPE:
[S] Single-column 89mm, 300 DPI, vector (table; may run slightly wider if needed).
[C] Chapter-confirmed rows (from the TABLE stub): Baron–Kenny; Natural Direct/Indirect Effects (NDE/NIE); Controlled Direct Effect (CDE); Interventional Indirect Effects (Vansteelandt & Daniel 2017). Columns chapter-explicit: what it answers; key identifying assumptions; cross-world required?; exact three-correlated-mediator decomposition?; appropriate use / one-line verdict. (Five rows if Baron–Kenny + NDE + NIE counted together vs. separate — chapter groups NDE/NIE; keep as one row plus the others = 4 rows, or split to 5. Default 4 substantive rows. [inferred row count])
[O] Matrix layout; rows = estimands, columns = attributes; binary columns (cross-world; exact decomposition) rendered as filled/open status glyphs for instant ⟶ vs. ⊣ reading.
[P] Flat vector, white background, Okabe-Ito sparingly: header rule #000000; status glyph "required/no-good" #D55E00 (filled), "not-required/good" #009E73 (open or filled per legend); grid rules neutral gray 1pt. Keep mostly monochrome — this is a table, color only on the two binary status columns.
[E] No baked cell text (structure only; placeholder rectangles). No zebra striping/shaded rows. No vertical rules competing with content. Do not invent rows beyond the four chapter estimands. The cross-world and decomposition columns must be the only color-coded ones.

BLOCK 3 NEGATIVE PROMPT:
baked cell text, zebra striping, shaded alternating rows, heavy vertical rules, invented estimand rows, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

FIGURE 3 — Assumption hierarchy (what quasi-randomization buys) · Important · Layered stack · VG + MC

BLOCK 1 ILLUSTRAE PASTE:
Render a flat vector three-tier stacked diagram, like a pyramid of nested horizontal layers, widest at the bottom and narrowing upward. The bottom layer is the broadest and represents the total effect, resting on the most secure foundation. The middle layer, narrower, sits on top of it and represents the single-mediator direct-and-indirect split. The top layer, narrowest, represents the three-mediator per-pathway split, the most precarious. To the left of the stack, draw a vertical arrow pointing upward spanning all three layers, indicating increasing assumption burden from bottom to top. To the right of each layer, place a small bracket marking how many identifying assumptions that layer requires: the bottom layer the fewest, the top the most. Use a solid fill saturation that decreases from bottom to top, so the most secure layer reads as most solid and the least identified layer reads as faintest — but keep all fills within a single hue family to avoid implying categorical difference. Keep the three layers concentrically centered and cleanly separated by thin gaps. Leave all layers and brackets unlabeled. Flat vector, white background, one-point strokes, no gradient fills.

BLOCK 2 FULL SCOPE:
[S] Single-column 89mm, 300 DPI, vector.
[C] Chapter-confirmed three tiers (from the DIAGRAM stub): bottom = Total Effect (IV estimate), requires assumptions 1 & 3, secured by quasi-randomization; middle = single-mediator NDE/NIE, requires all 4 including cross-world; top = three-mediator split, additionally requires no between-mediator confounding. The "increasing assumption burden upward" is the chapter's explicit point that TE is far more credible than any pathway share.
[O] Three nested/stacked layers, widest (most secure) at base → narrowest (least identified) at apex; left-side upward arrow ⟶ for rising assumption burden; right-side brackets indicating assumption count per tier.
[P] Flat vector, white background, single-hue saturation ramp using #0072B2 (most secure, deepest) → lighter tint at apex; burden arrow neutral gray. Avoid multi-hue (would imply categorical, not graded, difference). 1pt strokes.
[E] No gradient fills (use flat tint steps, not a continuous gradient). No assumption text baked in. No fourth tier. Keep within one hue family — do not color tiers green/amber/red (that semantic belongs to Figure 1, not here). Max 3 layers + 1 arrow + brackets.

BLOCK 3 NEGATIVE PROMPT:
continuous gradient fill, baked assumption text, fourth tier, multi-hue tier coloring, green-amber-red layers, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

FIGURE 4 — Non-identification: education share flips with mediator order · Important · Comparison chart · PQ + VG

BLOCK 1 ILLUSTRAE PASTE:
Render a flat vector chart that demonstrates instability rather than a settled value. Draw three short horizontal bars stacked vertically, each representing the estimated education-pathway share under a different analysis choice: the top bar under one mediator ordering, the middle bar under a different mediator ordering, and the bottom bar a reference marker for the unknown ground-truth share. The horizontal axis runs from zero to one hundred percent and begins at zero. The top two bars should reach visibly different lengths from one another, demonstrating that the same data yield different shares depending only on ordering. The bottom ground-truth reference should be drawn as a single distinct vertical tick or a hollow bar at yet another position, aligning with neither of the two estimates. Add a light vertical reference line dropped from each estimate so the eye registers the gap between them. The visual message is divergence: the two solid estimate bars must not agree, and neither must coincide with the reference. Keep all three bars equal in height, left-aligned to a common zero baseline. Leave bars and axis unlabeled. Flat vector, white background, one-point strokes, no shading.

BLOCK 2 FULL SCOPE:
[S] Single-column 89mm, 300 DPI, vector. Bar x-axis from zero (percentage share).
[C] Chapter-confirmed: Baron–Kenny education share changes with mediator entry order ("running the mediators in a different order produces a different answer"); neither ordering equals the ground-truth split because it is not point-identified; this instability "is the identification problem made visible." Two orderings + a ground-truth reference is the chapter's exact demonstration (CLI exercise: "two different orders... both should miss ground truth").
[O] Three horizontal bars (ordering A, ordering B, ground-truth reference), common zero baseline; deliberate non-coincidence is the message (⊣ — no convergence); vertical drop lines mark the gaps.
[P] Flat vector, white background, Okabe-Ito. Ordering-A estimate #E69F00; ordering-B estimate #CC79A7 (two contestable estimates, deliberately neither "good"); ground-truth reference neutral gray hollow/tick #000000. Avoid #009E73 here — do not signal any estimate as "correct." 1pt strokes.
[E] Y-axis/x-axis from zero. Do NOT render a stacked three-segment "M1/M2/M3" share bar — that would imply identification and is the exact error the chapter forbids. No percent values baked in. No "winner" coloring. Exactly three bars.

BLOCK 3 NEGATIVE PROMPT:
stacked three-segment share bar, identified pathway split, percent value labels, winner/correct-estimate coloring, truncated axis, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

## Video candidate pass

**FIGURE 4 — Non-identification: education share flips with mediator order**
- **Status:** Recommended (not auto-selected).
- **Criterion met:** Sub-observable transformation made visible — the learning target is that *the same data and method yield a different answer when you change one arbitrary choice* (mediator entry order). That instability is the chapter's "single most important honesty point."
- **Reason:** A still image of two unequal bars states the result; an animation of the *act* of reordering — entering M1→M2→M3 to produce share A, then visibly swapping the order and watching the bar jump to share B while ground truth stays put — dramatizes that the share has no fixed value. The movement *is* the argument. This is the rare case where motion teaches non-identification better than a static contrast.
- **Suggested format:** 15–20s vector animation: bar settles at share A under one ordering, the mediator order swaps (small reorder animation), bar slides to share B, ground-truth reference line holds steady throughout so the viewer sees both estimates miss it. No baked narration.

Honesty notes carried into the figures: The three-pathway split (educational / relationship / reciprocity) is **generally NOT point-identified** with correlated, mutually-causal mediators — Figure 4 is built specifically to depict that, and Figure 2's "exact decomposition?" column flags it per-row. Any defensible decomposition reports **interventional** indirect effects (Vansteelandt & Daniel 2017), not naive products of coefficients — captured as a distinct row in Figure 2 with its own assumption profile. The regulatory green/amber/red of Figure 1 is realized via #009E73 / #E69F00 / #D55E00 to honor the colorblind/no-red-green rule while preserving the chapter's M1/M2/M3 semantics. Public/IP firewall respected: meals enter from public Open Payments (rendered as the external #CC79A7 source); the M1 free-text proxy and M2 fields are synthetic-side and are not depicted as high-confidence measured edges.
