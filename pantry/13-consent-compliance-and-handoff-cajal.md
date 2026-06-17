# CAJAL Figure Report — Chapter 13: Consent, Compliance, and the Handoff
_Mode: /scan silent · 2026-06-16 · source: chapters/13-consent-compliance-and-handoff.md_

## Density recommendation

This chapter is governance-dense and structurally rich; it should ship **4 figures (2 Critical, 1 Important, 1 Supplementary)**. The chapter embeds two `[DIAGRAM/TABLE]` markers — the consent selection-collider graph and the handoff-brief template table — and CAJAL confirms both, promoting the collider graph to Critical because it is the chapter's load-bearing structural claim. Two additional figures earn their place: the four-stage test/build/kill pipeline with its evidence-level rubric (MC, Critical — it is the chapter's terminal governance process), and the public/IP firewall boundary (VG, Supplementary — a clean two-region selection structure). The regulatory-pathway material (M1/M2/M3 drivability) is real but is better carried as prose plus the firewall figure than as its own diagram; rendering "route to counsel" graphically risks implying adjudication, which the chapter forbids. The handoff-brief template is intrinsically a table of fields, not a spatial structure — keep it Important and render as a labeled-row stack, not a process. Recommend moderate density: the reader needs the collider graph early (it justifies FCI), the pipeline near the handoff section, and the firewall where IP is discussed. One video candidate flagged for the four-stage pipeline; recommend, not auto-select.

## Figures

---

FIGURE 1 — Consent as a selection collider (Berkson selection node on the sampling frame) · Critical · Causal graph (DAG/PAG) · VG

**BLOCK 1 — ILLUSTRAE PASTE:**
Render a small directed causal graph on a white background. Draw an unobserved physician-characteristics node at the top, distinguished by a dashed circular outline to mark it as latent. Draw two solid directed arrows from this node: one descending left to an opt-out/consent node, and one descending right to a message-responsiveness node. From the opt-out node draw a directed arrow into a node representing the observed sample, and enclose that observed-sample node inside a rectangular selection boundary drawn as a thin bracket frame to mark conditioning on the sampling frame. Add the prescribing-outcome node lower right, with an arrow from message-responsiveness into it. Mark the consent/selection node distinctly as the conditioning point. Keep the graph to six nodes maximum — latent characteristics, opt-out/consent, message responsiveness, observed sample (boxed as the selection boundary), outcome — with single-headed directed arrows only and one dashed latent node. Flat colors, one-point strokes, circular nodes, no text.

**BLOCK 2 — FULL SCOPE:**
- **[S]** Single-column, 89mm, 300 DPI, vector, textbook.
- **[C]** Chapter-confirmed: latent "physician characteristics" (unobserved) → opt-out AND → message responsiveness; opt-out → observed sample; conditioning on "physicians we have data for" is the selection boundary; this is a Berkson selection collider that opens a non-causal path. The framing "FCI models this; PC and GES assume it away" is chapter-confirmed (carried in caption/prose, not baked into the figure).
- **[O]** ~5–6 nodes; latent node dashed at top; two ↓ arrows fan to opt-out and responsiveness; opt-out → observed-sample node; selection boundary drawn as a bracket frame around the conditioned node; responsiveness → outcome. Single-headed directed arrows only.
- **[P]** Flat vector, white bg, Okabe-Ito. Latent characteristics node neutral gray dashed; opt-out/consent (the collider/blocking selection) #D55E00; message responsiveness #0072B2 dominant; observed-sample/selection boundary #56B4E9 anchor; outcome #009E73; arrows #000000 1pt.
- **[E] Exclusions:** no bidirected (dual-headed) edges in the main draw — if a PAG marking is wanted, indicate the unoriented edge with an open circle endpoint as a named point, but do NOT use rainbow or dual arrowheads; no more than six nodes; no FCI/PC/GES algorithm boxes; no equations; do not draw the non-causal opened path as a colored highlight that looks like a real causal arrow; [inferred] exact PAG edge-mark conventions (circles vs arrowheads) are not specified in the chapter — if rendered, mark as [inferred] and keep to a single open-circle endpoint convention.

**BLOCK 3 — NEGATIVE PROMPT:**
bidirected edges, dual-headed arrows, more than six nodes, algorithm boxes, equations, highlighted fake causal path, undirected tangle, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, hand-drawn styles, sketch lines, human figures (unless explicitly requested), visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

FIGURE 2 — Four-stage test/build/kill pipeline with evidence-level rungs · Critical · Process pipeline with gated stages · MC

**BLOCK 1 — ILLUSTRAE PASTE:**
Render a horizontal left-to-right pipeline of four sequential stage blocks on a white background, connected by single directional arrows. The first block is research and identification, the second is synthetic validation, the third is public-data demonstration, the fourth is partner replication and product discovery, with the fourth visually separated by a vertical boundary line marking the firewall it sits behind. Along the bottom, align a four-rung ascending ladder where each rung sits under the stage that can reach it, the rungs climbing left to right from associational to associational-plus-heterogeneity to quasi-experimentally-identified to replicated-on-partner-data. Between the third and fourth stage, draw a blocking bar with a perpendicular end-cap to mark that a public prototype cannot cross into product without replication. Keep to six to eight components: four stage blocks, the ascending rung set as one element, the firewall boundary line, and the blocking gate. Flat colors, one-point strokes, single-headed arrows, no text.

**BLOCK 2 — FULL SCOPE:**
- **[S]** Single-column, 89mm, 300 DPI, vector, textbook.
- **[C]** Chapter-confirmed: four stages in order — (1) research & identification, (2) synthetic validation, (3) public-data demonstration, (4) partner replication & product discovery; Stage 4 sits behind the firewall (internal/proprietary); evidence-level rubric Levels 1–4 mapped to the stages (Level 2 = associational+heterogeneity reached at Stage 3 on public Open Payments + Part D; Level 4 = replicated on partner data at Stage 4); the gate "handoff gates, does not bless" and "replicate before building" is chapter-confirmed.
- **[O]** Horizontal spine, four stage blocks with → arrows; vertical firewall boundary before Stage 4; ⊣ blocking bar between Stage 3 and Stage 4; ascending 4-rung ladder aligned beneath stages. 6–8 components.
- **[P]** Flat vector, white bg, Okabe-Ito. Stages 1–3 (public/synthetic) #56B4E9 anchor; Stage 4 (proprietary, behind firewall) #0072B2 dominant; evidence-rung ladder #009E73 active ascending; firewall boundary line + blocking bar #D55E00; spine arrows #000000 1pt.
- **[O-note]** The rung ladder is a single named element climbing left→right; do not render four separate disconnected charts.
- **[E] Exclusions:** no numeric evidence-level values printed; no waterfall imagery implying no feedback (chapter calls it validated-learning, not waterfall — keep arrows forward but the gate makes it conditional); no partner logos; no CRM screenshots; do not let Stage 4 appear reachable without crossing the gate; [inferred] vertical placement of rungs is illustrative; keep heights ordered but not numerically scaled.

**BLOCK 3 — NEGATIVE PROMPT:**
numeric level values, waterfall imagery, feedback-free linearity, partner logos, screenshots, Stage 4 reachable without gate, disconnected sub-charts, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures (unless explicitly requested), visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

FIGURE 3 — Handoff brief: the eight required governance fields · Important · Labeled-row template stack · MC (composite checklist)

**BLOCK 1 — ILLUSTRAE PASTE:**
Render a single vertical stack of eight aligned horizontal row blocks on a white background, representing the required fields of the terminal handoff brief. From top to bottom the rows are: test/build/kill recommendation, evidence level, assumptions, compliance flags, patient-welfare check, proprietary-replication-required, kill criteria, and the Risk-1 statement. Draw each row as a flat rectangular band of equal width and consistent height, separated by thin one-point dividers, left-aligned to a common spine. Give the bottom Risk-1 row a distinct fill and a small perpendicular marker on its left edge to mark it as the non-omittable field whose absence turns the brief into a sales pitch. Mark the compliance-flags row with a small routing stub pointing rightward off the block to indicate route-to-counsel, without resolving where it routes. Keep to eight rows plus the two markers, flat colors, one-point strokes, no text inside the rows.

**BLOCK 2 — FULL SCOPE:**
- **[S]** Single-column, 89mm, 300 DPI, vector, textbook.
- **[C]** Chapter-confirmed: the eight per-finding required fields verbatim from the chapter — test/build/kill; evidence level; assumptions; compliance flags; patient-welfare check; proprietary replication required; kill criteria; Risk-1 statement. "A brief missing any row is not a handoff; it is a sales pitch" is chapter-confirmed → motivates the Risk-1 emphasis marker. Compliance flags route-to-counsel (do not adjudicate) is chapter-confirmed → routing stub points out, unresolved.
- **[O]** 8 equal horizontal rows, left-aligned spine, thin dividers; Risk-1 row given distinct fill + left-edge marker; compliance-flags row given a single rightward route stub (open-ended, no terminus).
- **[P]** Flat vector, white bg, Okabe-Ito. Standard rows #56B4E9 anchor; Risk-1 row #D55E00 (non-omittable / blocking-if-absent emphasis); route-to-counsel stub #CC79A7 composite; dividers/spine #000000 1pt.
- **[E] Exclusions:** no text in rows; no checkbox glyphs implying completion; no more or fewer than eight rows; the route stub must NOT terminate in a labeled "counsel" box (routing, not adjudication); no traffic-light red-green status dots; do not imply an order of importance beyond the Risk-1 emphasis.

**BLOCK 3 — NEGATIVE PROMPT:**
row text, checkbox glyphs, completion ticks, more than eight rows, fewer than eight rows, route terminating in labeled box, traffic-light dots, red-green status, importance ranking, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures (unless explicitly requested), visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

FIGURE 4 — The public-data / IP firewall boundary · Supplementary · Boundary partition (two-region) · VG

**BLOCK 1 — ILLUSTRAE PASTE:**
Render a single vertical-boundary partition on a white background: one tall region on the left and one on the right, separated by a bold vertical firewall line down the center. In the left region place three stacked flat blocks representing the public artifacts — the methods, the synthetic dataset with known ground-truth structure, and the public-data project of Open Payments meals plus Medicare Part D. In the right region place three stacked flat blocks representing the proprietary artifacts — real CRM field mappings, partner performance deltas, and proprietary targeting methods. Draw one directional arrow crossing the boundary from left to right to represent the partner replicating the validated public methods internally, and place a perpendicular blocking bar on any reverse direction to show that proprietary content does not cross back out. Keep to eight components: the boundary line, three left blocks, three right blocks, and the single crossing arrow with its blocking end-cap. Flat colors, one-point strokes, no text.

**BLOCK 2 — FULL SCOPE:**
- **[S]** Single-column, 89mm, 300 DPI, vector, textbook.
- **[C]** Chapter-confirmed: public side = methods + synthetic dataset (known ground truth) + public-data project (Open Payments meals + Medicare Part D); private side (`private/`, gitignored, partner-internal) = real CRM field mappings + partner performance deltas + proprietary targeting methods; the partner replicates validated methods on real CRM data internally; firewall is a habit/reflex; failure mode is IP leakage outward. The directionality (validated methods flow in to be replicated; proprietary content must not leak out) is chapter-confirmed.
- **[O]** Central vertical boundary; 3 blocks left, 3 blocks right; one → crossing arrow left-to-right (replication); ⊣ blocking bar on the reverse/outward direction. 8 components.
- **[P]** Flat vector, white bg, Okabe-Ito. Public-side blocks #009E73 active (shippable); private-side blocks #0072B2 dominant (proprietary); firewall boundary + outward blocking bar #D55E00; crossing replication arrow #000000 1pt.
- **[E] Exclusions:** no lock/shield/padlock icons (no decorative metaphor); no git or folder iconography; no more than three blocks per side; the outward direction must be blocked (no symmetric two-way arrow); no partner names; do not depict the synthetic dataset on the private side (it is public).

**BLOCK 3 — NEGATIVE PROMPT:**
padlock icons, shield icons, lock metaphors, folder icons, git iconography, more than three blocks per side, symmetric two-way arrow, partner names, synthetic on private side, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures (unless explicitly requested), visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

## Video candidate pass

**FIGURE 2 (four-stage test/build/kill pipeline)**
- **Status:** Recommend (do not auto-select).
- **Criterion met:** ≥3 sequential causal stages and an essential gating transition — the learning target is precisely the *transition discipline* between stages (the handoff "gates, does not bless"), and the firewall crossing into Stage 4 is the conceptual hinge of the chapter. The evidence level *rising* as the work passes each stage, and the *blocking* of the public→product jump, are dynamic mechanisms, not static states.
- **Reason:** The chapter's two dead ends (a Fellow declaring the public prototype ready; a Fellow leaking a field mapping) are both *premature-transition* errors. An animation that walks a finding stage by stage, raises its evidence rung as each stage completes, then visibly *halts* at the firewall gate until partner replication occurs, teaches the gating logic better than a static pipeline — it shows the reader where the work is *not yet allowed to go*. Motion encodes the prohibition, which is the chapter's whole point.
- **Suggested format:** ~18–22s silent loop, four beats: (1) finding enters Stage 1, rung 1 lights; (2) advances through Stages 2–3, rung climbs to Level 2 on public data; (3) the finding pushes toward Stage 4 and is stopped by the firewall blocking bar (brief pause to register the halt); (4) only when a "partner replication" condition is met does it cross, rung climbing to Level 4. Okabe-Ito, no text, blocking bar #D55E00.

**Honesty caveats reflected in this report:** The consent figure depicts opt-out as a **Berkson/selection collider** whose population is **invisible by construction** — FCI plus the drift monitor are shown as *partial mitigations*, not a solution (consistent with the chapter's "structural limit, not a bug"). The regulatory material is rendered only as the firewall/handoff structure and a non-terminating route-to-counsel stub — **nothing in any figure adjudicates a regulatory question**; the educational pathway (M1) is the only one the chapter treats as legally drivable, and M3 reciprocity / off-label items are routed, not resolved. **Risk 1 (proprietary-data access, partner framing, consent ethics) is depicted honestly as an OPEN blocker** — it is the non-omittable bottom row of Figure 3 and the unmet condition gating Stage 4 in Figure 2; no figure implies a partner agreement exists. Veeva field names (`CLM_EXPLICIT_OPT_IN_vod`, `CLM_OPT_OUT_BEHAVIOR_vod`) are dated mid-2026 and are not baked into any figure as text.
