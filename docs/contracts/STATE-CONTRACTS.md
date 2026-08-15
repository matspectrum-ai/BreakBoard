# State Contracts v0.1

Status: **SPECIFIED — BB-CONTRACT-GATE-001 CLOSED**

## BB-ST-001 — GameState
`GameState` is the deterministic tactical snapshot nested inside one BattleState. It contains only gameplay-authoritative data required to validate/resolve the battle:
- board/topology;
- sides/orientations;
- PieceState registry;
- active side;
- turn/round counters and action allowance;
- active Piece/Rule/Board Mutation instances;
- ScheduledEffects;
- Collapse state;
- battle RNG namespace cursors needed by deterministic resolution.

A stable GameState has no active OperationGroup and no queued Reaction awaiting resolution.

## BB-ST-002 — GameState invariants
At every committed stable GameState:
1. entity IDs are unique within their semantic scope;
2. every ACTIVE piece references one valid Side and one existing Tile;
3. every REMOVED piece has no Coordinate;
4. no two ACTIVE pieces violate resolved `TileOccupancy`;
5. every MutationInstance references an existing definition and valid owner/target for its scope;
6. ScheduledEffects are serializable declarative records with deterministic due metadata;
7. active-side reference is valid while battle lifecycle accepts turns;
8. no partial OperationGroup is observable;
9. all links reference valid declared endpoints or are explicitly inert by content rule;
10. schema validity does not suppress terminal victory evaluation.

## BB-ST-003 — BattleState
`BattleState` contains:
- `battle_id`;
- immutable provenance: encounter/config IDs, ruleset/content version, battle generation context;
- lifecycle phase: `SETUP|STARTING|ACTIVE|RESOLVING|ENDED|FAULTED`;
- current `GameState`;
- resolution metadata (`stable|resolving`, queue metadata, action-resolution boundary);
- result when terminal;
- monotonic domain event sequence number.

Presentation history may mirror emitted events but is not authoritative BattleState.

## BB-ST-004 — Battle terminal result
Terminal result is a discriminated value with at least:
- `PLAYER_VICTORY`;
- `PLAYER_DEFEAT`;
- `DOUBLE_BREAK` (single-player outcome maps to defeat but remains a distinct reason);
- `FAULT` with deterministic reason code.

Once `ENDED`, no gameplay Action is legal.

## BB-ST-005 — RunGenerationContext
Run generation context is immutable after run creation and contains at least:
- user-visible run seed;
- `content_version`;
- `ruleset_version`;
- eligible-content/unlock snapshot identity;
- `rng_algorithm_id`;
- stable namespace-derivation version.

Changing ProfileState later cannot mutate this context.

## BB-ST-006 — RunState
`RunState` contains:
- `run_id` and generation context;
- status `CREATED|ACTIVE|COMPLETED|FAILED|ABANDONED|FAULTED`;
- region graph/progress and current node;
- run-persistent army slots and Piece Mutation assignments;
- active run Rule Mutations;
- completed encounter history;
- generated/persisted RewardOffers and selections;
- boss/result history;
- deterministic run-generation cursors/state that are not derivable from stable namespace identity.

RunState does not store finished BattleState snapshots as gameplay authority.

## BB-ST-007 — ArmySlot
A run army slot contains stable `piece_slot_id`, base archetype/config, run-persistent Piece Mutation references, and explicit run-lifetime transform state if a specified mechanic requires it. Battle-local death/position/status never deletes or corrupts the slot.

## BB-ST-008 — RewardOffer
A RewardOffer contains:
- stable `reward_offer_id`;
- generation namespace/context;
- reward class;
- ordered candidate ContentIds;
- generation-complete marker;
- optional selected candidate;
- optional target/replacement selection state;
- status `UNRESOLVED|RESOLVED|DECLINED` only where declining is explicitly allowed.

Once generated, candidate IDs are immutable. UI reopen/save/load cannot regenerate them.

## BB-ST-009 — ProfileState
Gameplay ProfileState contains cross-run progression only:
- `profile_id`;
- schema/version metadata;
- unlocked content IDs;
- discovery/achievement state;
- cosmetic entitlements/selections whose gameplay semantics remain neutral;
- aggregate progression history needed by unlock conditions.

ProfileState may not contain permanent raw combat modifiers forbidden by BB-META.

## BB-ST-010 — Profile/Run/Battle dependency direction
Canonical creation direction is:
`ProfileState -> RunGenerationContext/RunState -> BattleState`.

No Battle mutation writes ProfileState directly. Meta unlock evaluation occurs only through an explicit post-run/profile progression transaction.

## BB-ST-011 — Stable persistence checkpoints
A persisted RunState is valid only at an explicitly named stable run checkpoint: before encounter entry, after encounter resolution, while a generated RewardOffer is unresolved, or after reward resolution/route choice according to the Run contract. Partial battle-resolution state is not a Run save checkpoint.

## BB-ST-012 — Serialization authority
Serialization is a representation of these contracts, not the contracts themselves. Loading must reconstruct an equivalent state that passes schema validation plus semantic invariants before it becomes authoritative.
