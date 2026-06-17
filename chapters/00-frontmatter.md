<!--
    00-frontmatter.md
    FRONT MATTER — everything that appears before the Introduction.
    Four sections, unnumbered: Title page, Copyright, Dedication, Preface.
-->

# Pharma Rep Visit Data as an Untapped Causal Dataset

### A Living Model Waiting to Be Built

**Humanitarians AI and Nik Bear Brown**

*Irreducibly Human · The AI+1 Series*

<!-- Humanitarians AI is the author of record. Nik Bear Brown is the series' founder and curriculum architect. -->

---

## Copyright

Copyright © 2026 Humanitarians AI and Nik Bear Brown. All rights reserved.

Published by Bear Brown, LLC, in partnership with Humanitarians AI, a 501(c)(3) nonprofit organization.

No part of this publication may be reproduced, distributed, or transmitted in any form or by any means — including photocopying, recording, or other electronic or mechanical methods — without the prior written permission of the publisher, except in the case of brief quotations embodied in critical reviews and certain other noncommercial uses permitted by copyright law.

Veeva, Vault CRM, IQVIA, Xponent, Symphony Health, Aktana, Indegene, and other product and company names are trademarks of their respective owners and are used here for identification and commentary only.

ISBN: [INSERT ISBN]

First edition: 2026

---

## Dedication

*For the reps who watched the physician's face — the knowledge the iPad never logged, and the reason this book exists.*

---

## Preface

This book began with a failure I have watched repeat in three different companies.

A data team is handed years of detailing telemetry — the per-slide records a field rep's iPad writes during sales calls. The data is gorgeous: seconds per slide, navigation order, a colored reaction button, all keyed to physician, rep, and territory, hundreds of thousands of traces a year. The team builds a dashboard, shows it to leadership, and someone says the words that doom the project: *now we understand what works.* They do not. They understand what *happened*. The dashboard can tell you a physician lingered on the safety slide and later prescribed more. It cannot tell you whether the slide *caused* the prescribing or whether the physicians who were always going to prescribe were also the ones who lingered. A million high-resolution observations of a confounded process give you a precisely estimated biased number. The precision feels like rigor. It is not.

That gap — between a *cool* dataset and a *causal* dataset — is the whole subject of this book.

**Why this book exists.** Pharmaceutical companies have been running a fifteen-year natural experiment on physicians and have never analyzed it causally. New clinical messages roll out in training cohorts, wave by wave, on a schedule set by hiring calendars and regional logistics rather than by anything about the physicians. One doctor hears the new safety-first deck this quarter because her rep certified early; a comparable doctor across town keeps hearing the old efficacy-first deck because his rep trains later. That is the structure of a quasi-experiment, and the telemetry recorded all of it. Meanwhile every next-best-action engine in the industry optimizes an engagement proxy — email open rate, click-through, visit acceptance — which is a Rung-1 question on Pearl's ladder. The budget question is Rung 2: *what would prescribing be if we deliberately assigned this message?* No amount of additional sophistication on the lower rung reaches the upper one. The data to answer the right question has been accruing, unexamined, for over a decade.

**Why now.** Two things became usable at the same time. The causal-inference toolkit — staggered difference-in-differences that survives the two-way-fixed-effects critique, instrumental variables, causal forests, double machine learning, latent-variable discovery — matured into software a capable data scientist can actually run. And agentic AI made it practical to build the missing piece: a bot that interviews reps in their own language and extracts the structural knowledge the data can never contain — the *why* behind the forty-seven seconds on the safety slide. The methods identify a class of graphs; the reps orient the graph the data cannot decide. Both halves are finally within reach of one person with a laptop.

**Why us.** Humanitarians AI runs a Fellows program — at any given time, one to two hundred Fellows across data science, design, engineering, the sciences, and business — and this book is that program's method written down on a single dataset. The Fellows pipeline, under the curriculum architecture of Nik Bear Brown, is where the exercises in this book were pressure-tested. Nik's through-line is computational skepticism: using AI to extract and draft while keeping the human responsible for every claim that reaches a decision. That discipline is the spine of the running project here.

> **The innovation-lab offer.** Bear Brown, LLC and Humanitarians AI together operate as an *external innovation lab* for companies. We research and prototype the AI that might help you — scope the question, build a proof of concept on public or synthetic data, test whether it actually holds — and then work alongside your own team to implement anything that proves promising. Because Humanitarians AI runs one to two hundred Fellows at a time across many majors, a project that needs extra hands for a few weeks can be staffed on a per-hour basis. The commitment is low for you; the evidence is high for the decision. You see whether the idea works *before* building a team around it.
>
> This book is that lab's method on one dataset. The contract that makes it safe is a firewall: Fellows work only on public Open Payments data and a synthetic rep-visit dataset with known ground truth, while the partner replicates anything that proves promising on its proprietary CRM data internally. The Chapter 13 handoff brief — a test/build/kill recommendation with evidence level, assumptions, compliance flags, and an explicit statement of what proprietary replication remains — is the document that crosses that firewall. The book is built to be worked with a pharma martech partner. We name the firewall, not any confidential specifics.

**What this book does not do.** It is not a general causal-inference textbook — Pearl, Imbens, and Angrist already wrote those, and we lean on them rather than reproduce them. It is not a Veeva administration manual; every field name here is an example of a *kind* of field, not a current API guarantee, and the platform is mid-migration as we write. It does not use proprietary data, and it never will: everything runs on public Open Payments and Medicare Part D data, or on a synthetic dataset whose ground truth you can check your methods against. And it does not give legal advice — the regulatory landscape is flagged and routed to counsel, never adjudicated here.

What it does is teach one capable data scientist to take a dataset everyone has and nobody has questioned correctly, and to build — honestly, with every assumption named and every dead end shown — a living causal model of how a message actually lands on the physician it was aimed at.

— *Humanitarians AI and Nik Bear Brown*

## References (fact-check pass)

1. Veeva Systems, "Veeva Announces First Customer Win and April 2024 Availability for Vault CRM" (2023); Veeva CRM Help, End of Support (crmhelp.veeva.com) — Vault CRM GA April 2024; legacy Veeva CRM end-of-support December 2029. [CONFIRMED — supports "platform mid-migration."]
