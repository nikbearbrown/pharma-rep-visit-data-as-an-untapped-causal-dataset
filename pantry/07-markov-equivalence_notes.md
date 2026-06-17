# Chapter 7 Notes — Markov Equivalence: Why the Data Can't Decide

*Research-gathering notes for Week 7 (Act II). Markov-equivalence classes; the Verma–Pearl result
(observational data identifies the class, not the graph); the peer-influence three-story example; why
expert/rep structural knowledge is mathematically necessary to orient edges. Notes only — not chapter
prose.*

Primary source: `pantry/rep-visit-as-causal-dataset_source.md` ("Markov equivalence — why interviews
are mathematically necessary"). TIKTOC Week 7 entry.

---

## A. Foundations

**Markov equivalence class — definition + graphical test.** Two DAGs are **Markov equivalent** iff they
encode the same set of conditional-independence (CI) relations over the observed variables. Because
observational data reveals *only* CI relations, data can identify a DAG **only up to its equivalence
class** — never the unique graph. The operational test (Verma & Pearl 1990): **two DAGs are Markov
equivalent iff they have (a) the same skeleton (undirected adjacencies) and (b) the same set of
v-structures / unshielded colliders** (a triple i→k←j with i,j non-adjacent).

**The Verma–Pearl result (the chapter's spine).** Observational CI structure fixes the equivalence
class, not the graph. So whenever two orientations are Markov equivalent, **no amount of observational
data can distinguish them.** The canonical three-node case *is* the whole lesson:
- **Chain** A→B→C: signature A⊥C | B; A,C marginally dependent.
- **Fork (common cause)** A←B→C: *same* signature — A⊥C | B; marginally dependent.
- Chain and fork share skeleton and have *no* v-structure → **Markov equivalent.** The data literally
  cannot tell "A causes C through B" from "B is a common cause of A and C."
- **Collider** A→B←C: *opposite* signature — A⊥C *marginally*, but A,C become dependent given B. It has
  a v-structure → sits in its **own** class → **is** identifiable from data.

**The CPDAG (essential graph) — what the data settled vs. left open.** An equivalence class is uniquely
represented by a **completed partially directed acyclic graph (CPDAG)** / essential graph. **Directed
edges are *compelled*** — oriented the same in every member (the data *did* settle these). **Undirected
edges are *reversible*** — they flip across members (the data *cannot* orient these). Every undirected
edge is a place where an expert must supply the arrow. (Chickering 2002; Andersson, Madigan & Perlman
1997; orientation propagation via Meek 1995.)

**Why expert/structural knowledge is *mathematically* necessary (not a nicety).** Within a class the
orientation of reversible edges is *empirically underdetermined* — observational data is exhausted.
Ties break only with information from *outside* the data: (i) **background/domain knowledge**, (ii)
**temporal order** (causes precede effects), (iii) **interventions/experiments**. Meek (1995) shows how
such knowledge is injected as edge constraints and then *propagated* by orientation rules — a single
expert-oriented edge can cascade into more orientations. The broader principle: causal discovery from
observational data *requires* assumptions (causal Markov, faithfulness, causal sufficiency — Spirtes,
Glymour & Scheines 2000); Cartwright's slogan "**no causes in, no causes out**" — you cannot derive
causal conclusions from purely associational premises. **This is the book's thesis in miniature:** the
rep interviews are the *source* of structural information, not supplementary data.

**Common misconception (load-bearing — TIKTOC learner profile #3).**
> "If a tool's DAG fits the data, it's the right DAG."
**Wrong.** Many DAGs fit the same data equally well — the whole equivalence class. A tool that displays
*one* oriented DAG is hiding the reversible edges it guessed. "Fits the data" = "is in the class," not
"is the causal truth."

**Worked example (on this dataset — process incl. dead end → lesson → limit).**
The book's peer-influence puzzle is the three-node theorem wearing a lab coat. A physician's exposure
to peer influence and her prescribing are correlated. **Three Markov-equivalent stories:**
- (A) **Contagion / peer influence → prescribing** (chain).
- (B) **Latent homophily / common cause:** an underlying trait (openness, practice setting) drives both
  tie-formation/peer-exposure *and* prescribing (fork).
- (C) **Reverse:** high prescribers *become* influencers / get perceived as peers (reverse chain).
1. **Dead end:** run causal discovery (PC/GES) on the observational panel, read off the single returned
   DAG, and report "peer influence causes prescribing." The algorithm returned a *class*; the displayed
   arrow on that edge was a coin flip among A/B/C.
2. **Lesson:** Shalizi & Thomas (2011) *prove* contagion, latent homophily, and individual-covariate
   effects are **generically confounded** in observational social-network data — observationally
   indistinguishable without strong parametric assumptions or adequate covariates. The confounding is
   *generic*, not an artifact of this dataset.
3. **The orientation:** the experienced rep supplies the outside information — "I've watched Dr. Johnson
   switch within two weeks of a colleague switching; I've *never* seen it run the other way." That is
   temporal + structural knowledge that orients the edge (A over C), and via Meek-propagation may orient
   others. The interview is the *source* of the arrow.
4. **Limit:** rep knowledge is fallible and variable in quality; the orientation is a *prior* with
   confidence, not certainty (feeds the annotated prior DAG of Ch 8 and the parameterized SCM with
   provenance/confidence of Ch 9). Bridge: "so how do you extract it?" → Ch 8 (Causal Interview Bot).

---

## B. Examples + failure case

- **The three-node chain/fork/collider** is the irreducible teaching example — every member of the
  family (Verma-Pearl, the CPDAG, faithfulness) can be shown on it. Borrow Shalizi's CMU lecture-notes
  framing (chain and fork equivalent, collider not).
- **Peer influence / homophily** (Shalizi & Thomas 2011) — the book's exact structure; cite as the
  rigorous proof that the three stories are confounded, not just "hard to tell apart."
- **Failure case (the chapter's sting).** A vendor NBA tool ships a "causal" DAG of physician influence
  networks and recommends targeting based on the displayed arrows. Because the influence↔prescribing
  edge is reversible (homophily vs contagion vs reverse all fit), the displayed orientation is an
  artifact of the algorithm's tie-breaking, not the data. Acting on it — targeting "influencers" who are
  actually just *high prescribers* (story C) — wastes budget on sure-things (connects to Ch 12,
  persuadables-vs-sure-things). The data never could have justified the arrow.

---

## C. Connections

- **← Ch 6 (colliders):** v-structures (colliders) are *exactly* what distinguishes equivalence classes
  — the one three-node structure the data *can* orient. Ch 6's collider concept is the load-bearing
  mechanism of Ch 7's equivalence test. Deliberate dependency (TIKTOC: Ch 7 depends on 5,6).
- **→ Ch 8 (Causal Interview Bot):** Ch 7 proves *why* rep knowledge is necessary (orient reversible
  edges); Ch 8 is *how* to extract it (KEBN elicitation). The necessity result motivates the entire
  elicitation apparatus.
- **→ Ch 9 (parameterized SCM):** elicited edge orientations enter the SCM as prior structure with
  provenance + confidence; Ch 7's "reversible edge" becomes Ch 9's "edge oriented by rep knowledge,
  confidence X."
- **→ Ch 6 (FCI):** FCI's PAG output is the latent-aware analogue of the CPDAG — it leaves circle
  marks/bidirected edges precisely where direction is undetermined. Ch 7 (CPDAG, causally sufficient)
  and Ch 6 (PAG, latent-aware) are the two faces of "the data identifies a class, not a graph."
- **→ Living Model / Pearl's ladder:** orienting edges is the Rung-2/3 structural commitment a Living
  Model needs and a Rung-1 predictor never makes (`living-models.md`).
- **Assessment tie:** the own-thread bonus (+5) is earned when the AI Use Disclosure "names one edge
  orientation or assumption that required human/rep structural knowledge no dataset could supply" —
  literally Ch 7's lesson operationalized in grading.

---

## D. Current state / key refs / last-3-yr

**Markov equivalence (foundational):**
- **Verma & Pearl (1990), "Equivalence and Synthesis of Causal Models," *Proc. UAI 6*, 220–227** — the
  skeleton + v-structures theorem; the result the chapter is named for.
- Andersson, Madigan & Perlman (1997), "A Characterization of Markov Equivalence Classes for Acyclic
  Digraphs," *Annals of Statistics* 25(2):505–541 (rigorous class characterization; essential graph).
- Frydenberg (1990), "The chain graph Markov property," *Scand. J. Statistics* 17:333–353 [verify
  pages] (contemporaneous equivalent characterization for chain graphs).

**CPDAG / learning classes / orientation:**
- Chickering (2002), "Learning Equivalence Classes of Bayesian-Network Structures," *JMLR* 2:445–498
  (transformational characterization; GES over equivalence classes). Conf. version Chickering (1995),
  UAI.
- **Meek (1995), "Causal Inference and Causal Explanation with Background Knowledge," *Proc. UAI 11*,
  403–410** — Meek's orientation rules; how *background knowledge* orients additional edges (the formal
  bridge from "rep says X→Y" to a more-oriented graph).

**Necessity of assumptions / underdetermination:**
- Spirtes, Glymour & Scheines (2000), *Causation, Prediction, and Search*, 2nd ed., MIT Press (causal
  Markov, faithfulness, causal sufficiency — discovery requires assumptions).
- Cartwright (1989), *Nature's Capacities and Their Measurement*, Oxford UP — "no causes in, no causes
  out." [verify exact page for the slogan]

**Identification under equivalence (latent-aware, recent):**
- Jaber, Zhang & Bareinboim (2018/2019), identification of causal effects under Markov equivalence /
  with PAGs, arXiv:1812.06209 [verify exact title/venue] — extends do-calculus-style identification to
  equivalence classes / PAGs; the bridge between Ch 6 (FCI/PAG) and Ch 7 (equivalence) for effect
  *identification* (not just structure).
- Spirtes, Meek & Richardson on latent variables and selection bias, arXiv:1302.4983 [verify] —
  foundational on discovery under latents/selection (flagged from prior stub).

**The peer-influence identification result (last-15-yr, directly applicable):**
- **Shalizi & Thomas (2011), "Homophily and Contagion Are Generically Confounded in Observational Social
  Network Studies," *Sociological Methods & Research* 40(2):211–239** (arXiv:1004.4704) — proves the
  three-story confounding is generic. *The empirical anchor for the chapter's example.*

---

## E. Teaching

- **This is the abstract chapter** (TIKTOC "hard chapters"). Ground every claim on the three-node
  chain/fork/collider — show the *same* correlation arising from chain and fork, then reveal the
  collider as the lone exception. Do it before any formal definition.
- **Make "the tool shows one DAG" the villain:** display a discovery algorithm's single output, then
  reveal the equivalence class behind it. The misconception ("fits the data = right DAG") dies on contact.
- **CPDAG as a two-color picture:** directed = "data settled it"; undirected = "expert must orient it."
  Every undirected edge is a Ch 8 interview question. This visual makes the necessity of rep knowledge
  concrete.
- **The peer-influence walk-through** is the emotional core: three plausible stories, identical data, a
  rep's lived observation as the tiebreaker. Pair with Shalizi–Thomas to show it's a theorem, not a
  hand-wave.
- **Connect to grading:** the +5 own-thread bonus rewards naming an edge only rep knowledge could
  orient — Ch 7's whole point as an incentive.

---

## F. Library files (in `pantry/`)

- `_lib_ai-ml-assisted-adjustment.md` — **highly relevant.** Discusses Markov equivalence, collider,
  Berkson, the expert-interview → graph-audit pipeline, and estimability classification — essentially a
  design conversation for the same architecture this book builds. Best internal source for "why expert
  elicitation orients what data cannot."
- `_lib_science-the-book-of-why-the-new-science-of-cause-and-effect.md` — causal discovery, Verma-Pearl
  lineage, equivalence intuition (Pearl/Mackenzie, narrative — Pearl was Verma's advisor; good for the
  origin story).
- `_lib_ai-applied-causal-inference-powered-by-ml-and-ai.md` — causal discovery, Spirtes, do-calculus,
  Markov-equivalence machinery (the technical source).
- `_lib_math-causal-inference.md` / `_lib_math-causal-inference-mit-press-essential-knowledge-series.md`
  — causal discovery + do-calculus, conceptual depth.
- (Legacy MD library has only thin direct coverage of "Markov equivalence" as a phrase — mostly inside
  ml-assisted-adjustment and book-of-why; the formal Verma-Pearl/Meek/Chickering/Shalizi-Thomas refs are
  web-sourced above and must be cited directly.)

---

## G. Gaps / flags

- **[verify]** Frydenberg (1990) exact page range (cited from secondary literature).
- **[verify]** Cartwright's exact source page for "no causes in, no causes out" (attribute to Cartwright
  1989, *Nature's Capacities and Their Measurement*).
- **[verify]** Verma & Pearl (1990) page numbers (220–227 per dblp; confirm against UAI proceedings).
- **Defensible-but-open claim (TIKTOC Part 11):** "rep knowledge can orient causal edges" is defensible
  *in principle* (Verma-Pearl + Meek make it mathematically necessary that *some* outside knowledge
  orient edges). The *quality* of rep elicitation is the open empirical question — the chapter should
  assert the necessity firmly and the elicitation-quality as the live research frontier (handed to
  Ch 8). Do not let "necessary in principle" read as "reps are always right."
- **Faithfulness caveat:** the whole "data → equivalence class" story assumes faithfulness (no
  cancelling paths). Worth one flag — it is an assumption, occasionally violated, and the chapter
  shouldn't present the equivalence test as assumption-free.
- **Stable content:** unlike the Veeva field names, the Markov-equivalence/Verma-Pearl material is
  mathematically stable (low aging risk per TIKTOC Part 11) — safe to teach as settled.
