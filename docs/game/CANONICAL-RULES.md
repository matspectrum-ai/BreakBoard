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
Moves one tile forward into valid unoccupied tile; captures one diagonal forward. No double move, en passant, or automatic promotion. Player forward is +row and Enemy forward is -row in the standard orientation.
### BB-PCS-006
Current **balance hypothesis**: 1 Core, 2 Towers, 2 Leapers, 1 Seer, 6 Pawns.

## Turn and actions
Canonical battle lifecycle, action legality, Move/Capture transactions, event ordering, Pass, stable-state checkpoints, GameState invariants, disconnected topology, and Collapse semantics are defined by `BATTLE-SYSTEM.md`.

### BB-ACT-001
Base action vocabulary: Move, Capture, Ability.
### BB-ACT-002
State-changing actions resolve through proposed atomic operations/OperationGroups. Capture is transactional: target removal must resolve before attacker relocation commits.

## Victory
### BB-VIC-001
A side becomes a loss candidate when `active_cores == 0` at a canonical stable checkpoint.
### BB-VIC-002
Single-player simultaneous final-Core loss in one stabilized resolution is Double Break and Player defeat.
### BB-VIC-003
A side with no legal non-Pass Primary Action may Pass; Pass is otherwise illegal.
### BB-VIC-004
Collapse is Battle Lifecycle anti-stall policy. Its exact numeric activation parameters remain balance values; the termination guarantee is canonical.

## Mutations
### BB-MUT-001
Every mutation is Piece, Board, or Rule.
### BB-MUT-002
Compatibility vocabulary: `requires`, `excludes`, `overrides`; contradictions are never arbitrarily resolved and Kernel Rules cannot be overridden.
### BB-MUT-003
Piece and Rule Mutations persist for run by default (Rule may be explicitly temporary). Board Mutations are battle-scoped by default.
### BB-MUT-004
Capacity exists. Current **balance hypotheses**: up to 3 Piece Mutations per piece and 5 active Rule Mutations. Board capacity is not persistent player inventory in Run v0.1.
### BB-MUT-005
Seeded generator presents persisted candidate offers and the player explicitly chooses. Current **balance hypothesis**: 3 choose 1.
### BB-MUT-006
Mutation behavior is declarative through Reactions and/or Modifiers. Reactions respond to immutable post-commit events; Modifiers contribute only to whitelisted Rule Queries.
### BB-MUT-007
Operations may be intercepted before commit; committed Events are immutable facts. Compound changes commit atomically or not at all.
### BB-MUT-008
Delayed mutation behavior is explicit serializable ScheduledEffect state; historical restoration uses explicit snapshot profiles.

## RNG
### BB-RNG-001
Every run has a user-visible seed plus deterministic generation context. Independent derived namespaces isolate route, encounter, battle, reward, event, and boss generation.
### BB-RNG-002
Movement/capture/effect resolution is deterministic; randomness generates options/content, not hidden arbitrary resolution. Random selectors use explicit seeded RNG state.
### BB-RNG-003
Full procedural reproducibility identity includes run seed, content version, and eligible-content/unlock snapshot.

## Run and progression
### BB-RUN-001
A run is a deterministic branching sequence of regions/encounters ending in COMPLETED, FAILED, ABANDONED, or FAULTED outcome.
### BB-RUN-002
Standard run baseline uses three regions with layered DAG route graphs. Exact layer/node counts are balance parameters.
### BB-RUN-003
Canonical encounter categories are Battle, Elite, Event, and Boss. Elite is optional and never the sole route in the standard graph.
### BB-RUN-004
Normal Battle rewards build Piece Mutations; Elite rewards enhanced Piece Mutation opportunities; non-final Region Bosses reward Rule Mutation offers; Final Boss victory completes the run.
### BB-RUN-005
Board Mutations are battle/encounter configuration in v0.1, not persistent player inventory rewards.
### BB-RUN-006
Reward offers are generated once and persist; reload/UI reopening cannot reroll them. Full capacity requires explicit replacement and never silent eviction.
### BB-RUN-007
No free arbitrary mutation removal exists during standard v0.1 runs; removal is explicit through replacement, Event, or effect.
### BB-RUN-008
Army pieces do not have base inter-battle permadeath; standard army state is restored between battles while run-persistent Piece/Rule Mutations remain.
### BB-RUN-009
Run generation uses template constraints, complexity budgets, route-diversity/anti-repetition constraints, and bounded deterministic generation attempts.

## Meta progression
### BB-META-001
Persistent progression is horizontal: unlock possibilities rather than permanent raw combat advantages.
### BB-META-002
Active runs snapshot eligible content at creation; later profile unlocks do not mutate the active run generation pool.
### BB-META-003
Base progression integrity forbids paid permanent power, paid combat advantage, paid in-run rerolls, paid mutation slots, and paid seed manipulation.
