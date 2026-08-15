# Formal Domain Model v0.1

Status: **DRAFT — BB-CONTRACT-GATE-001 IN PROGRESS**

## Objective
Define technology-neutral domain identities, value objects, aggregates, ownership boundaries, and invariants before implementation. These contracts refine the canonical design; they do not select a programming language, engine, framework, database, or serialization library.

## BB-CTR-001 — Canonical schema notation
Machine-readable data-shape contracts use **JSON Schema 2020-12** as the canonical schema notation. Canonical content/state documents may be authored in JSON or YAML if they decode to the same JSON data model. Semantics that cannot be expressed safely in JSON Schema remain normative prose invariants plus executable verification cases.

Generated language types may derive from schemas later; generated types are not the source of truth.

## BB-CTR-002 — Typed identities
Domain identifiers are semantic types, not interchangeable strings. Baseline identities include:
- `ProfileId`
- `RunId`
- `BattleId`
- `SideId`
- `PieceId`
- `PieceSlotId` (run-persistent army identity)
- `TileId` / `Coordinate`
- `ContentId`
- `MutationInstanceId`
- `OperationId`
- `OperationGroupId`
- `EventId`
- `ScheduledEffectId`
- `RewardOfferId`
- `RegionId`
- `EncounterNodeId`

Concrete textual/binary ID encoding is deferred. Equality is exact semantic identity; IDs are never reused for different live entities in the same scope.

## BB-CTR-003 — Stable ordering
Whenever a rule needs deterministic iteration and no stronger authored order exists, v0.1 canonical fallback ordering is:
1. explicit authored order;
2. lifecycle/activation sequence where specified;
3. board coordinates by row ascending then column ascending;
4. stable semantic identifier lexicographically.

A container's implementation iteration order is never gameplay authority.

## BB-CTR-004 — Coordinate
`Coordinate` is a board-local value object with integer `column_index` and `row_index`. Display labels such as `A1` are presentation-derived. A Coordinate may exist mathematically while its Tile is absent from topology.

## BB-CTR-005 — BoardState
A BoardState contains:
- board identity/configuration reference;
- finite coordinate bounds/configuration;
- existing TileState records keyed by Coordinate;
- explicit link records such as Portal `link_id` relationships.

Absence of a TileState at a coordinate means the tile does not exist in topology. Existing-but-empty and missing are distinct states.

## BB-CTR-006 — TileState
A TileState contains identity/coordinate plus declarative feature instances/tags and feature-local state. Occupancy is derived from active PieceState positions and must satisfy resolved `TileOccupancy`; it is not an independently mutable duplicate source of truth.

## BB-CTR-007 — SideState
A SideState contains `side_id`, semantic role, orientation (`ROW_INCREASING` or `ROW_DECREASING`), and side-scoped battle state. Player/Enemy presentation is not part of the domain contract.

## BB-CTR-008 — PieceState
A PieceState contains:
- `piece_id`;
- optional originating `piece_slot_id` for run-owned pieces;
- owner `side_id`;
- base archetype/content reference;
- lifecycle state `ACTIVE` or `REMOVED`;
- Coordinate iff ACTIVE;
- active Piece Mutation instance references;
- battle-temporary declarative state required by specified mechanics.

ACTIVE pieces have exactly one existing tile. REMOVED pieces have no board Coordinate. Core status is resolved by `CoreClassification`; it is not inferred solely from archetype.

## BB-CTR-009 — MutationInstance
A MutationInstance references one immutable content definition plus runtime scope metadata: instance ID, owner/target reference, lifetime, activation sequence, stacking state, consumable/charge state when declared, and source provenance. Mechanics remain in the content definition; runtime instances contain only state needed by those mechanics.

## BB-CTR-010 — Run army identity
`PieceSlotId` is the persistent identity to which run-persistent Piece Mutations attach. A Battle creates PieceState entities from the run army configuration. Battle death removes the Battle PieceState but does not delete its PieceSlot from RunState.

## BB-CTR-011 — Aggregate boundaries
Canonical aggregate ownership:
- `ProfileState`: cross-run progression only.
- `RunState`: one run's generation identity, route progress, persistent army/build, offers/history.
- `BattleState`: one battle's lifecycle and tactical state.
- `GameState`: deterministic tactical snapshot nested inside BattleState.

RunState must not persist battle-local positions, Collapse state, temporary battle effects, or battle ScheduledEffects after the encounter ends.

## BB-CTR-012 — No presentation state in domain authority
Selection, camera, hover, animation progress, audio playback, visual particles, panel state, and haptic state are not authoritative GameState/RunState fields. Presentation may derive from domain events/state but cannot mutate them directly.

## BB-CTR-013 — Validity vs terminality
Schema-valid state and gameplay-nonterminal state are separate predicates. A GameState with zero active Cores may be structurally valid while requiring terminal victory evaluation.

## BB-CTR-014 — Content references
Runtime state references content by stable `ContentId` plus the enclosing `content_version`/ruleset identity. Runtime saves never embed executable content behavior.

## Deferred within this gate
Concrete ID encoding, numeric capacities/balance values, exact Rule Query composition algebra, exact resolution budget/cycle signature, exact RNG algorithm, and mid-battle interruption recovery remain unresolved until their designated contract micro-pass.
