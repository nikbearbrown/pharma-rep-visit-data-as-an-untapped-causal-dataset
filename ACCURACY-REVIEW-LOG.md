# Accuracy Review Log — Pharma Rep Visit Data as an Untapped Causal Dataset

Run: 2026-06-17 · method: domain-expert web verification of every `[verify]` tag and load-bearing empirical/citation/method/legal claim, chapter by chapter. Rule enforced: **never fabricate a citation, statistic, author, date, or source** — confirmed claims resolved with a real source; unconfirmable claims kept honestly flagged; no invented replacements.

## Outcome
- `[verify]` tags in chapters: **33 → 17**. Of the 17 remaining: 2 are honest `[verify — unconfirmed: …]` hedges, the rest are (a) pedagogical `[verify]` instructions inside the running-project exercise/prompt blocks (intentional) and (b) a small number of narrow Veeva schema-name items and synthetic-magnitude placeholders that are correctly hedged.
- Fabrications introduced: **0**. The previously-removed arXiv 2602.01483 was **not** reintroduced.

## Substantive corrections (caught and fixed against primary sources)
- **Ch3 — citation title error.** Lewis & Rao (2015, *QJE* 130(4)) is "**The Unfavorable Economics of Measuring the Returns to Advertising**" (the "Near Impossibility" title was an earlier working paper). Corrected; Gordon et al. 2019 *Marketing Science* added.
- **Ch5 — promotional-spend misattribution.** Schwartz & Woloshin 2019 (*JAMA* 321(1):80–96): the $17.7B→$29.9B figures are **total** US medical marketing; marketing to health professionals was $15.6B→$20.3B. Corrected; the unsupported "$40B pathway" dropped.
- **Ch10 — author-name error.** DiD practitioner's-guide author "Poet" → **Poe** (John Poe); "What's Trending in Difference-in-Differences?" (*J. Econometrics* 235(2)) cited correctly; Rambachan & Roth "Honest DiD" title/venue added.
- **Ch1 — Veeva share** anchored (80%+ of global pharma reps by 2022, up from ~⅓ in 2013; 19 of top-20 life-sciences companies).
- **Citation page/venue completions** across Ch4/5/6/7/8/10/11/13 — Imbens–Angrist 1994, Angrist–Imbens–Rubin 1996, Lee–McCrary–Moreira–Porter 2022 (tF, F≈104.7), Goodman-Bacon/Callaway–Sant'Anna/Sun–Abraham/de Chaisemartin–D'Haultfœuille, Verma–Pearl 1990 + Meek 1995, Shalizi–Thomas 2011, Vansteelandt–Daniel 2017, DeJong 2016, Wager–Athey 2018, Athey–Tibshirani–Wager 2019, Chernozhukov 2018, Ascarza 2018, Zhang 2008 (FCI), Caronia/Amarin/SIUU guidance — all verified exact before insertion.
- **Veeva CLM fields** (`Duration_vod`, `Reaction_vod`, `CLM_Opt_Type_vod`, `CLM_EXPLICIT_OPT_IN_vod`, `CLM_OPT_OUT_BEHAVIOR_vod`) verified against crmhelp/vaultcrmhelp; Vault CRM GA April 2024, legacy EOS Dec 31 2029 confirmed; dated-example hedge retained (schema velocity).

## Kept honestly flagged (unconfirmable; not asserted as fact)
3 narrow Ch2 Veeva schema-name items + 1 `View_Order_vod` split; Ch3 vendor magnitudes ("100M+ suggestions", "+25%"); Ch8 Korb & Nicholson exact page; Ch10/Ch11 synthetic-magnitude placeholders and the author's-own slide-sequence extension.

## Honesty positions preserved (correct stances, not errors)
Three-pathway split not point-identified (interventional, not naive products); weak-IV F>10 is legacy (Lee–McCrary–Moreira–Porter 2022); cohort instrument testable-not-assumed; dwell/reaction are post-treatment colliders; Markov equivalence → data identifies the class not the graph (Verma–Pearl); consent opt-out as Berkson/selection collider → FCI; educational pathway only legally drivable; regulatory routed to counsel; Risk 1 (proprietary data access) kept open.
