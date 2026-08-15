# Bounded Resolution & Cycle Detection v0.1

Status: **SPECIFIED — BB-CONTRACT-GATE-001 CLOSED**

## Objective
Make BB-KRN-003 / BB-MSYS-019 executable and deterministic: every action-resolution boundary either reaches a stable state or faults without retaining a partially resolved prefix.

## BB-BND-001 — Resolution boundary
A resolution boundary begins from one canonical stable BattleState immediately before a state-changing lifecycle/action operation and ends when the battle reaches the next stable checkpoint or deterministic resolution fault.

Before work begins, the resolver captures a rollback snapshot containing all gameplay-authoritative state, including GameState, Battle lifecycle counters relevant to the boundary, RNG cursors, Mutation runtime state/charges, action allowance, ScheduledEffects, and event-sequence counter.

## BB-BND-002 — Resolution work units
The v0.1 budget counts deterministic work units:
- `GROUP_ATTEMPT`: one OperationGroup proposal/interception/revalidation pass; a REPLACE that requires a new interception pass consumes another GROUP_ATTEMPT;
- `REACTION_RESOLVE`: dequeue and materialize one queued Reaction;
- `SCHEDULED_ACTIVATE`: activate one due ScheduledEffect into resolution work.

Pure Conditions/Selectors and individual Operations inside one already-counted OperationGroup do not each consume separate budget units; they remain bounded by finite state/content collections and their own schema constraints.

## BB-BND-003 — v0.1 budget
Maximum work units per resolution boundary: **512**.

The limit is Kernel/Battle safety configuration, not a Mutation/Rule Query. Content cannot raise, remove, or intercept it. Changing this value after v0.1 is a ruleset/contract version decision, not silent balance data.

Before executing a work unit that would make the counter exceed 512, resolution faults with `RESOLUTION_BUDGET_EXCEEDED`.

## BB-BND-004 — Cycle checkpoint
After every completed work unit and before selecting the next work unit, the resolver constructs a canonical **cycle projection** of the unresolved boundary.

The projection includes all semantics that can affect future deterministic resolution:
- canonical gameplay state including positions, lifecycle, mutations/charges, action allowance, Collapse and ScheduledEffect state;
- all gameplay RNG namespace cursors/state used by the battle;
- ordered pending Reaction descriptors;
- ordered due/immediate ScheduledEffect descriptors;
- pending OperationGroup/interception stage if one exists;
- current lifecycle/action boundary semantic identity.

## BB-BND-005 — Excluded from cycle projection
The projection excludes values that are monotonic bookkeeping but cannot alter future gameplay semantics:
- EventId/OperationId/OperationGroupId identity values;
- event sequence number itself;
- timestamps/wall clock;
- presentation/audio/VFX/haptic state;
- debug/log identifiers.

For queued reactions, triggering-event semantic kind/payload/cause is included, while the EventId is not.

## BB-BND-006 — Canonical representation
Before signature calculation:
- maps/objects are represented with canonical key ordering;
- semantic sets are sorted using BB-CTR-003;
- ordered queues retain their canonical queue order;
- irrelevant serialization whitespace/field order is eliminated.

Canonical bytes use JSON Canonicalization Scheme semantics (RFC 8785-compatible canonical JSON). A SHA-256 digest is used as the lookup key; on digest collision, canonical bytes must also compare equal before declaring a repeated projection. Hash collision alone is never gameplay authority.

## BB-BND-007 — Cycle detection
The first cycle projection at boundary start is recorded. After each work unit, if the canonical projection exactly matches any prior projection in the same resolution boundary, resolution faults with `RESOLUTION_CYCLE_DETECTED` before executing further work.

An identical projection including identical RNG cursors and pending work is a deterministic proof that continuing would repeat the same future resolution behavior.

## BB-BND-008 — Rollback semantics
On budget/cycle/semantic resolution fault:
1. discard all provisional events/IDs/presentation intents produced inside the boundary;
2. restore the exact rollback snapshot;
3. restore RNG cursors, charges, action allowance, ScheduledEffects, event sequence and all other authoritative counters;
4. set Battle lifecycle to `FAULTED` with immutable FaultRecord;
5. do not evaluate the partially resolved state for victory;
6. propagate the system fault according to Run fault handling, not Player defeat.

The previous stable gameplay state is retained for diagnostics/reproduction but is no longer an input-accepting ACTIVE BattleState after FAULTED transition.

## BB-BND-009 — Fault record
FaultRecord contains at least:
- stable reason code;
- battle/run generation identity references;
- action/lifecycle boundary descriptor;
- work-unit count;
- repeated cycle digest when applicable;
- ruleset/content versions.

It must not contain presentation-dependent data required to reproduce the fault.

## BB-BND-010 — No content interception
Resolution safety faults, budget accounting, cycle projection, rollback, and FAULTED transition expose no Mutation Reaction/Modifier hook. Content cannot cancel or replace them.

## Required golden verification
- finite Explosive/Split chain reaches stable state under 512;
- intentionally self-recreating reaction loop repeats canonical projection and faults as cycle;
- non-cyclic long chain exceeding 512 faults as budget overflow;
- both failures restore GameState, RNG cursors, charges, action allowance, ScheduledEffects and event counter exactly;
- changing presentation/audio/settings does not change cycle signature;
- changing RNG cursor does change cycle projection when future random behavior can differ.
