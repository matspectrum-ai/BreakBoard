# Initial Rule Mutation & Board Feature Catalog v0.1

## Rule Mutations
All are run-persistent, unique within one Run, and same-rule stacking is prohibited.

| ID | Name | Initial unlock | Complexity | Mechanic |
|---|---|---:|---:|---|
| BB-RM-001 | Revolution | yes | 1 | Player Pawns gain one-tile lateral non-capture movement. |
| BB-RM-002 | Momentum | yes | 2 | First Player Capture each turn grants the capturing Piece one additional Primary Action. |
| BB-RM-003 | Scorched Origins | yes | 2 | After each Player Capture, destroy the attacker's vacated origin tile if still valid. |
| BB-RM-004 | Twin Crown | no | 2 | Player Seer has CoreClassification=true. |
| BB-RM-005 | Open Lines | yes | 2 | Player Towers/Seers ignore allied Pieces as movement blockers; destination occupancy unchanged. |
| BB-RM-006 | Afterlife Pact | no | 3 | First Player non-Core death each Battle schedules resurrection at next owner TurnStart using explicit snapshot/original-tile/skip policy. |
| BB-RM-007 | Blood Price | no | 4 | Player may Capture allied non-Core Pieces when normal pattern/validity otherwise passes; normal capture/death reactions apply. |
| BB-RM-008 | Shattered Law | no | 3 | Every committed Capture globally adds Fragile to the destination tile after capture commit if tile exists. |

### Rule compatibility
Initial Rule Mutations have no pairwise `excludes`. Composition is intentional: Momentum+Chain may grant multiple bounded additional actions; Scorched Origins+Shattered Law alters both origin and destination; Twin Crown composes with Piece-level Crowned; Afterlife Pact may coexist with Piece Afterlife and duplicate later resurrection attempts resolve by explicit target validity.

## Board Features

| ID | Name | Earliest region | Complexity | Semantics |
|---|---|---:|---:|---|
| BB-BF-001 | Fragile | 1 | 1 | On TileLeft, destroy this tile after the departure operation has committed. |
| BB-BF-002 | Wall | 1 | 1 | Existing non-occupiable, non-traversable, path-blocking tile; excludes every other functional Board Feature on same tile. |
| BB-BF-003 | Portal | 1 | 2 | Linked pair; voluntary TileEntered transfers occupant to linked valid endpoint with `cause=portal_transfer`; no recursive portal transfer within that transfer; incomplete link inert. |
| BB-BF-004 | Conveyor | 2 | 2 | At occupant owner's TurnStarted, propose one external MovePiece in configured direction; invalid destination → skip; Anchor may intercept. |
| BB-BF-005 | Void | 2 | 2 | At occupant owner's TurnEnded while still occupied, propose ordinary RemovePiece; ordinary interception such as Armored applies. |
| BB-BF-006 | Sanctuary | 2 | 2 | Occupant cannot be an ordinary Capture target; environmental/forced removal unaffected. |
| BB-BF-007 | Brittle | 2 | 2 | On committed Capture on this tile, add Fragile; does not immediately destroy tile. |
| BB-BF-008 | Beacon | 2 | 3 | At occupant owner's TurnStarted, grant occupant one additional Primary Action once for that turn. |

### Feature composition
Except Wall exclusions, initial Board Features may coexist. Existing Mutation/Battle ordering defines results. Examples: Portal+Fragile transfers the entering Piece then Portal TileLeft may destroy the endpoint; remaining endpoint becomes inert. Sanctuary+Void protects from Capture but not TurnEnd removal. Beacon+Void may grant an action before later TurnEnd threat.
