# Mutation System v0.1

Status: **SPECIFIED — BB-MUT-GATE-001 CLOSED**

## Objective
Represent Piece, Board, and Rule Mutations with a compact deterministic composable vocabulary, avoiding bespoke engine branches per mutation.

## BB-MSYS-001 — Mutation structure
A mutation is declarative and may contain metadata, compatibility, lifetime, stacking, Reactions, and Modifiers.

Conceptual form for a Reaction:
`WHEN <trigger> IF <conditions> SELECT <targets> APPLY <effects>`.

Only Effects/committed operations mutate GameState. Triggers, Conditions, and Selectors are pure observations/queries.

## BB-MSYS-002 — Two behavior families
- **Reaction**: responds to an emitted event and proposes effects.
- **Modifier**: contributes to a whitelisted Rule Query before an operation is proposed.

A mutation may contain either or both.

## BB-MSYS-003 — Event taxonomy
Canonical v0.1 event families:
- BattleStarted, BattleEnded
- TurnStarted, TurnEnded
- ActionStarted, ActionResolved
- PieceMoved
- PieceCaptured
- PieceDied
- TileEntered, TileLeft, TileModified, TileDestroyed
- MutationAdded, MutationRemoved

Events are immutable facts after commit. `PieceCaptured` carries actor, target, origin, and destination context; attacker/victim semantics are expressed through conditions rather than separate capture event types.

## BB-MSYS-004 — Conditions
Conditions are pure deterministic predicates over stable state and event/query context. The baseline vocabulary includes identity/ownership, piece type/tag/mutation, tile existence/tag, Core count, turn number, adjacency/range, mutation/rule activity, plus logical AND/OR/NOT composition.

## BB-MSYS-005 — Selectors
Selectors deterministically resolve zero or more references. Baseline concepts include Self, EventActor, EventTarget, TileOf, PieceAt, AdjacentTiles, AdjacentPieces, AllAllies, AllEnemies, AllPieces, Row, Column, WithinRange, LinkedTile, and deterministic RandomFrom using the seeded battle/run RNG context.

A selector returning zero targets is valid. Effects define what to do with targets that become invalid before commit.

## BB-MSYS-006 — Effect vocabulary
Baseline effect intents include MovePiece, RemovePiece, SpawnPiece, TransformPiece, AddPieceMutation, RemovePieceMutation, ModifyTile, DestroyTile, RestoreTile, CreateLink, AddRuleMutation, RemoveRuleMutation, GrantAction, CancelAction, and ScheduleEffect.

Capture is an Action/transaction, not an Effect primitive.

## BB-MSYS-007 — Intercept operations; react to events
State-changing Effects produce proposed Operations or OperationGroups before commit. A controlled interception window may ALLOW, CANCEL, or REPLACE an interceptable proposed operation. After commit, emitted Events cannot be retroactively cancelled.

This is the canonical distinction: **intercept operations; react to events**.

## BB-MSYS-008 — Atomic OperationGroup
Compound state changes are represented as an atomic OperationGroup:
1. propose;
2. validate;
3. collect/resolve interceptors deterministically;
4. revalidate;
5. commit all, or commit none;
6. emit resulting immutable events.

No invalid intermediate GameState is externally observable.

Capture is transactional: target removal must be allowed before attacker relocation commits. If target removal is cancelled, the capture transaction fails unless an explicit replacement operation defines another valid result.

## BB-MSYS-009 — Occupied tile destruction
DestroyTile has no implicit occupant side effect. An occupied target requires an explicit occupant policy:
- `reject`;
- `remove`;
- `relocate` with an explicit deterministic selector.

If the required occupant operation cannot commit (for example removal is intercepted and leaves an occupant), tile destruction cannot commit. This preserves valid-state invariants.

## BB-MSYS-010 — Scheduled effects
Delayed behavior is represented as serializable declarative ScheduledEffect state, never callbacks/closures. It records execution phase/time offset, source, effect payload, required snapshot/reference data, and invalid-target policy.

Scheduled effects participate in the same deterministic resolution pipeline when due.

## BB-MSYS-011 — Snapshots
Historical resurrection/transform behavior uses explicit snapshots captured at a specified lifecycle point. Snapshot profiles explicitly state which properties are preserved or discarded (for example archetype, owner, mutations, temporary effects). No implicit resurrection-copy policy exists.

## BB-MSYS-012 — Invalid targets
Invalid-target behavior is explicit. Baseline policies are `skip` and `fail_resolution`. Fallback targeting must be expressed by deterministic selectors (for example FirstValid[OriginalTile, AdjacentEmptyTiles]); the engine never silently chooses a nearest target.

## BB-MSYS-013 — Lifetime
Baseline mutation/effect lifetimes are `instant`, `turn`, `battle`, and `run`. Meta-persistent/permanent behavior is outside normal v0.1 battle mutation semantics.

## BB-MSYS-014 — Stacking
Every stackable definition declares one policy: `prohibited`, `replace`, `refresh`, or `independent`. Duplicate behavior is never inferred from content name or category.

## BB-MSYS-015 — Compatibility
`requires` means eligibility depends on another definition/state. `excludes` prevents coexistence. `overrides` explicitly replaces a modifiable base/rule behavior. No mutation may override a Kernel Rule.

## BB-MSYS-016 — Modifier hooks / Rule Queries
Modifiers may affect only a finite whitelisted Rule Query vocabulary. Initial v0.1 hooks:
- MovementPattern
- MovementRange
- MovementBlocking
- DestinationValidity
- CapturePattern
- CaptureValidity
- AvailableActions
- PrimaryActionCount
- TileTraversability
- TileOccupancy
- CoreClassification

Each hook owns an explicit composition strategy. Generic arbitrary function/code modification is forbidden.

Examples: Ghost modifies MovementBlocking but does not automatically alter DestinationValidity. A lateral-Pawn rule contributes vectors to MovementPattern.

## BB-MSYS-017 — Modifier composition
Modifiers are deterministic. A hook must define its algebra/composition strategy (for example override/add/remove/constraint phases or `deny_wins` for a boolean constraint). Generic priority alone is not a substitute for hook-specific composition semantics.

Kernel concerns such as atomicity, deterministic ordering, resolution budget, stable-state requirement, seed discipline, and transaction integrity expose no mutation hook.

## BB-MSYS-018 — Reaction ordering and queue
After atomic commit:
1. emit resulting Events;
2. collect eligible Reactions;
3. order deterministically by system layer, declared priority, activation sequence, then stable mutation identifier;
4. enqueue;
5. resolve one queued Reaction into proposed operations;
6. commit atomically;
7. emit resulting Events and collect further Reactions;
8. continue until queue empty;
9. only then is state stable for the next canonical checkpoint/input.

System-layer precedence baseline is Kernel constraints, then Rule Mutation, Board Mutation, Piece Mutation. Exact numeric priority ranges are a future contract detail.

## BB-MSYS-019 — Bounded resolution
Reaction execution uses both a finite resolution budget and cycle detection. Overflow/cycle detection cannot accept a partially resolved action: the current atomic action resolution rolls back to its previous stable state and produces a deterministic resolution fault. Exact budget and cycle-signature algorithm are deferred to the Resolution Contract; production recovery UX is deferred.

## BB-MSYS-020 — Seeded randomness
Any selector/generator requiring randomness consumes explicit seeded RNG context. Unseeded process/global randomness is forbidden. Same state, seed state, and input must reproduce the same selection and resolution.

## BB-MSYS-021 — Mutation capacity/removal
Capacity is required by canonical rules, but numeric capacities remain balance parameters. Adding a mutation at capacity requires an explicit acquisition/replacement decision; the engine does not silently evict an existing mutation. Voluntary removal outside a defined replacement/acquisition flow remains a later product-design question.

## Validation matrix
| Representative mechanic | Representation | Result |
|---|---|---|
| Explosive | PieceDied Reaction → adjacent selector → RemovePiece | PASS |
| Armored | intercept proposed RemovePiece; consume armor | PASS |
| Ghost | MovementBlocking Modifier | PASS |
| Portal | TileEntered Reaction + LinkedTile + MovePiece/CreateLink | PASS |
| Fragile | TileLeft Reaction → DestroyTile | PASS |
| Capture destroys tile | PieceCaptured Reaction → DestroyTile with explicit occupant policy/atomic group | PASS |
| Afterlife | PieceDied Reaction → ScheduledEffect + explicit snapshot/target policy | PASS |
| Multiple Core | setup/rule effect + existing active-Core victory query | PASS |
| Pawn lateral movement | MovementPattern Modifier | PASS |

No representative mutation above requires a mutation-name conditional in the engine.

## Collapse reclassification
Collapse is not a normal Mutation. It is part of Battle Lifecycle / anti-stall policy because it enforces BB-KRN-007 battle termination. Future Rule Mutations may modify explicitly exposed Collapse parameters but cannot remove the termination guarantee.

## Deferred contract details
The following do not reopen this design gate: exact schemas/types; numeric priority ranges; exact resolution budget; exact cycle signature; full Rule Query composition algebra; concrete snapshot serialization; concrete seeded RNG algorithm; production resolution-fault UX; content-specific mutation definitions and balance values.
