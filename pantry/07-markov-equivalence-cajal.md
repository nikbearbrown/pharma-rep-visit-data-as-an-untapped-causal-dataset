# CAJAL Figure Report — Chapter 07: Markov Equivalence: Why the Data Can't Decide
_Mode: /scan silent · 2026-06-16 · source: chapters/07-markov-equivalence.md_

## Density recommendation

For this text, I recommend **4 figures using Mechanistic density.** The chapter's spine is three formal objects — the three-node theorem (chain/fork/collider independence signatures), the CPDAG two-color map, and the peer-influence triad (contagion/homophily/reverse causation) — each of which is a structural claim the prose asserts but cannot let the reader verify by eye. These are textbook VG (verification-gap) figures: the whole pedagogical point is that the data *cannot decide* between visually distinct graphs that share a signature, and only side-by-side graph rendering makes "indistinguishable" legible. A fourth MC figure carries the three sources of outside knowledge (temporal/intervention/domain) that orient reversible edges. The chapter already contains three author-placed diagram comments; CAJAL's pass confirms all three and adds the orienting-evidence figure. No PQ figures — the chapter carries no percentages, magnitudes, or distributions; its "counting undirected edges" motif is conceptual, not quantitative.

## Figures

---

FIGURE 1 — The three-node theorem (chain / fork / collider) · Critical · Comparison panels · VG + MC

**BLOCK 1 — ILLUSTRAE PASTE**

Create a single-row comparison figure with three equal panels mapped to one shared horizontal baseline. Each panel contains exactly three nodes drawn as identical circles labelled by position only (left, middle, right), connected by directed edges. Panel one (chain): left node arrow to middle node, middle node arrow to right node, a straight left-to-right flow. Panel two (fork): middle node with two arrows fanning outward, one to the left node and one to the right node, so the middle sits above as a common source. Panel three (collider): left node and right node each send one arrow inward into the middle node, two arrowheads meeting at the center. Render the first two panels in identical neutral styling to signal they are indistinguishable; render the third panel's converging edges and central node in a distinct emphasis color to signal it is the one structure the data can orient. Use flat vector circles of uniform diameter, single-weight straight arrows with standard solid arrowheads, generous equal spacing between panels, white background. Leave all panels unlabeled for later typesetting of node names and independence signatures.

**BLOCK 2 — FULL SCOPE**

- **[S]** Single-column, 89mm width, 300 DPI, vector output, textbook default.
- **[C]** Three confirmed structures over three nodes: Chain (A→B→C), Fork (A←B→C), Collider (A→B←C). Confirmed relationships only: chain and fork share the independence signature A⊥C|B with A,C marginally dependent; collider has the opposite signature (A,C marginally independent, dependent given B). No nodes beyond the three positions per panel.
- **[O]** Three side-by-side panels, shared horizontal reference baseline; arrow direction is the entire content — chain flows left-to-right, fork fans out from center, collider converges into center. → for causal direction. Panels 1 and 2 visually matched (the equivalent pair); panel 3 visually set apart (the identifiable one).
- **[P]** Flat vector, white background, uniform 1pt strokes. Nodes neutral gray (#999999) for panels 1–2; collider central node and its edges in anchor Sky Blue #56B4E9 to mark identifiability. All arrows black #000000. No baked text.
- **[E]** Exclude: independence-signature equations as baked text; pharma variable names; any fourth node; curved/dashed edges; "equivalence class" set-bracket notation; the CPDAG; probability values; v-structure annotation text; legends.

**BLOCK 3 — NEGATIVE PROMPT**

baked independence-signature equations, pharma node names, fourth node, curved edges, dashed edges, set-bracket notation, probability numbers, legends, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

FIGURE 2 — The CPDAG two-color map (compelled vs reversible edges) · Critical · Systems diagram · VG

**BLOCK 1 — ILLUSTRAE PASTE**

Draw one small directed-acyclic graph of six nodes arranged as a clean layered layout, where the edges come in two visually distinct kinds. The first kind: directed edges drawn with a single solid arrowhead, rendered in one strong color, representing edges the data has settled and that point the same way in every member of the class. The second kind: undirected edges drawn as plain line segments with no arrowhead at either end, rendered in a contrasting warning color, representing edges whose direction the data leaves open. Mix roughly half directed and half undirected edges across the six nodes so the reader sees a graph that is partly committed and partly open. Keep nodes as uniform plain circles, evenly spaced, no crossing edges. Use flat vector style, uniform stroke weight, white background, ample whitespace. Leave the figure completely unlabeled — no node names, no "compelled/reversible" captions — so a designer can add the two-color legend and the count-of-undirected-edges annotation afterward.

**BLOCK 2 — FULL SCOPE**

- **[S]** Single-column, 89mm width, 300 DPI, vector, textbook default.
- **[C]** One CPDAG (completed partially directed acyclic graph / essential graph): directed edges = compelled (data settled, same in every class member); undirected edges = reversible (data silent, expert must orient). Six nodes [inferred — chapter does not specify a node count; six chosen to stay within component limit and show a mix]. The directed/undirected distinction and its meaning are chapter-confirmed.
- **[O]** Single-panel layered DAG, top-to-bottom or left-to-right; two edge encodings: solid-with-arrowhead (compelled) vs plain-segment-no-arrowhead (reversible). No crossing edges.
- **[P]** Flat vector, white background, 1pt strokes. Compelled directed edges in dominant Blue #0072B2 with solid arrowheads. Reversible undirected edges in blocking Vermillion #D55E00, no arrowheads. Nodes neutral gray #999999. No baked text.
- **[E]** Exclude: node names; the words "compelled"/"reversible"; the open-question count annotation (added in layout); v-structure highlighting; the three-node theorem panels; probability tables; any second graph; dashed styling for undirected edges (use plain segments, not dashes, to avoid implying weakness rather than non-orientation).

**BLOCK 3 — NEGATIVE PROMPT**

node names, compelled/reversible word labels, count annotations, dashed undirected edges, second graph, probability tables, v-structure highlights, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

FIGURE 3 — The peer-influence puzzle: three equivalent stories · Critical · Comparison panels · VG

**BLOCK 1 — ILLUSTRAE PASTE**

Build a three-panel comparison figure, panels side by side on a shared baseline, each showing a small causal graph that fits the same observed positive correlation. Panel one (contagion): two nodes, a single directed arrow from the left node into the right node. Panel two (latent homophily): three nodes, with one upper node sending two directed arrows downward — one into the left node and one into the right node — so the upper node is a shared hidden cause; draw this upper latent node as a circle with a distinct dashed outline to mark it as unobserved. Panel three (reverse causation): two nodes, a single directed arrow running the opposite way, from the right node back into the left node. Across all three panels keep node size, spacing, and arrow weight identical so the panels read as equally valid renderings of one dataset. Use flat vector circles, single-weight solid arrows with standard arrowheads, white background, equal inter-panel gutters. Leave unlabeled for later typesetting of node names and the shared-correlation caption.

**BLOCK 2 — FULL SCOPE**

- **[S]** Single-column, 89mm width, 300 DPI, vector, textbook default.
- **[C]** Three Markov-equivalent stories for the peer-exposure/prescribing correlation, all chapter-confirmed: Story A Contagion (peer_exposure → NRx); Story B Latent homophily (latent_trait → peer_exposure and latent_trait → NRx, latent node unobserved); Story C Reverse causation (NRx → peer_exposure). All three fit the same positive observed correlation.
- **[O]** Three side-by-side panels, shared baseline, matched scale; the only variable is edge configuration and direction. Latent node marked unobserved via dashed circle outline. → for causal direction.
- **[P]** Flat vector, white background, 1pt strokes. Observed nodes Sky Blue #56B4E9; latent/unobserved node neutral gray #999999 with dashed outline; arrows black #000000. Panels styled identically to signal indistinguishability. No baked text.
- **[E]** Exclude: pharma node names as baked text; the shared-correlation equation/scatterplot; independence-signature text; the rep observation that breaks the tie (that is Figure 4 / prose); the Shalizi–Thomas citation; confidence tags; any fourth story; arrowheads on the dashed latent-marking outline.

**BLOCK 3 — NEGATIVE PROMPT**

pharma node names, scatterplot, correlation equation, independence-signature text, citation text, confidence tags, fourth panel, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

FIGURE 4 — Three sources of outside knowledge that orient reversible edges · Important · Process flowchart · MC

**BLOCK 1 — ILLUSTRAE PASTE**

Construct a convergence flowchart: three labeled input streams on the left feeding into a single reversible edge on the right that starts undirected and ends directed. Draw three stacked source boxes on the left — top, middle, bottom — each connected by a horizontal arrow to a central junction. At the junction, show one edge between two nodes that transitions from a plain undirected line segment (before) to a directed arrow with a single arrowhead (after), illustrating that an outside input is what supplies the direction. From the directed result, show one short cascade arrow to a second nearby edge that also becomes directed, depicting propagation — a single oriented edge forcing a neighbor to orient. Keep the three input boxes uniform in size and the two-state edge transition the visual focus. Use flat vector rectangles for sources, plain circles for the graph nodes, single-weight arrows, white background, clear left-to-right reading order. Leave all elements unlabeled for later typesetting of the three source names and the before/after edge states.

**BLOCK 2 — FULL SCOPE**

- **[S]** Single-column, 89mm width, 300 DPI, vector, textbook default.
- **[C]** Three chapter-confirmed kinds of outside information that orient reversible edges: Temporal order, Interventions, Background/domain (rep) knowledge. One reversible edge transitioning undirected → directed once outside info is supplied. One propagation step (Meek) where the newly oriented edge forces a neighbor to orient. All confirmed in text.
- **[O]** Left-to-right convergence: three source boxes → junction → edge state transition (undirected to directed) → one propagation cascade arrow to a neighboring edge. → for flow and orientation; the undirected-to-directed change is the focal mechanism.
- **[P]** Flat vector, white background, 1pt strokes. Three source boxes secondary Orange #E69F00; the orienting/directed result active Bluish Green #009E73; the still-undirected starting state neutral gray #999999; nodes Sky Blue #56B4E9; arrows black #000000. No baked text.
- **[E]** Exclude: the names of the three sources as baked text; "Meek 1995"/"Verma–Pearl" citations; the CPDAG full graph (Figure 2); the peer-influence stories; Cartwright's slogan; faithfulness caveat; probability values; more than two graph edges in the transition zone.

**BLOCK 3 — NEGATIVE PROMPT**

source-name text, citation text, full CPDAG graph, slogan text, probability values, extra edges, text labels, words, gibberish letters, titles, captions, decorative borders, realistic textures, plastic wrap effects, drop shadows, gradient backgrounds, photographic elements, non-standard arrows, dual-headed arrows, hand-drawn styles, sketch lines, human figures, visual clutter, overlapping unaligned paths, fuzzy borders, watermarks, red-green color combinations, rainbow color scales, 3D perspective distortion

---

## Video candidate pass

FIGURE 1 — three-node theorem (chain/fork/collider)
Status: STATIC SUFFICIENT
Criterion met: none — the learning target is comparing fixed independence signatures across three static structures, not a transition.
Reason: Static side-by-side panels let the reader inspect and re-inspect the matched chain/fork pair against the distinct collider at their own pace; motion would add nothing the eye cannot already hold.

FIGURE 2 — CPDAG two-color map
Status: STATIC SUFFICIENT
Criterion met: none — a state map of what is settled vs open, not a process.
Reason: The figure is a snapshot of compelled vs reversible edges; nothing changes over time within it.

FIGURE 3 — peer-influence three equivalent stories
Status: STATIC SUFFICIENT
Criterion met: none — comparison of three fixed structures sharing one correlation.
Reason: The pedagogical force is that all three coexist as equal readings of the same data; static panels show coexistence better than a sequence, which would imply false ordering.

FIGURE 4 — three sources orienting a reversible edge (with Meek propagation)
Status: VIDEO CANDIDATE
Criterion met: Criterion 1 (transition mechanism is the learning target) and Criterion 2 (sequential causal stages — outside input → edge orients → neighbor orients).
Reason: Static arrows can only approximate Meek propagation cascading; an animation of an undirected edge snapping to directed and then forcing a neighbor to orient shows the *mechanism* of "one good rep observation orients more than one arrow," which the prose treats as the chapter's payoff. A static figure makes the reader mentally simulate the cascade.

Video candidates identified: 1. Recommended for production: Figure 4 — a short looping animation (or interactive step-through) of Meek propagation, because the cascade of one oriented edge compelling its neighbors is a transition mechanism that static arrows can only suggest. The other three figures are well-served by static treatment — formats noted above.
