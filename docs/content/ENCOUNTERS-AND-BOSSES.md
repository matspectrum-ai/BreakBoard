# Encounter Templates, Elites & Bosses v0.1

## Normal Encounter Templates

| ID | Name | Regions | Cost | Configuration |
|---|---|---|---:|---|
| BB-ENC-001 | Clean Board | 1,2 | 0 | Standard board/army; no authored mutations/features. |
| BB-ENC-002 | Fragile Center | 1,2,3 | 2 | Central valid tiles receive Fragile via deterministic symmetric pattern. |
| BB-ENC-003 | Broken Corners | 1,2,3 | 2 | Deterministic symmetric subset of outer corner-area tiles starts destroyed; initial formation tiles remain valid. |
| BB-ENC-004 | Portal Cross | 1,2,3 | 3 | One or two symmetric Portal pairs placed only on initially empty valid tiles. |
| BB-ENC-005 | Iron Line | 1,2,3 | 3 | Enemy Core and Towers receive Armored. |
| BB-ENC-006 | Powder Line | 1,2,3 | 4 | Enemy Pawns receive Explosive. |
| BB-ENC-007 | Ghost Corridor | 2,3 | 4 | Wall lane pattern plus Enemy Tower/Seer Ghost; generated setup validates non-blocked initial state. |
| BB-ENC-008 | Conveyor Grid | 2,3 | 4 | 2–4 deterministic Conveyors on initially empty valid tiles. |
| BB-ENC-009 | Shattered Battlefield | 2,3 | 5 | Combination of Fragile, Brittle, and bounded missing topology; initial GameState and reachability validation required. |
| BB-ENC-010 | Rift Legion | 3 | 6 | Enemy Towers/Seer receive Riftwalker; board contains bounded gaps and Portal structure. |

No identical template may appear consecutively in one Run. Templates are filtered by region and encounter complexity budget before weighting.

## Elite Presets

| ID | Name | Regions | Configuration |
|---|---|---|---|
| BB-ELT-001 | Iron Phalanx | 1,2,3 | Enemy Core/Towers Armored; authored symmetric Wall pattern on C3/D4 where valid. |
| BB-ELT-002 | Powder Keg | 1,2,3 | Enemy Pawns Explosive; center C3,D3,C4,D4 receive Fragile where valid. |
| BB-ELT-003 | Phase Choir | 2,3 | Enemy Towers/Seer Ghost; Portal pairs B3↔E4 and E3↔B4 where valid. |
| BB-ELT-004 | Chain Hunt | 2,3 | Enemy Leapers Chain; Beacon tiles C3 and D4 where valid. |
| BB-ELT-005 | Twin Crown | 2,3 | Enemy Seer CoreClassification=true; original Enemy Core Armored. |
| BB-ELT-006 | Rift Legion | 3 | Enemy Towers/Seer Riftwalker; bounded destroyed lanes, one Portal pair, one authored Void tile. |

Elite remains optional in the route graph and rewards enhanced Piece Mutation offers.

## Bosses

### BB-BOSS-001 — The Architect (Region I)
Identity: battlefield architecture changes each completed round.

Configuration:
- Enemy Core has Armored.
- A pre-authored deterministic sequence of mirrored tile pairs is stored in encounter config.
- At each completed round, restore the previous architecture pair to normal tile state when valid, then attempt to convert the next pair to Wall.
- Conversion uses explicit `relocate` occupant policy with deterministic adjacent-empty selector; if relocation has no valid result, that individual conversion is skipped rather than committing invalid state.
- All changes use ordinary Board/Operation/ScheduledEffect primitives; no Boss-specific engine branch.

Complexity: 5.
Reward: Region I Boss Rule Mutation offer.

### BB-BOSS-002 — The Chain Sovereign (Region II)
Configuration:
- Boss Rule: first Enemy committed Capture each turn grants the capturing Piece one additional Primary Action.
- Enemy Leapers have Chain.
- Beacon tiles C3 and D4 where valid.
- All action grants remain bounded by per-turn trigger conditions and Mutation resolution budget.

Complexity: 7.
Reward: Region II Boss Rule Mutation offer.

### BB-BOSS-003 — The Void (Final Boss)
Configuration:
- Collapse lifecycle override baseline: activate after completed round 4; cadence every completed round. This is authored Boss balance data and may be tuned without changing the Battle termination contract.
- Portal pair C3↔D4 where valid.
- Enemy Towers receive Riftwalker.
- Enemy Seer receives Ghost.
- Standard Collapse uses Battle lifecycle forced-removal semantics; no Mutation can suppress the termination guarantee.

Complexity: 9.
Reward: none; victory completes the Run.

Boss IDs are fixed to the standard three-region baseline and cannot duplicate within a standard Run.
