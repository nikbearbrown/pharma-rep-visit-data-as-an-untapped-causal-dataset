# Pharma Rep Visit Data as an Untapped Causal Dataset

### A Living Model Waiting to Be Built

**Authors:** Humanitarians AI and Nik Bear Brown
**Series:** Irreducibly Human · The AI+1 Series
**Publisher:** Bear Brown, LLC, in partnership with Humanitarians AI (a 501(c)(3) nonprofit)

## What this is

For fifteen years, pharmaceutical companies have run a massive natural experiment on physicians and never analyzed it causally. Every rep visit, an iPad silently logs which slides were shown, for how long, in what order, with a reaction button tapped per slide — all keyed to physician, territory, rep, and training cohort, and joinable to downstream prescribing. New clinical messages roll out in training cohorts on a schedule set by hiring logistics rather than by anything about the physicians, which generates quasi-experimental variation nobody has used. This book is an applied, single-dataset causal practicum: it teaches a data-capable reader to turn that "cool" dataset into a *causal* one — to ask the do-question the budget is really asking, identify message effects from the cohort rollout, and orient the parts of the causal graph the data can never decide by eliciting structural knowledge from the reps themselves. It runs entirely on public Open Payments / Medicare Part D data and a synthetic rep-visit dataset with known ground truth. Nothing proprietary.

## Who it's for

The data-capable Fellow or practitioner: comfortable with Python and basic statistics, **not** assumed to be a pharma insider, a Veeva administrator, or a senior causal-inference researcher. All causal machinery — Pearl's ladder, the do-operator, instruments, DiD, colliders, Markov equivalence, mediation, causal forests, double ML, persuadables — is built from the ground up.

## The innovation-lab offer

Bear Brown, LLC and Humanitarians AI operate as an **external innovation lab**: we scope the AI question, build a proof of concept on public or synthetic data, test whether it holds, and then work alongside your team to implement what proves promising. With one to two hundred Humanitarians AI Fellows across many majors, a project that needs extra hands for a few weeks can be staffed on a per-hour basis — low commitment for you, high evidence for the decision. This book is that lab's method on one dataset, with a public-data / IP firewall (Fellows work on public + synthetic data; the partner replicates on proprietary CRM data internally) and a Chapter 13 handoff brief as the contract that makes it safe.

## The book (three acts)

**Act I — The Untapped Dataset**
1. The Experiment Nobody Knows They're Running
2. What the Data Actually Is
3. Stuck on Rung 1
4. The Natural Experiment in Every Training Rollout

**Act II — Building the Causal Model**
5. The Causal Graph and the Three Pathways
6. The Collider Trap (and the Consent Collider)
7. Markov Equivalence: Why the Data Can't Decide
8. The Causal Interview Bot
9. From Priors to a Living Model

**Act III — Running It**
10. Project 1: Staggered Difference-in-Differences on Message-Variant Transitions
11. Projects 2 & 3: Meals (Causal Forest) and Slide Sequencing (Double Machine Learning)
12. From Propensity to Persuadables
13. Consent, Compliance, and the Handoff

## Repo usage

```
chapters/        ← markdown source (00-frontmatter, 00-introduction, 01–13, 99-back-matter)
images/          ← PNGs used by the EPUB (generated from SVG)
d3/              ← interactive D3 HTML figures (browser-runnable)
pantry/          ← source-essay spine and scratch fragments
SCRIPTS/         ← svg-to-png conversion and build helpers
```

- **Synthetic-data-first.** Work every exercise on the synthetic dataset (known ground truth) first, so you can confirm a method recovered the answer before touching real public data.
- Each chapter ends with a **five-part running-project exercise block** that builds **The Causal Interview Bot** — when to use AI, when not to, a hands-on LLM build, a CLI build, and an AI-validation audit.
- Each chapter also closes with *What would change my mind* and *Still puzzling* sections.

Build:

```bash
npm install        # first time only
./build.sh         # output goes to output/ (gitignored)
```

## About Humanitarians AI & Bear Brown, LLC

**Humanitarians AI** is a 501(c)(3) nonprofit (EIN 33-1984805), founded in 2019 in Boston, running the Fellows Program and the *Irreducibly Human* curriculum. **Bear Brown, LLC** is the publisher and the lab's commercial arm. Together they research and prototype AI for partners on a public-data / synthetic-data firewall and hand off what proves promising.

- [humanitarians.ai](https://www.humanitarians.ai/)
- [bearbrown.co](https://www.bearbrown.co/)
- [info@humanitarians.ai](mailto:info@humanitarians.ai)

## Copyright

Copyright © 2026 Humanitarians AI and Nik Bear Brown. All rights reserved. Published by Bear Brown, LLC, in partnership with Humanitarians AI. See [LICENSE.md](LICENSE.md) for terms. Veeva, IQVIA, and other product names are trademarks of their respective owners.

## A note on Medhavy

These are Kindle / online editions, designed for integration with **Medhavy** (मेधावी, "intelligent") — an AI-powered intelligent-textbook system that turns these chapters into adaptive practice. Come learn something with us: [medhavy.com](https://www.medhavy.com/).

## Data ethics

This book and repo use **public and synthetic data only** — CMS Open Payments, Medicare Part D, and a synthetic rep-visit dataset with known ground truth. No proprietary CRM telemetry, no real physician identifiers, and no partner-specific field mappings or performance figures appear here. Proprietary replication happens behind the firewall, on the partner's side. Redaction is a reflex, not a policy paragraph.
