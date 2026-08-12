# Battle System v0.1

Status: **SPECIFIED — BB-BATTLE-GATE-001 CLOSED**

## Objective
Define the deterministic lifecycle of a complete battle: setup, orientation, turns, action legality, transactions, stable-state victory evaluation, GameState validity, disconnected topology, and anti-stall termination.

## BB-BTL-001 — Battle state
A battle conceptually tracks battle identity, seeded RNG context, lifecycle phase, board, sides, pieces, active side, turn/round counters, active mutations, scheduled effects, resolution state, Collapse state, and result.

Lifecycle states: `SETUP`, `STARTING`, `ACTIVE`, `RESOLVING`, `ENDED`, `FAULTED`. Player input is accepted only at a stable input checkpoint in ACTIVE state. ENDED battles reject actions. FAULTED denotes deterministic resolution/system failure, not player defeat.

## BB-BTL-002 — Orientation
Orientation is domain-relative, never screen-relative. For the standard coordinate system, Player forward increases row number and Enemy forward decreases row number.

## BB-BTL-003 — Standard formation
The standard 6×6 hypothesis uses:

```text
6 | T L S C L T |
5 | P P P P P P |
4 | . . . . . . |
3 | . . . . . . |
2 | P P P P P P |
1 | T L S C L T |
    A B C D E F
```

Player: A1/F1 Towers, B1/E1 Leapers, C1 Seer, D1 Core, A2–F2 Pawns.
Enemy: A6/F6 Towers, B6/E6 Leapers, C6 Seer, D6 Core, A5–F5 Pawns.

StandardFormation is a baseline BattleConfiguration, not a Kernel Rule. Encounters/mutations may explicitly modify setup.

## BB-BTL-004 — First active side
Standard battles begin with Player as first active side. This consumes no RNG. An encounter/configuration may explicitly override first side. No v0.1 Mutation Rule Query for first side is exposed.

## BB-BTL-005 — Setup pipeline
1. initialize battle/seed context;
2. create board;
3. create sides and assign orientations;
4. place StandardFormation;
5. apply encounter configuration;
6. apply persistent Piece/Rule Mutations;
7. apply battle-scoped Board Mutations;
8. resolve setup modifiers;
9. validate initial GameState;
10. determine first active side;
11. emit BattleStarted;
12. resolve to stable state;
13. victory checkpoint;
14. begin first turn if non-terminal.

First-side authority exists before BattleStarted; no player input occurs until BattleStarted reactions stabilize.

## BB-BTL-006 — Turn lifecycle
Canonical order:
1. TURN_BEGIN;
2. update turn/round counters;
3. activate due ScheduledEffects;
4. emit TurnStarted;
5. resolve reactions/operations to stable state;
6. victory checkpoint;
7. generate legal Primary Actions;
8. if zero legal non-Pass actions, Pass becomes the sole Primary Action;
9. accept player input;
10. resolve selected Action transaction and all reactions to stable state;
11. victory checkpoint;
12. emit TurnEnded;
13. resolve to stable state;
14. victory checkpoint;
15. update completed-round / Collapse lifecycle;
16. if Collapse step is due, resolve it atomically to stable state and run victory checkpoint;
17. advance active side if battle remains non-terminal.

ScheduledEffects due at turn start activate before TurnStarted is emitted.

## BB-BTL-007 — Action legality
A candidate action is evaluated through base rules plus whitelisted Rule Query modifiers, ownership/source validation, target/destination validation, and topology/occupancy constraints.

Illegal action semantics: no GameState mutation, no RNG consumption, no turn consumption, and deterministic rejection reason.

## BB-BTL-008 — Pass
Pass is illegal while at least one legal non-Pass Primary Action exists. When the active side has zero legal non-Pass Primary Actions, Pass is the sole legal Primary Action and consumes the turn normally.

## BB-BTL-009 — Move transaction
Move requires an existing active-side piece, valid existing destination, resolved movement pattern/range, valid blocking/path semantics, DestinationValidity, and resolved occupancy validity. Successful relocation is atomic.

Canonical post-commit event sequence: `PieceMoved`, `TileLeft`, `TileEntered`.

## BB-BTL-010 — Capture transaction
Capture validates attacker/target ownership and resolved CapturePattern/CaptureValidity. It proposes target removal and resolves interception before relocation. If required target removal cannot commit, capture fails unless an explicit replacement operation provides another valid atomic result.

Successful base capture atomically removes target and relocates attacker. Canonical post-commit event sequence: `PieceCaptured`, `PieceDied(target)`, `PieceMoved(attacker)`, `TileLeft(origin)`, `TileEntered(destination)`.

`PieceCaptured` denotes committed capture, never attempted capture.

## BB-BTL-011 — Stable state
Victory/input checkpoints require: no active OperationGroup, resolution queue empty, and no ScheduledEffect due in the current lifecycle point. Mid-operation or mid-reaction state is never victory-evaluated and never player-observable as an input state.

## BB-BTL-012 — Victory
At each stable victory checkpoint:
- Player active Cores = 0, Enemy > 0 → Player defeat.
- Enemy active Cores = 0, Player > 0 → Player victory.
- Both = 0 → Double Break; in v0.1 single-player this is Player defeat.

A future ScheduledEffect promising Core resurrection does not count as an active Core. If the last Core dies and resolution stabilizes with zero active Cores, the battle terminates before that future resurrection.

## BB-BTL-013 — GameState invariants
At every stable committed state:
- entity identifiers are unique;
- each active piece has exactly one valid owner;
- each active piece occupies exactly one existing tile;
- occupancy satisfies the resolved TileOccupancy rule (base maximum: one active piece per tile);
- coordinates are unique within a board;
- side/ownership references are valid;
- active side references an existing side while battle is active;
- mutation scope/owner references are valid and compatibility constraints hold;
- ScheduledEffects contain deterministic execution metadata and valid snapshot/source semantics;
- no partial OperationGroup is externally visible;
- ENDED battles reject actions;
- input occurs only at stable ACTIVE input checkpoint.

A side having zero active Cores is a valid but terminal-candidate GameState. Valid state is not synonymous with non-terminal state.

## BB-BTL-014 — Disconnected topology
Disconnected board regions are valid. The engine does not implicitly reconnect them or move isolated pieces. If an entire active side has zero legal non-Pass Primary Actions, Pass applies. Anti-stall lifecycle guarantees eventual termination.

## BB-BTL-015 — Collapse
Collapse is Battle Lifecycle anti-stall policy, not a normal Mutation. Its activation threshold is a balance parameter. Once active, a Collapse step occurs at a defined completed-round cadence (baseline hypothesis: each completed round) and contracts the board from the outermost remaining ring inward.

A Collapse step is one atomic OperationGroup across the complete selected ring. Occupants of collapsing tiles are removed by kernel/lifecycle-authorized `forced` removal, which is non-interceptable by ordinary mutation interception, then the ring tiles are destroyed. Mutations cannot emit forced removal.

All removals/tile destruction in a Collapse step commit atomically, emit deterministic events, resolve reactions to stable state, then run a victory checkpoint. Thus simultaneous final-Core removal produces Double Break rather than order-dependent victory.

Collapse parameters may later expose explicitly safe modifiers, but no mutation may remove the BB-KRN-007 termination guarantee.

## BB-BTL-016 — Battle termination guarantee
Pass cannot create an infinite battle because Collapse eventually activates and monotonically removes finite board topology. Repeated Collapse steps eventually eliminate all remaining playable tiles/pieces if combat has not already produced a result. Each step is followed by stable-state victory evaluation.

## Structural validation matrix
The design passes the following scenarios without bespoke battle exceptions: deterministic setup; orientation; standard formation; Player-first default; BattleStarted mutation; turn lifecycle; illegal-action no-op; Pass; Move; Capture; Armored interception; event ordering; Portal+Fragile; stable victory; final-Core explosion; Double Break; Afterlife final Core; GameState invariants; disconnected topology; repeated Pass; Collapse; Collapse+Armored; Collapse+Explosive; guaranteed termination.

## Deferred details
The following do not reopen this gate: final board-size balance; final army-size balance; numeric Collapse threshold/cadence; concrete schemas/types; serialization; RNG algorithm; production FAULTED UX; encounter-specific formations; multiplayer victory/initiative semantics; exhaustive content definitions.
