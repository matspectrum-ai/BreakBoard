# Resolution Contract v0.1

Status: **SPECIFIED — BB-CONTRACT-GATE-001 CLOSED**

## BB-RSL-001 — Action intent
Canonical primary Action kinds are `MOVE`, `CAPTURE`, `ABILITY`, and `PASS`.

An ActionIntent contains stable intent identity, kind, acting Side, optional actor Piece, source Coordinate where applicable, target/destination references, optional ability ContentId, and explicit user-selected parameters required by that action.

ActionIntent is a request, not a state mutation.

## BB-RSL-002 — Pure legality evaluation
`EvaluateAction(GameState, ActionIntent)` is observational. It returns deterministic legality plus a stable rejection reason code and may return direct-consequence preview data. Evaluation must not mutate state, consume gameplay RNG, advance turn counters, emit committed Events, or consume mutation charges.

## BB-RSL-003 — Operation
Only canonical Operations may mutate GameState. Baseline operation kinds mirror the Mutation System: `MovePiece`, `RemovePiece`, `SpawnPiece`, `TransformPiece`, mutation add/remove, Rule add/remove, `ModifyTile`, `DestroyTile`, `RestoreTile`, `CreateLink`, `GrantAction`, `CancelAction`, and `ScheduleEffect`.

Each Operation carries stable identity, source/cause provenance, typed payload, and interception policy.

## BB-RSL-004 — Removal authority
`RemovePiece` declares `mode: ORDINARY|FORCED`.
- ORDINARY may be intercepted by permitted mutation mechanics.
- FORCED is reserved for explicit Kernel/Battle Lifecycle authority such as Collapse and cannot be emitted by ordinary content definitions.

Schema/content validation rejects content that attempts to create FORCED removal authority.

## BB-RSL-005 — OperationGroup
An OperationGroup is the atomic transaction boundary. It contains ordered Operations, cause/source metadata, and rollback boundary identity.

Lifecycle:
1. propose;
2. structural/semantic validate;
3. collect eligible interceptors;
4. resolve interceptors deterministically;
5. revalidate replacement/final operations;
6. commit all or none;
7. emit canonical immutable Events in defined order.

No partially committed OperationGroup may be externally visible.

## BB-RSL-006 — Interception result
For an interceptable proposed Operation, an interceptor may return exactly one semantic result:
- `ALLOW`;
- `CANCEL`;
- `REPLACE` with a complete replacement operation/group that must revalidate.

Interceptors do not mutate committed state directly and cannot retroactively modify emitted Events.

## BB-RSL-007 — Capture transaction
Base Capture resolves as one atomic transaction in which target removal is validated/intercepted before attacker relocation can commit. If required target removal cannot commit, base Capture commits nothing unless an explicit REPLACE result defines another complete valid transaction.

Successful base Capture emits the canonical ordered facts:
1. `PieceCaptured`;
2. `PieceDied(target)`;
3. `PieceMoved(attacker)`;
4. `TileLeft(origin)`;
5. `TileEntered(destination)`.

## BB-RSL-008 — Event envelope
A committed Event is immutable and contains:
- `event_id`;
- monotonic `event_sequence` within Battle;
- event kind;
- source/cause provenance;
- committed OperationGroup ID;
- typed payload;
- lifecycle point/turn context needed by Conditions.

Event IDs/sequence are presentation-independent.

## BB-RSL-009 — Reaction contract
A Reaction definition contains trigger kind, pure Conditions, deterministic Selector, Effects, system layer, declared priority, and source mutation definition. A queued Reaction instance additionally records source MutationInstance, triggering Event, activation sequence, and queue identity.

Reactions cannot execute arbitrary code supplied by content.

## BB-RSL-010 — Reaction queue ordering
Eligible Reactions sort by:
1. system layer: Kernel constraints > Rule > Board > Piece;
2. declared priority within the layer;
3. MutationInstance activation sequence;
4. stable mutation ContentId;
5. stable instance ID only as final tie-break.

Reaction `priority` is a signed integer in `[-1000, 1000]`, default `0`. Content/schema validation rejects values outside this range. Equal semantic input must produce equal total ordering.

## BB-RSL-011 — Modifier contract
A Modifier contributes only to one whitelisted Rule Query hook. It declares hook, contribution phase/type, value payload, source, and deterministic ordering metadata. It cannot mutate GameState.

The complete v0.1 hook-specific algebra is authoritative in `RULE-QUERY-ALGEBRA.md`; generic priority is never a substitute for that algebra.

## BB-RSL-012 — ScheduledEffect
ScheduledEffect is serializable state with stable ID, source/cause, due lifecycle point, deterministic offset/counter, Effect payload, optional explicit snapshot, target selector/reference strategy, and invalid-target policy `SKIP|FAIL_RESOLUTION`.

No callback, closure, script body, or executable pointer is valid ScheduledEffect state.

## BB-RSL-013 — Stable state
BattleState is stable only when:
- no OperationGroup is active;
- Reaction queue is empty;
- no ScheduledEffect is due at the current lifecycle point;
- no action transaction is partially resolved.

Input and victory checkpoints occur only at stable states.

## BB-RSL-014 — Resolution transaction boundary
A player Action plus all synchronously caused Reaction/Operation chains up to the next stable state forms one action-resolution boundary for rollback/fault purposes. If bounded-resolution protection detects a cycle/overflow inside that boundary, the Battle restores the previous stable state and enters deterministic resolution-fault handling; it does not keep a partially resolved prefix.

## BB-RSL-015 — Fault codes
Resolution failures return stable semantic reason codes rather than presentation copy. Baseline classes include `ILLEGAL_ACTION`, `INVALID_OPERATION`, `INTERCEPTION_INVALID_REPLACEMENT`, `RESOLUTION_BUDGET_EXCEEDED`, `RESOLUTION_CYCLE_DETECTED`, `INVALID_TARGET_FAIL_POLICY`, `STATE_INVARIANT_VIOLATION`, and `RULE_QUERY_CONFLICT`.

Concrete user-facing messages are presentation/localization data.

## BB-RSL-016 — Cross-contract closure
Rule Query algebra is specified by `RULE-QUERY-ALGEBRA.md`. Resolution work-unit accounting, the 512-unit bound, canonical cycle projection/signature, rollback semantics, and golden verification obligations are specified by `BOUNDED-RESOLUTION.md` and `VERIFICATION-PLAN.md`.
