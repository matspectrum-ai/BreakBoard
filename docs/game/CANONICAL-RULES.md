# Canonical Game Rules v0.1

Status: canonical baseline with marked hypotheses/balance parameters.

## Kernel
### BB-KRN-001 Determinism
Same stable state, action, and seeded randomness context produces the same result.
### BB-KRN-002 Atomic resolution
An action and all scheduled effects reach stable state before another player input.
### BB-KRN-003 Bounded effects
Effect chains cannot recurse forever. Mutation System v0.1 requires a finite resolution budget plus cycle detection; exact contract parameters remain deferred.
### BB-KRN-004 Victory checkpoints
Victory is evaluated only at defined stable checkpoints.
### BB-KRN-005 Valid state
No mutation/effect may commit invalid GameState.
### BB-KRN-006 Deterministic precedence
Interacting rules/effects require explicit deterministic ordering.
### BB-KRN-007 Termination
Every battle has a termination mechanism. Collapse is Battle Lifecycle/anti-stall policy, not a normal Mutation.

## Board
### BB-BRD-001
Current **hypothesis**: default 6×6; dimensions are not a kernel constant.
### BB-BRD-002
A destroyed tile is absent from topology and differs from an empty tile.
### BB-BRD-003
For 6×6, coordinates are columns A–F and rows 1–6.

## Pieces
Canonical base archetypes: Core, Tower, Leaper, Seer, Pawn.
### BB-PCS-001 Core
Moves one valid tile in any direction and may capture.
### BB-PCS-002 Tower
Moves orthogonally through contiguous valid unoccupied tiles until blocked; capture ends on target.
### BB-PCS-003 Seer
Moves diagonally through contiguous valid unoccupied tiles until blocked; capture ends on target.
### BB-PCS-004 Leaper
Displacements (+/-2,+/-1) and (+/-1,+/-2); may cross intervening occupancy/missing locations, but destination must be valid.
### BB-PCS-005 Pawn
Moves one tile forward into valid unoccupied tile; captures one diagonal forward. No double move, en passant, or automatic promotion. Player-relative forward orientation remains open.
### BB-PCS-006
Current **balance hypothesis**: 1 Core, 2 Towers, 2 Leapers, 1 Seer, 6 Pawns.

## Turn pipeline
TURN_START → START_EFFECTS → PLAYER_ACTION → ACTION_RESOLUTION → TRIGGER_RESOLUTION → VICTORY_CHECK → END_EFFECTS → FINAL_VICTORY_CHECK → TURN_END.
Mutation System v0.1 defines event/reaction resolution to stable state. Battle System v0.1 must formalize the complete lifecycle/checkpoint semantics.

## Actions
### BB-ACT-001
Base vocabulary: Move, Capture, Ability.
### BB-ACT-002
State-changing actions resolve through proposed atomic operations/OperationGroups. Capture is transactional: target removal must resolve before attacker relocation commits.

## Victory
### BB-VIC-001
A side becomes a loss candidate when `active_cores == 0` at a canonical checkpoint.
### BB-VIC-002
Single-player simultaneous final-Core loss in one stabilized resolution is **Double Break** and player battle failure. Multiplayer deferred.
### BB-VIC-003
No legal action permits Pass rather than automatic loss.
### BB-VIC-004
Current anti-stall hypothesis: Collapse Phase after a threshold progressively forces resolution. Exact Battle Lifecycle algorithm is open.

## Mutations
### BB-MUT-001
Every mutation is Piece, Board, or Rule.
### BB-MUT-002
Compatibility vocabulary: `requires`, `excludes`, `overrides`; contradictions are never arbitrarily resolved and Kernel Rules cannot be overridden.
### BB-MUT-003
Piece and Rule Mutations persist for run by default (Rule may be explicitly temporary). Board Mutations are battle-scoped by default.
### BB-MUT-004
Capacity exists. Current **balance hypotheses**: up to 3 Piece Mutations per piece and 5 active Rule Mutations. Board capacity unresolved.
### BB-MUT-005
Seeded generator presents candidates after qualifying encounters; player chooses. Current **balance hypothesis**: 3 choose 1.
### BB-MUT-006
Mutation behavior is declarative through Reactions and/or Modifiers. Reactions respond to immutable post-commit events; Modifiers contribute only to whitelisted Rule Queries.
### BB-MUT-007
Operations may be intercepted before commit; committed Events are immutable facts. Compound changes commit atomically or not at all.
### BB-MUT-008
Delayed mutation behavior is represented as explicit serializable ScheduledEffect state; historical restoration uses explicit snapshot profiles.

## RNG
### BB-RNG-001
Every run has a seed controlling procedural generation such as encounters, offers, enemy variants, boards, and events.
### BB-RNG-002
Movement/capture/effect resolution is deterministic; randomness generates options/content, not hidden arbitrary resolution. Random selectors use explicit seeded RNG state.

## Meta
### BB-META-001
Persistent progression is horizontal: unlock possibilities rather than permanent raw combat advantages.

## Run
A run has multiple regions with battles/events/elites/bosses. Previously discussed 12–15 encounters and 30–60 minutes are unvalidated hypotheses.
