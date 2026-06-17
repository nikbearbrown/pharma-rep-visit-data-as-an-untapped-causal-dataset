# CAJAL Figure Report — Chapter 12: From Propensity to Persuadables
_Mode: /scan silent · 2026-06-16 · source: chapters/12-from-propensity-to-persuadables.md_

## Density recommendation

This chapter carries five figure-worthy concepts but should ship **4 figures (1 Critical, 2 Important, 1 Supplementary)**. The chapter already embeds four `[DIAGRAM/TABLE]` markers; CAJAL confirms three of them as genuinely figure-worthy and consolidates the two list-comparison ideas. The conceptual spine is one idea — *baseline volume and causal responsiveness order the same population differently* — visualized first as a 2x2 structure (VG), then as a divergent-list allocation (PQ/MC), then proven as a Qini curve (PQ). The four-quadrant taxonomy is better rendered as a labeled 2x2 grid figure than as the chapter's table, because the sign-of-effect axis is the load-bearing distinction. Recommended density is moderate: the chapter is argument-dense and a reader needs the persuadables-vs-sure-things grid early, the Qini curve near the proof, and one allocation-divergence figure in between. Do not over-illustrate the constrained-optimization prose — the three gates are a list, not a process diagram, unless you split them (see Supplementary). One video candidate flagged for the allocation/Qini learning sequence; recommend, not auto-select.

## Figures

---

FIGURE 1 — Persuadables-vs-Sure-Things 2x2 (propensity axis × persuadability axis) · Critical · Conceptual grid (2x2) · VG

**BLOCK 1 — ILLUSTRAE PASTE:**
Render a single-panel two-axis quadrant grid on a white background. Draw a horizontal axis running left to right representing prescribing propensity from low to high, and a vertical axis running bottom to top representing persuadability from negative through zero to positive, with a clear zero gridline crossing the vertical axis partway up. Partition the plane into an upper band (positive effect), a middle zero band, and a lower band (negative effect), and into a left and right column by propensity. Place a distinct flat-color region marker in each zone: upper-right for the target population, upper-left for the population missed by propensity ranking, the high-propensity zero band for the wasteful population a propensity model over-selects, the low-propensity zero band for the inert population, and the lower band below the zero line for the population that responds negatively. Draw one diagonal directional arrow along the horizontal axis representing the propensity-based selection direction and a second directional arrow along the vertical axis representing the responsiveness-based selection direction, showing the two orderings as orthogonal. Keep all regions as flat filled blocks with thin one-point borders, no text, six zones maximum, aligned to the axes.

**BLOCK 2 — FULL SCOPE:**
- **[S]** Single-column, 89mm width, 300 DPI, vector, textbook figure.
- **[C]** Chapter-confirmed: horizontal axis = propensity P(NRx|X) low→high; vertical axis = persuadability τ(x) negative→positive with explicit zero line; five named populations — Persuadables (upper-right, target), Hidden Persuadables (upper-left, missed by propensity ranking), Sure Things (high-propensity zero band, the propensity trap), Lost Causes (low-propensity zero band), Sleeping Dogs (below zero line, detailing reduces prescribing). The two selection directions (NBA along propensity axis; Living Model along persuadability axis) and "overlap is low" are chapter-confirmed.
- **[O]** Two orthogonal axes; plane split into 6 labeled-by-position zones (3 vertical bands × 2 columns, with the negative band spanning width); one → arrow along each axis indicating selection direction; zero gridline marked on vertical axis.
- **[P]** Flat vector, white bg, Okabe-Ito only. Persuadables (active/target) #009E73; Sure Things (the trap, waste) #D55E00; Sleeping Dogs (backfire/blocking) #D55E00 with distinct treatment (use #CC79A7 composite if two simultaneous #D55E00 zones confuse — mark second backfire region #CC79A7); Hidden Persuadables #56B4E9 anchor; Lost Causes neutral gray; axes/arrows #000000; 1pt strokes. NO red-green pairing — keep #009E73 and #D55E00 separated by neutral bands.
- **[E] Exclusions:** no scatter of individual physician dots (this is a conceptual partition, not data); no quadrant where only two boxes appear (must show the negative band as a third row); no numeric tick values; no Dr. Alvarez/Dr. Okafor named markers; no correlation cloud implying a measured joint distribution; no τ formula rendered; no fourth axis.

**BLOCK 3 — NEGATIVE PROMPT:**
scatter point clouds, individual data dots, numeric axis ticks, named markers, correlation ellipse, two-box-only quadrant, regression line, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures (unless explicitly requested), visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

FIGURE 2 — Two target lists from the same population (propensity-ranked vs τ-ranked allocation divergence) · Important · Side-by-side comparison / allocation map · MC + PQ

**BLOCK 1 — ILLUSTRAE PASTE:**
Render two vertical stacked-bar columns side by side on a white background, each representing the same 200-physician population sorted two different ways. The left column is sorted top-to-bottom by descending propensity; the right column is sorted top-to-bottom by descending causal responsiveness. Shade each column's segments by responsiveness level so the eye sees where the high-responsiveness mass sits: in the left column the high-responsiveness segments scatter and most of the top is low-responsiveness fill; in the right column the high-responsiveness segments concentrate at the top. Mark the top fraction of each column with a bracket indicating the budget-receiving band. Draw thin connector lines between a few physicians' positions in the left column and their positions in the right column to show that the same individuals land in very different ranks, with most connectors crossing steeply. Keep segments as flat filled blocks with one-point borders, two columns only, no text, brackets and connectors as simple thin lines.

**BLOCK 2 — FULL SCOPE:**
- **[S]** Single-column, 89mm, 300 DPI, vector, textbook.
- **[C]** Chapter-confirmed: same 200-physician population; left = propensity-ranked NBA list, right = τ(x)-ranked persuadables list; shading by τ(x); top-decile budget band on each; the divergence (low overlap, same physicians different ranks). Budget concentration ("84% of budget" to top decile producing "3% of caused increment" on the propensity side vs "61% of caused increment" on the persuadables side) is chapter-confirmed as the contrast but must be shown as relative band area, not printed numbers.
- **[O]** Two vertical columns; per-segment shading by τ level; top-fraction bracket (⊐) on each; 3–5 connector lines linking same-physician positions across columns showing steep crossing.
- **[P]** Flat vector, white bg, Okabe-Ito. High-τ segments #009E73 (active increment); low-τ/Sure-Thing fill #E69F00 secondary; budget bracket #0072B2 dominant; connectors neutral gray 1pt; borders #000000 1pt.
- **[E] Exclusions:** no printed percentage numbers (show as band proportion); no more than two columns; no per-physician text IDs; no third "random" column (that belongs to the Qini figure); no axis with numeric prescribing values; no implied time axis; do not render all 200 connector lines — 3 to 5 representative ones only.

**BLOCK 3 — NEGATIVE PROMPT:**
printed percentages, numeric labels, third column, per-row text IDs, time axis, all-pairs connector spaghetti, random-baseline column, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures (unless explicitly requested), visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

FIGURE 3 — Qini curve: causal-responsiveness ranking vs propensity ranking vs random · Critical · Line chart (cumulative gain) · PQ

**BLOCK 1 — ILLUSTRAE PASTE:**
Render a single-panel line chart on a white background. The horizontal axis runs from zero to full population, representing the cumulative fraction of physicians treated, top-ranked first. The vertical axis runs from zero upward, representing cumulative caused incremental prescribing. Origin sits at the bottom-left corner with both axes starting at zero. Draw a straight diagonal reference line from the origin to the top-right corner representing random targeting. Draw a second curve that rises steeply early then flattens, sitting well above the diagonal, representing the causal-responsiveness ranking. Draw a third curve that tracks close to the diagonal, only modestly above it, representing the propensity ranking. Shade the area between the responsiveness curve and the diagonal as a flat fill to denote the Qini coefficient as area. Keep three lines plus one shaded region, flat colors, one-point strokes, axes starting from zero, no text, no numeric ticks beyond the zero origin.

**BLOCK 2 — FULL SCOPE:**
- **[S]** Single-column, 89mm, 300 DPI, vector, textbook.
- **[C]** Chapter-confirmed: x = cumulative fraction of physicians treated (top-ranked first); y = cumulative caused incremental prescribing, from zero; diagonal = random targeting; high curve = causal-responsiveness ranking (higher Qini); low curve = propensity ranking (typically low Qini on caused prescribing); shaded area = Qini coefficient (area between curve and diagonal). The "Qini gap is the argument" framing is chapter-confirmed.
- **[O]** Cartesian axes both from origin; diagonal reference; two ranking curves (one steep-then-flat well above, one near-diagonal); flat-fill area between the high curve and the diagonal.
- **[P]** Flat vector, white bg, Okabe-Ito. Responsiveness curve #009E73 (active); propensity curve #E69F00 (secondary); random diagonal neutral gray; Qini-area fill #009E73 at reduced opacity OR light hatch-free flat #56B4E9 anchor tint (choose one flat fill, no gradient); axes #000000 1pt.
- **[E] Exclusions:** y-axis must start at zero (no truncation); no numeric tick labels; no confidence-band ribbons (chapter notes CIs are thrown away by ranking — do not depict CIs as if shown); no ROC framing/labels; no second panel; do not draw the propensity curve below the diagonal (chapter says low, not negative).

**BLOCK 3 — NEGATIVE PROMPT:**
truncated y-axis, y-axis not starting at zero, numeric ticks, confidence band ribbons, ROC curve labels, second panel, curve below diagonal, gradient area fill, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures (unless explicitly requested), visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

FIGURE 4 — Constrained allocation objective: three load-bearing gates on a CATE ranking · Supplementary · Pipeline / filter diagram · MC

**BLOCK 1 — ILLUSTRAE PASTE:**
Render a single horizontal left-to-right filter pipeline on a white background. Begin at the left with a tall block representing the full physician population ranked by causal responsiveness. Pass it rightward through three sequential gate symbols, each drawn as a narrowing constriction with a perpendicular blocking bar on the rejected branch: the first gate is the capacity-and-frequency constraint, the second is the patient-welfare gate, the third is the educational-pathway gate. After each gate draw a short downward stub with a blocking bar end-cap representing the physicians removed at that stage, so the main flow narrows step by step. End at the right with a smaller block representing the deployable target list. Use a single straight directional arrow as the main spine and perpendicular blocking-bar end-caps for the three rejection branches. Keep to six components: source block, three gates, the rejection stubs as one visual class, and the final block. Flat colors, one-point strokes, no text.

**BLOCK 2 — FULL SCOPE:**
- **[S]** Single-column, 89mm, 300 DPI, vector, textbook.
- **[C]** Chapter-confirmed: input = physicians ranked by τ(x); three sequential constraints in chapter order — (1) rep capacity / call-frequency caps, (2) patient-welfare gate, (3) educational-pathway gate; each excludes (not deprioritizes) some physicians; output = deployable target list ("the only version that can actually ship"). Sleeping-dog/negative-τ and reciprocity-pathway exclusions are chapter-confirmed as what gets removed.
- **[O]** Horizontal spine with main → arrow; three gate constrictions in series; three ⊣ blocking-bar rejection stubs; narrowing flow; terminal block. Six components max.
- **[P]** Flat vector, white bg, Okabe-Ito. Source/flow #56B4E9 anchor; gates #0072B2 dominant; blocking rejection bars #D55E00 (⊣); deployable output #009E73 active; spine arrow #000000 1pt.
- **[E] Exclusions:** no objective-function equation rendered; no Markowitz/portfolio imagery; no budget numbers; no more than three gates; do not depict the gates as reorderable (order is chapter-fixed); no decision-diamond shapes (use constriction + blocking bar, not flowchart diamonds); [inferred] the relative narrowing magnitudes are illustrative, not chapter-quantified — keep proportions generic.

**BLOCK 3 — NEGATIVE PROMPT:**
equations, formula text, portfolio chart imagery, budget numbers, more than three gates, flowchart decision diamonds, reorderable layout, quantified narrowing, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures (unless explicitly requested), visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

## Video candidate pass

**FIGURE 2/3 (allocation divergence → Qini proof)**
- **Status:** Recommend (do not auto-select).
- **Criterion met:** ≥3 sequential causal stages and a sub-observable transformation — the learning target is *watching the same population re-sort* and seeing caused gain accumulate differently as you sweep down each ranked list. That sweep (treat top fraction → measure cumulative caused gain → trace the Qini area filling) is a transition mechanism, not a static state.
- **Reason:** The core pedagogical payload of the chapter — that propensity "buys conversions" while responsiveness "causes them" — is fundamentally a dynamic claim about what happens *as you walk down a ranked list at a fixed budget*. A short animation that sorts the population, sweeps a treatment cutoff down each list, and grows the Qini curve in lockstep makes the divergence legible in a way two static panels cannot. It is the one place in the chapter where motion teaches the mechanism rather than decorating it.
- **Suggested format:** ~15–20s silent loop, three beats: (1) population re-sorts from propensity order into responsiveness order (connector lines cross); (2) a budget cutoff sweeps down both lists; (3) the Qini curves draw themselves as the cutoff advances, the responsiveness area filling visibly larger than the propensity area. Okabe-Ito throughout, no text, y from zero.

**Honesty caveats reflected in this report:** The persuadables figures depict the claim that the Living-Model list differs *systematically* from the NBA list as a **hypothesis to be tested by a held-out Qini comparison (Ascarza transfer is not guaranteed)** — the Qini figure shows the *gap as the argument/test*, not as a settled empirical magnitude. The 84%/3% vs 84%/61% allocation figures are chapter illustrative values and are rendered as relative band area, not asserted measurements. Sleeping Dogs (negative τ) are shown as a distinct below-zero population requiring a *signed* estimate, consistent with the chapter's note that propensity is unsigned and that full four-quadrant separation may be unachievable on thin public data.
