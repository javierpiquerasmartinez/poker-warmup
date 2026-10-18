---
status: accepted
---

# Precalculated flop equities from Chart-derived Ranges, via offline Monte Carlo script

The postflop drill validates answers using Flopzilla-style range-vs-range equity on each flop. We precalculate that equity for all 19,600 flops at dev time (Node script, Monte Carlo over random runouts with combo-weighted sampling from each Chart-derived Range) and ship the result as a static JSON asset — instead of running Monte Carlo in the browser per question. Then BTN ≥ 52% classes as Hero advantage, ≤ 48% as Villain, else Neutral (thresholds are consts in code).

The alternatives were runtime Monte Carlo in the SPA (rejected: violates the "instant" NFR, adds CPU spikes, non-deterministic feedback) and a real solver (rejected: out of MVP scope; all-in equity on the flop is what defines range advantage here). Because Ranges are derived from the Charts (see GLOSSARY), changing charts means re-running one command; per-combo detail (hit categories %) can be added later as more precalculated columns without changing the app model.
