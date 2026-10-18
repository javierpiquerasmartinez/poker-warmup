# Spec: Poker Warmup Gym — MVP (Postflop + Preflop drills)

Status: ready-for-agent

Origin: settled via a grilling session; every decision below was explicitly confirmed by the owner. Follows ADR-0001 (precalculated flop equities) and the vocabulary in `GLOSSARY.md` (Spot, SRP, RFI, Chart, Subaction, Hand Class, Combo, Range, Hero, Villain).

## Problem Statement

Before a poker session, a player wants a fast mental warmup: a few minutes of quick tactical decisions to sharpen board-reading and chart recall. Existing tools (solvers, Flopzilla) are heavyweight, slow to boot, and not built as a drill loop. The owner has no lightweight app that turns their personal 6-max charts into a rapid-fire warmup they can run on a phone right before sitting down.

## Solution

A tiny SPA with two drill modules sharing one visual language:

- **Postflop drill** — one fixed Spot (single-raised pot, Hero at BTN vs Villain at BB). The app deals a random flop; the user classifies who holds the range advantage (Hero / Neutral / Villain) in three buttons. Instant feedback shows whether they were right *and* the actual range-vs-range equities on that flop, then the next flop deals immediately.
- **Preflop drill** — the app rolls a fully random scenario from the MVP Spot pool (`RFI Btn` and `BB vs Btn` in SRP) plus a combo dealt with real deck weighting. The user picks the Subaction from the buttons the current Spot allows. Feedback reveals the Chart's 13×13 grid with the Hand Class colored by its canonical Subaction.

Sessions are an endless loop the user closes whenever they want, with live counters (completed spots, accuracy, streak) and a closing summary on exit. No accounts, no backend, no persistence: everything lives in memory and local data files.

## User Stories

1. As a poker player preparing for a session, I want to open the app and land directly on a drill, so that I start warming up without any setup or login.
2. As a poker player, I want to choose between Postflop and Preflop drills with one tap, so that I can warm up the skill my session demands today.
3. As a postflop-module user, I want the app to deal a random flop for the fixed BTN-vs-BB SRP Spot, so that I practice reading textures without hand-picking spots.
4. As a postflop driller, I want exactly three answer buttons (Hero advantage / Neutral / Villain advantage), so that classification takes under a second.
5. As a postflop driller, I want instant visual validation of my answer, so that I build intuition with immediate correction.
6. As a postflop driller, I want the feedback to show both ranges' equities on the flop (e.g. "BTN 54.3% / BB 45.7%"), so that I learn the magnitude of each advantage, not just the class.
7. As a postflop driller, I want the next flop to appear automatically after feedback, so that the drill keeps a fluid tempo.
8. As a poker player, I want flops dealt uniformly across the full 19,600 possible flops, so that no texture family is systematically favored.
9. As a preflop driller, I want the app to roll a random Spot from the MVP pool (`RFI Btn` or `BB vs Btn`), so that I drill mixed situations like a real session.
10. As a preflop driller, I want hands dealt with true deck weighting (each Combo out of the 1,326 equally likely, so `AKo` shows up proportionally more than `AKs`), so that the drill matches real table frequencies.
11. As a preflop driller, I want the app to show only the Subaction buttons the current Spot allows (`FOLD | RAISE` for RFI, `FOLD | CALL | 3BET` for defense), so that I never face impossible choices.
12. As a preflop driller, I want instant validation against the Chart's canonical Subaction for my Combo, so that I confirm or correct my chart recall.
13. As a preflop driller, I want the feedback to reveal the 13×13 Chart grid with the correct Subaction's cells shaded, so that each answer doubles as chart review.
14. As a warmup user, I want live counters for completed Spots, accuracy, and current streak, so that I can feel momentum without hunting for stats.
15. As a warmup user, I want a final summary when I stop the loop (accuracy, spots done, best streak), so that I close the warmup knowing where I stand.
16. As a warmup user, I want no history stored anywhere persistent, so that every warmup is a clean slate and there is nothing to have to manage.
17. As the app owner, I want my Charts shipped as one data file per Spot, so that chart maintenance stays decoupled from app logic.
18. As the app owner, I want each Chart cell to declare exactly one canonical Subaction per Hand Class, so that validation is unambiguous and binary.
19. As the app owner, I want the postflop equities derived from the same Charts (the non-`FOLD` entries), so that there is a single source of truth for ranges.
20. As the app owner, I want a one-command regeneration of the 19,600 flop equity table whenever Charts change, so that data updates are trivial and safe.
21. As the app owner, I want BTN ≥ 52% / ≤ 48% / else-Neutral thresholds as named constants, so that tuning classification does not require code hunting.
22. As the app owner, I want error handling such that a malformed or missing Chart file fails loudly at load, so that I never silently drill against wrong data.
23. As the app owner, I want the preflop Spot pool defined as data (not hardcoded logic), so that adding Spots in v1.1 is additive.
24. As a mobile player, I want the entire app thumb-friendly on a phone, so that I can warm up in a hotel lobby before a session.
25. As a Spanish-speaking player, I want the UI in Spanish, so that the warmup reads effortlessly.
26. As a warmup user, I want answer interactions to feel instant (no loading spinners between questions), so that the drill rhythm is never broken.
27. As the app owner, I want all domain tests plain Vitest with no browsers, so that CI can gate correctness cheaply.
28. As a returning owner, I want placeholder Chart files clearly marked TODO, so that I know exactly where to paste my real charts at development time.

## Implementation Decisions

- **Stack**: React + Vite SPA in TypeScript, Spanish UI. No backend, no persistence layer, no router — navigation is a two-tab toggle (Postflop / Preflop).justified by grilling (nothing needs SSR; FSD suggested Next.js only as an option).
- **Domain vocabulary**: modules and types follow `GLOSSARY.md` terms (Spot, Chart, Subaction, Hand Class, Combo, Range, Hero, Villain). The 169-cell Chart grids resolve at load time into per-Combo entries.
- **Range derivation**: a Range is never stored independently; it is computed as the set of non-`FOLD` entries of the relevant Chart. The postflop Range comes from the `RFI Btn` Chart; the BB Flat Range from the `BB vs Btn` Chart. (Single source of truth, per grilling.)
- **Data files**: one JSON file per MVP Spot holding a 169-entry map of Hand Class → Subaction. Chart content ships as clearly marked placeholders for the owner to paste real charts during development.
- **Flop equities (ADR-0001)**: a dev-time Node script (Monte Carlo over random runouts, combo-weighted sampling of both Ranges) generates one static JSON asset covering all 19,600 flops, storing both ranges' equities. An npm script regenerates it in a single command. Thresholds BTN ≥ 52% / ≤ 48% / else Neutral are named constants in code.
- **Generation of deals**: flops are drawn strictly from a 52-card deck (uniform across all 19,600 possible flops). Preflop hands are drawn as two random distinct cards without replacement from the deck — which gives Combo weighting (pair 6 / suited 4 / offsuit 12) by construction.
- **Preflop Spot pool as data**: the pool of Spots (currently 2) and each Spot's permitted Subaction labels live in the Chart data, not in branching code.
- **Feedback content**: postflop reveals both equities; preflop reveals the full 13×13 grid for the current Spot with the correct Subaction shaded. Postflop module has no position-picker (Hero is always BTN in MVP).
- **Session loop**: endless; each module maintains completed Spots, hits, misses, best and current streak; exit produces a summary view. Counters are in memory only.
- **i18n**: Spanish only; copy lives centrally to keep it swappable, not a full i18n framework.
- **Strictness the app errs on**: Chart/equity data validation at load fails loudly rather than defaulting.

## Testing Decisions

- **What makes a good test here**: only external behavior at the seam — deterministic scenarios through seeded RNG and minimal fixture data, asserting inputs to outputs (question dealt, verdict, counters, summary), never internal call stacks or rendering internals.
- **Primary seam — the session engine** (confirmed with owner): a pure state machine per drill module that composes the generators (flop deal, Spot + Combo roll), the validators (flop classification, Subaction lookup), and counters behind one interface producing events/questions/feedback. Both drills' whole user stories are tested through it: seeded streams of questions with fixture Charts and a fixture equity table.
- **Secondary seam — the equity generation script**: build-time Node script tested separately with small anchors (AA vs KK ≈ 81/19; selected full-range vs empty-range cases) that pin the Monte Carlo math. This seam lives with the script, not the SPA.
- **Not direct seams**: Chart JSON loading (exercised with loud-failure fixture tests through the session engine's data contract) and React components (thin render; smoke-level at most).
- **Prior art**: none — greenfield. Vitest is the runner for everything, including the Node script.

## Out of Scope

- Any Spot beyond `RFI Btn` and `BB vs Btn` (3-bet defense, cold 4-bets, other positions) — the Subaction taxonomy from the FSD (`OR_4B_BLUFF`, etc.) is reserved for those.
- Nut advantage or Flopzilla-style hit-category breakdowns (owner liked both: v1.1 candidates).
- Biased/curated flop selection; flops are uniform in the MVP.
- Position selection in the postflop UI (Hero fixed to BTN).
- Persistence of any kind (accounts, localStorage stats, sync between devices).
- Spaced repetition/recall of missed Spots, settings screens, i18n beyond Spanish, native mobile apps, solver integration.

## Further Notes

- The owner provides the real Charts at development time ("te los paso en el momento del desarrollo"); everything else can be built and tested against fixture Charts in the meantime.
- The equity data contract is deliberately simple (per-flop pair of percentages) so a future flip from precalculated JSON to runtime Monte Carlo touches one file — but ADR-0001 currently rules that out for the perf/determinism reasons recorded there.
- FSD source document: owner's functional requirements doc (`Documento de Requisitos Funcionales — MVP`), the root document this spec was drilled from.
