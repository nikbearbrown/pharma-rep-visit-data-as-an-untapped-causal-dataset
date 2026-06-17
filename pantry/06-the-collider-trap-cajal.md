# CAJAL Figure Report — Chapter 06: The Collider Trap (and the Consent Collider)
_Mode: /scan silent · 2026-06-16 · source: chapters/06-the-collider-trap.md_

## Density recommendation

This chapter teaches one structural inversion (conditioning on a collider opens a path) and applies it twice: to an *observed* post-treatment collider (dwell/reaction) and to a *selection* collider (consent / sample membership). The figure-worthy concepts are: the three-structure comparison (fork vs. chain vs. collider — the inversion itself); the dwell/reaction collider in the pharma DAG, shown both unconditioned (blocked) and conditioned (spurious path opened); the consent selection collider; and the "more wrong, more confident" quantitative tell (estimate diverges from truth while the standard error shrinks). FCI/PAG output is a candidate but is described, not visualized in detail — keep it as supplementary at most.

Recommended density: **4 figures** — 1 Critical (three-structure inversion: fork/chain/collider), 1 Critical (dwell/reaction collider, blocked-vs-conditioned two-state), 1 Important (consent selection collider with the absent opt-out population), 1 Important (the "more wrong, more confident" estimate-vs-SE chart). The fork/chain/collider triptych is the conceptual keystone and must be Critical. The two-state dwell figure (path closed → path opened by conditioning) is the chapter's core applied claim. The Berkson/consent figure must make the *absent* population visible — the bias "lives in who is absent."

This is a chapter where the temptation is to over-illustrate the d-separation rules; resist it. Four figures cover the load. One video candidate (the two-state conditioning toggle on the dwell collider) is strongly indicated because the *transition* from closed to open path is exactly the learning target.

## Figures

---

FIGURE 1 — The one inversion: fork vs. chain vs. collider · Critical · Three-panel structural comparison · VG + MC

BLOCK 1 ILLUSTRAE PASTE:
Render three small flat vector causal-graph panels side by side, each containing three nodes labeled by position only. In the left panel, draw a fork: a top center node with two single-headed arrows descending, one to a lower-left node and one to a lower-right node. In the middle panel, draw a chain: a left node with a single-headed arrow to a center node, and the center node with a single-headed arrow to a right node, all in a row. In the right panel, draw a collider: a lower-left node and a lower-right node each sending one single-headed arrow up into a shared top center node, so the two arrows point inward and meet at the common effect. Beneath each panel reserve a blank caption strip. To make the inversion legible, render a small conditioning indicator — a square box drawn around the central/common node — in all three panels, and use a consistent visual convention: in the fork and chain panels the boxed node sits between the two endpoints, while in the collider panel the boxed node is the meeting point of the two inbound arrows. Keep the three panels identical in size, evenly spaced, and aligned on a shared baseline. Leave all nodes unlabeled. Flat vector, white background, one-point strokes, no shading.

BLOCK 2 FULL SCOPE:
[S] Single-column 89mm, 300 DPI, vector, three-panel horizontal strip.
[C] Chapter-confirmed: Panel 1 fork A←C→B ("conditioning on C removes the A–B association — this helps"); Panel 2 chain A→C→B ("conditioning on C blocks the path — appropriate if C is a mediator you want to block"); Panel 3 collider A→C←B ("A and B independent through C; conditioning on C opens a spurious path — this hurts"). The chapter's DIAGRAM stub specifies exactly these three panels and their labels.
[O] Three equal panels left→right; fork (arrows out), chain (arrows through), collider (arrows in); a conditioning box around C in each to make the same operation visibly differ in consequence (helps / blocks / hurts).
[P] Flat vector, white background, Okabe-Ito. Fork (helpful conditioning) #009E73; chain (context-dependent) #E69F00; collider (harmful conditioning) #D55E00; nodes neutral gray #000000 outline; conditioning box neutral gray dashed 1pt. Arrows 1pt single-headed.
[E] No A/B/C letters baked in (caption strip handles it). The collider's two arrows must point INTO the common node — not out. No dashed "spurious path" line in this overview figure (reserve that for Figure 2). Exactly three panels, three nodes each.

BLOCK 3 NEGATIVE PROMPT:
baked node letters, collider arrows pointing outward, spurious-path dashed line in overview, fourth panel, more than three nodes per panel, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

FIGURE 2 — Dwell/reaction collider: path closed vs. path opened · Critical · Two-state DAG · MC + VG

BLOCK 1 ILLUSTRAE PASTE:
Render two flat vector causal graphs side by side sharing an identical four-node layout, so the only difference between them is a single conditioning operation. In each graph: place a treatment node on the left and an outcome node on the right; place a latent trait node above center, drawn with a distinct outline to mark it unobserved; place a central lower node representing an in-visit engagement measure. Draw these solid single-headed arrows in both graphs: treatment to outcome (the direct effect along the bottom); treatment up-and-in to the central engagement node; the latent trait down to the central engagement node; and the latent trait to the outcome. The central engagement node is therefore a common effect of treatment and latent trait — a collider. In the LEFT graph, leave the engagement node untouched: the path between treatment and outcome that would run through the latent trait stays blocked. In the RIGHT graph, draw a conditioning box around the engagement node and add a single dashed spurious path connecting treatment to outcome that runs via the latent trait, indicating the path opened by conditioning. Keep both graphs identical in node position and arrow set; only the conditioning box and the dashed spurious path distinguish the right from the left. Leave all nodes unlabeled. Flat vector, white background, one-point strokes.

BLOCK 2 FULL SCOPE:
[S] Single-column 89mm, 300 DPI, vector, two-panel.
[C] Chapter-confirmed (DIAGRAM stub + body): nodes Message (D), NRx (Y), latent Physician Openness, Dwell/Reaction (the collider). Arrows: Message→NRx, Message→Dwell/Reaction, Openness→Dwell/Reaction, Openness→NRx. Left = without conditioning, collider blocks spurious path; Right = conditioning box around Dwell/Reaction opens dashed spurious Message–NRx path through Openness. Both states are chapter-explicit ("Without conditioning... With conditioning...").
[O] Two identical four-node graphs; only difference is the ⊣ (blocked, left) vs. opened-via-conditioning (right) state; dashed spurious path appears only on the right; latent node marked by distinct (e.g., dashed/circle) outline in both.
[P] Flat vector, white background, Okabe-Ito. Message #0072B2; NRx neutral gray; latent Openness #CC79A7 (composite/latent) with dashed outline; Dwell/Reaction collider node #56B4E9; conditioning box + dashed spurious path #D55E00 (the bias) on the right panel only. Solid 1pt for true edges; dashed 1pt for the spurious induced path.
[E] Spurious dashed path appears ONLY in the right panel. Latent Openness must be visually distinct from observed nodes. Do NOT add samples/other downstream nodes (keep to the four-node core). No standard-error annotation here (that is Figure 4). Single-headed arrows; the spurious path is the only dashed line and must not be double-headed.

BLOCK 3 NEGATIVE PROMPT:
spurious path in left panel, double-headed spurious path, extra downstream nodes, standard-error annotation, indistinct latent node, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

FIGURE 3 — Consent as a selection collider (the absent population) · Important · Selection DAG · VG

BLOCK 1 ILLUSTRAE PASTE:
Render a flat vector causal graph centered on a selection node. Place a treatment-side cause node on the upper left and an outcome-side cause node on the upper right, each sending one single-headed arrow downward and inward into a central selection node representing consent — so the two arrows converge on the common effect from both sides. Below the selection node, draw a horizontal divider line. Above the divider, inside a solid enclosing boundary, place the observed sample: a cluster of small filled marks representing the physicians who consented. Below the divider, draw a second cluster of small hollow, faded marks representing the physicians who opted out and are absent from the data; enclose them in a dashed boundary to signal they are unobserved. Add a small dashed spurious path arcing between the treatment-side cause and the outcome-side cause to indicate the association manufactured by conditioning on the selection node. The visual contrast between the solid observed cluster and the faded dashed absent cluster is the central message: the bias lives in who is missing. Keep the two cause nodes symmetric, the selection node centered, and the two population clusters clearly separated by the divider. Leave all nodes and marks unlabeled. Flat vector, white background, one-point strokes, no shading.

BLOCK 2 FULL SCOPE:
[S] Single-column 89mm, 300 DPI, vector.
[C] Chapter-confirmed (DIAGRAM stub + body): treatment-side cause = rep engagement / visit frequency / high contact; outcome-side cause = industry-skepticism / prescribing propensity; both → Consent (S=1) selection node; observed sample = only S=1 physicians; opt-out population absent; conditioning on S opens a spurious message→NRx (cause-to-cause) association — Berkson / Hernán et al. 2004 selection-as-collider. "The selection collider is invisible in the data — it lives in who is absent" is the chapter's explicit framing.
[O] Two cause nodes ⟶ converging into central consent node (collider-in); divider separating solid observed cluster (above) from dashed faded absent cluster (below); dashed spurious arc between the two causes.
[P] Flat vector, white background, Okabe-Ito. Treatment-side cause #56B4E9; outcome-side cause #E69F00; consent selection node #0072B2; observed cluster marks neutral gray solid; absent opt-out cluster faded gray hollow with dashed boundary; spurious arc #D55E00 dashed. 1pt strokes.
[E] The absent population MUST appear (faded/dashed) — omitting it defeats the figure's point. Spurious arc is dashed and single-headed. No NPI numbers or field names baked in. Do not draw the full population DAG as a third panel (chapter mentions it but a two-zone single panel is cleaner; name the full-population contrast in caption). Max ~7 structural elements plus the two small clusters.

BLOCK 3 NEGATIVE PROMPT:
omitted absent population, opt-out cluster drawn solid, baked NPI or field names, third full-population panel, double-headed spurious arc, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

FIGURE 4 — More wrong, more confident (estimate vs. standard error) · Important · Dot/interval chart · PQ

BLOCK 1 ILLUSTRAE PASTE:
Render a flat vector estimate-and-interval chart with three stacked rows, each a point estimate drawn as a dot with a horizontal confidence interval whisker extending symmetrically to each side. The horizontal axis represents the estimated message effect and runs across the figure; draw one vertical reference line marking the planted true effect. The top row is the honest specification with no post-treatment controls: its dot sits on or very near the true-effect reference line, with a moderately wide whisker. The middle row is the collider-controlled specification: its dot sits visibly off to one side of the true-effect line, biased away from truth, yet its whisker is conspicuously the narrowest of the three. The bottom row is the consenting-subsample-only specification: its dot also sits off the true-effect line, with its own whisker. The intended reading is paradoxical and must be unmistakable: the row farthest from the truth (the collider-controlled one) has the tightest interval. Align all three dots on a common horizontal scale, equal row spacing, whiskers drawn at one-point weight with small end caps. Leave axis and rows unlabeled. Flat vector, white background, no shading.

BLOCK 2 FULL SCOPE:
[S] Single-column 89mm, 300 DPI, vector. (Interval plot; x-axis is an effect scale, may be centered on zero or on the true value — the true-effect reference line is the anchor.)
[C] Chapter-confirmed: three specifications — honest (no post-treatment controls, full sample) lands near planted truth; collider-controlled (dwell + reaction added) diverges from truth AND has the smaller standard error ("more wrong, more confident — the collider trap made visible"); consenting-subsample-only also diverges. Planted true effect is known (synthetic ground truth). The narrow-but-biased middle interval is the chapter's central quantitative tell.
[O] Three horizontal point-estimate rows with CI whiskers; vertical true-effect reference line; the deliberate inversion — narrowest whisker on the most-biased row — is the message (⊣ contradiction of the usual "tighter = better" intuition).
[P] Flat vector, white background, Okabe-Ito. Honest estimate #009E73 (lands on truth); collider-controlled estimate #D55E00 (biased, tightest CI — the trap); consenting-only estimate #CC79A7; true-effect reference line neutral gray #000000 vertical. Dots filled, whiskers 1pt.
[E] X-axis baseline/scale honest (the true-effect line is the reference; do not truncate to exaggerate). The middle (collider) interval MUST be the narrowest while being off-truth — that is the whole point. No numeric effect values baked in. Exactly three rows. Do not color the collider-controlled row green or otherwise as "best."

BLOCK 3 NEGATIVE PROMPT:
collider row drawn widest, collider row colored as best, truncated axis exaggeration, baked numeric estimates, fourth specification row, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

## Video candidate pass

**FIGURE 2 — Dwell/reaction collider: path closed vs. path opened**
- **Status:** Recommended (not auto-selected).
- **Criterion met:** The transition mechanism IS the learning target — conditioning on the collider is an *action* that *changes* the graph's connectivity (a closed path becomes open). This is precisely the sub-observable transformation a still cannot fully convey.
- **Reason:** The chapter's whole thesis is that one operation — adding dwell/reaction to the model — flips the estimate from honest to corrupted. A toggle animation (collider open in the graph → conditioning box drops onto the node → the dashed spurious Message–NRx path through Physician Openness lights up) makes the causal damage of the analyst's single keystroke visceral. The viewer sees the path that "was closed before she touched it" open in real time. Of the chapter's figures this is the one where motion teaches what a static pair only suggests.
- **Suggested format:** 10–15s vector toggle animation: start at the unconditioned (blocked) state, animate the conditioning box closing around the Dwell/Reaction node, then animate the dashed #D55E00 spurious path drawing itself from Message through Openness to NRx; hold; optionally loop back to closed. No baked narration. (Figure 4's "narrow-but-biased" inversion is a strong runner-up but reads well as a static; only one video per chapter is the budget.)

Honesty notes carried into the figures: Dwell time and reaction score are **post-treatment colliders, not confounders** — Figures 1, 2, and 4 all encode position (downstream of treatment) as the deciding fact, never predictive power; the chapter's rule "position decides inclusion, fit does not" is the reason Figure 4 shows the tightest interval on the *wrong* answer. Consent opt-out is a **Berkson / selection collider** (Figure 3), which is the structural motivation for FCI over PC/GES — the absent opt-out population is drawn explicitly because "the bias lives in who is absent." Veeva field names (`Duration_vod`, `Reaction_vod`, `CLM_EXPLICIT_OPT_IN_vod`) are dated mid-2026 and flagged in-chapter as an aging risk; they are intentionally NOT baked into any figure (blank vector, caption-only). Public/IP firewall: no real-data values depicted; all magnitudes are synthetic-ground-truth illustrations of mechanism, not population estimates.
