# `_lib_` excerpt — Counterfactuals / SCM / abduction (curated from library)

**Sources:**
- `MD/science-the-book-of-why-the-new-science-of-cause-and-effect.md` (Pearl & Mackenzie) — Rung 3,
  counterfactuals, the "three steps" intuition. (Source body is OCR-degraded; this is a paraphrase of
  the well-known content, cross-checked against `living-models.md` Ch 9 and web search.)
- `MD/math-causal-inference-mit-press-essential-knowledge-series.md` — concise causal-inference framing.
- Companion in-repo: `pantry/living-models.md` Ch 9 ("The Counterfactual") is the *primary* clean
  treatment — prefer it. This file is a backstop + library pointer for Section F.

---

## The ladder (Pearl) and why Rung 3 needs an SCM

- **Rung 1 — association** (seeing): `P(Y | X)`. What observational data gives you.
- **Rung 2 — intervention** (doing): `P(Y | do(X))`. The do-operator simulates a physical intervention
  by replacing the structural equation for `X` with a constant, leaving the rest of the model intact.
- **Rung 3 — counterfactual** (imagining): `P(Y_{x'} | X=x, Y=y)` — what *would* `Y` have been for *this*
  unit had `X` been `x'`, given we actually observed `X=x, Y=y`. Counterfactual data can never be
  collected directly (the alternate world didn't happen), so Rung 3 requires a **structural causal model**
  (DAG + structural equations + exogenous noise terms `U`).

## Abduction → Action → Prediction (the three steps)

1. **Abduction.** Use the observed evidence to update beliefs about the exogenous noise `U`:
   `P(U | X=x, Y=y)`. This extracts the **case-specific idiosyncrasy** that distinguishes this unit from
   the population average.
2. **Action.** Apply `do(X = x')` — surgically replace the equation for `X` with the counterfactual value,
   leave all other equations unchanged.
3. **Prediction.** Run the modified model forward using the `U` recovered in step 1 to compute `Y_{x'}`.

The key move is abduction: a population intervention estimate ignores `U`; the counterfactual carries the
unit's specific quirks (its recovered noise) into the alternate world. This is what makes Rung 3
*individual-level* rather than population-level.

## Three granularities of counterfactual (from living-models.md Ch 9)

- **Population** = average causal effect; no abduction needed; reachable from Rung 2.
- **Subgroup** = conditional average treatment effect (CATE); partial conditioning; causal forests.
- **Individual** = full abduction; hardest and most valuable; the form Pearl's 3 steps target directly.

Caveat: the individual counterfactual is **point-identified only when the SCM is fully specified and the
noise is uniquely recoverable**. Nonlinearity / non-identifiable noise → only *partial* identification
(bounds). Honest practice: report bounds when you cannot point-identify.

## Necessity vs sufficiency (PN / PS)

- **PN** (probability of necessity) = but-for / tort standard: would `Y` *not* have happened had `X` not?
- **PS** (probability of sufficiency) = would `X` have produced `Y` on its own?
  Both are SCM-computable via abduction-action-prediction. Maps to the pharma "persuadables vs
  sure-things" frame: a *sure thing* has low PN (would prescribe anyway); a *persuadable* has high PN for
  the message.

## Why this matters for the pharma book

Ch 9's Rung-3 deliverable — "**which message for THIS physician next week**" — is a *pre-factual*
individual counterfactual on the parameterized rep-visit SCM. The population IV/DiD estimate (Ch 4, Ch 10)
is Rung 2; the targeting decision is Rung 3. Abduction is where the rep's elicited per-physician knowledge
(Ch 8) and the physician's own history enter as the "evidence" that recovers `U`.
