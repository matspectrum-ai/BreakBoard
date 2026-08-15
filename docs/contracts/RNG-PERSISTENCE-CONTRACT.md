# RNG & Persistence Contract v0.1

Status: **DRAFT — BB-CONTRACT-GATE-001 IN PROGRESS**

## BB-RNGC-001 — No ambient randomness
Gameplay and procedural generation may obtain randomness only from an explicit RNG context. Process-global random APIs, wall-clock time, object hash iteration, platform entropy, and presentation randomness are forbidden as gameplay authority.

## BB-RNGC-002 — GenerationIdentity
A Run GenerationIdentity contains at least:
- run seed;
- content version;
- ruleset version;
- unlock/eligible-content snapshot identity;
- `rng_algorithm_id`;
- `namespace_derivation_version`.

Seed alone is not a complete reproducibility identity.

## BB-RNGC-003 — Namespace derivation
Independent deterministic namespaces isolate unrelated generation domains. Required conceptual roots include:
- `route.region.<region_id>`;
- `encounter.<node_id>`;
- `battle.<node_id>`;
- `reward.<reward_or_node_id>`;
- `event.<node_id>`;
- `boss.<boss_id>`;
- battle-local selector namespaces where randomness is explicitly required.

Changing draw count in one namespace must not alter another namespace's sequence.

## BB-RNGC-004 — RNG cursor
A stateful RNG namespace, when stateful draws are required, is represented by stable namespace identity plus deterministic draw index/state. Persistence must preserve enough information to continue exactly. Reload cannot repeat or skip a consumed gameplay draw.

## BB-RNGC-005 — Algorithm identity
The exact PRNG/derivation algorithm may be chosen during the Architecture/Technology gate, but its stable algorithm identifier and golden vectors become part of the contract before production implementation unlock. Changing algorithm identity is a versioned compatibility change, never a silent implementation swap.

## BB-RNGC-006 — Canonical seed material
Seed input is normalized by a versioned deterministic normalization contract before namespace derivation. The normalization version is persisted. Presentation formatting/case rules may not silently change seed meaning.

## BB-PER-001 — Persisted envelope
Every authoritative persisted Profile/Run payload uses an envelope containing at least:
- schema family/type;
- schema version;
- content version;
- ruleset version where relevant;
- RNG algorithm/namespace version where relevant;
- payload;
- integrity/check metadata if the selected architecture supplies it.

## BB-PER-002 — Validate before authority
Load pipeline is conceptually:
1. parse envelope;
2. recognize supported versions;
3. apply explicit migration chain if required;
4. validate JSON Schema;
5. validate semantic invariants and cross-references;
6. only then expose state as authoritative.

Best-effort loading that silently drops unknown/invalid gameplay fields is forbidden.

## BB-PER-003 — Migrations
A migration is an explicit version-to-version transformation with fixtures proving deterministic output. Migration may not invent player choices, reroll offers, or reinterpret a stable ContentId as a different mechanic.

If a state cannot be migrated without violating semantics, loading fails with an explicit compatibility result.

## BB-PER-004 — Content compatibility
A persisted active run references the content/ruleset identity with which it was created. A newer executable may resume it only if that identity is available or an explicit compatibility/migration contract exists. Active runs are never silently regenerated under a different content pool.

## BB-PER-005 — Stable run saves
Run save writes occur only at stable Run checkpoints defined by BB-ST-011. RewardOffers are persisted after generation and before user resolution, so reopening cannot reroll candidates.

## BB-PER-006 — Atomic persistence expectation
The persistence adapter selected later must provide an all-or-nothing replacement strategy for authoritative save updates or an equivalent recovery protocol. A partially written save must not be treated as valid state.

## BB-PER-007 — Mid-battle recovery status
Mid-battle serialization is not required by the current v0.1 Run contract. Exact crash/interruption recovery semantics remain **OPEN** under BB-OQ-049 and are not silently inferred here. This item must be classified P0/P1 before implementation unlock.

## BB-PER-008 — Presentation/settings separation
Non-gameplay settings such as volume, graphics preferences, input bindings, camera position, and panel state may use separate persistence. Corruption or migration of presentation settings must not mutate authoritative Profile/Run gameplay state.

## Verification obligations
- same complete GenerationIdentity => same golden generated outputs;
- namespace isolation property tests;
- save/load round-trip semantic equality;
- unresolved RewardOffer round-trip retains exact candidates/order;
- migration fixture determinism;
- unsupported future version fails explicitly;
- invalid cross-reference never becomes authoritative state;
- simulated interrupted write never yields a partially accepted gameplay state.
