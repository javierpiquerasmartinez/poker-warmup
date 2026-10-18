# Poker Warmup

A mental gym for pre-session poker warmup drills: postflop board-reading and preflop chart review for 6-max, driven entirely by local data.

## Language

**Spot**:
A concrete practice scenario: a pot situation plus the two players' positions (e.g., single-raised pot, BTN announces vs BB answers). One Spot fixes the ranges involved and the set of legal Subactions.
_Avoid_: scenario, situation, matchup

**SRP (Single-Raised Pot)**:
A pot where exactly one pre-flop raise has been made and it was called — no 3-bet yet.
_Avoid_: limp pot, 2-bet pot

**Chart**:
A 13×13 grid of Hand Classes for one Spot side, where each cell carries the canonical Subaction for that Hand Class. The 169 cells resolve offline into per-Combo entries at load time.
_Avoid_: table, matrix, rango

**Subaction**:
The exact action a Hand Class belongs to within a Chart (e.g., `FOLD`, `OR_CALL_3B`, `OR_4B_VALUE`). Each Combo has exactly one canonical Subaction — charts are unmixed in the MVP.
_Avoid_: action label, decisión

**Range**:
The set of Combos a player can hold in a Spot. Derived, never hand-maintained: a Range is exactly the non-`FOLD` entries of the relevant Chart (e.g., BB Flat Range = the `BB vs BTN` Chart lacking Subaction `FOLD`).
_Avoid_: rango, hand range

**Hero**:
The side of the Spot being drilled — the player whose answer we validate. Always the pre-flop aggressor in the MVP.
_Avoid_: jugador, usuario

**Villain**:
The rival side of the Hero within a Spot.
_Avoid_: oponente, enemigo

**Hand Class**:
One of the 169 grid entries — 13×13 — such as `TT`, `AKs`, `AKo`. A notation bucket, not a set of physical cards.
_Avoid_: hand, mano

**Combo**:
One specific pair of distinct physical cards (e.g., `A♦J♠`). A Hand Class maps to a fixed number of Combos: pocket pair 6, suited 4, offsuit 12 — 1,326 total in the deck.
_Avoid_: card combo, combinación

**RFI (Raise First In)**:
Opening the pot as the first raiser — no prior raise to face. In the MVP, the BTN RFI Spot has only two Subactions: `FOLD | RAISE`.
_Avoid_: open, apertura

## Preserved mistakes

_Wrong usage to watch for_: "range advantage" used loosely to mean anything favorable — the MVP defines one precise metric per Spot; do not extend it silently.
