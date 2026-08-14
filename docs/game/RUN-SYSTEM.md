# Run & Progression System v0.1

Status: **SPECIFIED — BB-RUN-GATE-001 CLOSED**

## Objective
Define how battles compose into complete deterministic roguelite runs with branching routes, mutation-driven builds, controlled pacing, bosses/elites/events, seeded generation, horizontal meta-progression, and no mandatory economy subsystem.

## BB-RUN-001 — Run lifecycle
Conceptual RunState fields include run identity, generation context, status, region, current node, route graph, army/build state, Piece/Rule Mutations, encounter/reward history, boss history, and result.

Lifecycle states: `CREATED`, `ACTIVE`, `COMPLETED`, `FAILED`, `ABANDONED`, `FAULTED`.

A battle defeat or Double Break transitions the active run to FAILED. Voluntary exit is ABANDONED. Final Boss victory transitions to COMPLETED. FAULTED is a deterministic system/generation failure, not normal player defeat.

## BB-RUN-002 — Lifetime hierarchy
State lifetimes are explicitly separated:
- ProfileState persists across runs.
- RunState persists across encounters within one run.
- BattleState exists only for one battle.

ProfileState snapshots eligible unlocks into RunGenerationContext at run creation. Subsequent profile changes do not mutate an active run.

## BB-RUN-003 — Run-persistent state
Persist through the run by default:
- army composition;
- Piece Mutations;
- Rule Mutations;
- run seed / derived generation identity;
- route progress;
- encounter history;
- reward history.

Do not persist between battles by default:
- destroyed/modified battle topology;
- battle-only Board Mutations;
- temporary effects;
- battle ScheduledEffects;
- battle-local positions;
- Collapse state;
- turn state.

Base v0.1 has no army permadeath between battles; standard army composition is restored for subsequent encounters, then run-persistent mutations/configuration are applied.

## BB-RUN-004 — Standard run pacing baseline
Current balance baseline: three regions. Each region uses approximately 4–5 intermediate route layers plus one Boss node, yielding roughly 15–18 completed encounters for a full standard run depending on path and configuration.

Three-region pacing intent:
1. Region I — BUILD: establish direction with low complexity.
2. Region II — SYNERGY: combine mutations and board mechanics.
3. Region III — BREAK: mature builds face multi-system interactions and final boss pressure.

Exact counts/durations remain balance parameters, not Kernel Rules.

## BB-RUN-005 — Region graph
Each region is a deterministic layered directed acyclic graph (DAG), not a free-form map.

Baseline graph constraints:
- exactly one region entry;
- exactly one region Boss;
- approximately 4–5 intermediate layers;
- approximately 2–3 nodes per intermediate layer;
- every non-Boss reachable node has at least one outgoing edge;
- every non-entry reachable node has at least one incoming edge;
- every reachable node can reach the Boss;
- no cycles;
- no orphan reachable nodes;
- deterministic generation from region generation context.

The player traverses one node per layer. Graph invalidity is a generation fault; a run never starts with an invalid region graph.

## BB-RUN-006 — Route information
Before route choice, the player can inspect at least node category, risk class, and reward class. Full encounter composition need not be revealed. Route decisions must not be blind RNG choices.

## BB-RUN-007 — Encounter categories
Canonical v0.1 encounter categories are:
- `BATTLE`;
- `ELITE`;
- `EVENT`;
- `BOSS`.

Mutation is a reward type, not a separate encounter category.

## BB-RUN-008 — Region variety constraints
Generation must prevent degenerate repetition. Baseline region constraints:
- at least two normal Battle opportunities;
- one or two Event opportunities;
- at most one Elite opportunity per region baseline;
- exactly one Boss;
- Elite is never the only route forward.

Exact counts are balance parameters, but Elite optionality and route diversity are design requirements.

## BB-RUN-009 — Encounter generation
Encounter configuration is derived from:
`EncounterTemplate + RegionConstraints + ComplexityBudget + deterministic seed namespace + validity context`.

Player build context may be used to prevent impossible/invalid encounters, but normal procedural generation must not secretly hard-counter the player build. Explicit bosses may define intentional counters as authored mechanics.

## BB-RUN-010 — Encounter templates
Procedural generation is template-constrained. Templates declare allowed regions, board features, enemy mutation budgets/tags, complexity cost, compatibility rules, and other declarative configuration. Free-form generation with arbitrary bespoke logic is outside the v0.1 design.

## BB-RUN-011 — Complexity budget
Each encounter has a complexity budget. Mechanics/templates carry complexity costs. Region I targets low complexity, Region II medium, Region III high. Generation may not exceed the configured budget.

Difficulty escalation is primarily mechanical complexity/synergy, not permanent percentage-stat inflation.

## BB-RUN-012 — Anti-repetition
RunState records encounter/reward history. Baseline constraints include no identical encounter template consecutively and no duplicate Boss within a standard run. Recent board/template patterns may receive deterministic weighting penalties. Anti-repetition changes selection weights; it does not introduce unseeded randomness.

## BB-RUN-013 — Starter reward
Before the first region, standard run baseline generates three eligible Piece Mutation candidates and the player chooses one, then chooses an eligible owned Piece target. `3 choose 1` is a balance parameter; the starter reward category is Piece Mutation.

## BB-RUN-014 — Reward classes
Baseline reward mapping:
- normal Battle victory → Piece Mutation offer;
- Elite victory → enhanced Piece Mutation offer with elevated Rare eligibility;
- Region I/II Boss victory → Rule Mutation offer;
- Final Boss victory → no build reward; run completes and meta evaluation begins.

Board Mutations are encounter/battle configuration in v0.1, not persistent player inventory/build rewards.

## BB-RUN-015 — Mutation rarity baseline
Initial persistent Piece Mutation rarity vocabulary is `COMMON` and `RARE`. Rare means more transformative, specialized, or complex, not necessarily a direct numerical upgrade. Additional rarity tiers are not part of v0.1.

## BB-RUN-016 — Reward generation
A RewardOffer is generated once and persisted with stable identity and candidate set. Opening/closing UI, save/resume, or re-reading the reward cannot reroll candidates.

Generation order:
1. build eligible content pool from the run unlock snapshot;
2. filter by category/reward class;
3. enforce `requires`, `excludes`, target/capacity eligibility, and encounter pool constraints;
4. apply deterministic weighting/anti-repetition;
5. select candidates from the dedicated reward RNG namespace;
6. persist the offer;
7. await explicit player selection.

## BB-RUN-017 — Piece Mutation acquisition
For a Piece Mutation reward:
1. choose candidate;
2. choose eligible owned Piece target;
3. validate compatibility;
4. if capacity available, install;
5. if full, require explicit replacement choice;
6. confirm atomic replacement/addition.

No existing mutation is silently evicted. Cancelling selection/replacement leaves run state unchanged and the persisted offer unresolved.

## BB-RUN-018 — Rule Mutation acquisition
Rule Mutation rewards apply to RunState and affect subsequent eligible battles. If Rule Mutation capacity is full, explicit replacement is required. No silent eviction occurs.

## BB-RUN-019 — Voluntary removal
Base v0.1 has no free arbitrary mutation removal during a run. Removal happens only through explicit replacement, Event outcome, or specified Mutation effect.

## BB-RUN-020 — Events
Events are declarative content with conditions, presentation, choices, and deterministic/seeded outcomes. Events may add/replace/remove/transform mutations, alter upcoming encounter state, or reveal route information.

Preferred design principle: randomness determines which option/event appears; the player determines the chosen outcome. Hidden coin-flip punishment is not the default progression model.

## BB-RUN-021 — Elite semantics
Elite encounters are optional player-chosen risk. They use higher mechanical complexity and provide enhanced Piece Mutation reward opportunity. An Elite cannot be a mandatory sole route in the standard graph.

## BB-RUN-022 — Boss semantics
Boss identity should primarily be defined by battle rules/mechanical behavior rather than expensive bespoke asset requirements. Region Boss victory gates progression to the next region and, for non-final region bosses, produces a Rule Mutation reward. Final Boss victory completes the run.

## BB-RUN-023 — Seed namespaces
Run generation does not rely on one fragile global sequential RNG stream. Deterministic namespaces derive independent sub-seeds/streams from stable identity, conceptually including:
- `route.region.<id>`;
- `encounter.<node_id>`;
- `battle.<node_id>`;
- `reward.<node_id>`;
- `event.<node_id>`;
- `boss.<boss_id>`.

Changing RNG consumption inside one namespace must not arbitrarily perturb unrelated generation domains.

## BB-RUN-024 — Generation identity
Reproducibility context is not only a user-visible seed. A deterministic run generation identity includes at least:
- run seed;
- content version;
- unlock snapshot / eligible-content snapshot.

The same generation identity produces the same graph and deterministic content inputs. Cross-version identical-seed behavior is not guaranteed unless the required content version remains available.

## BB-RUN-025 — Bounded deterministic generation
Graph/encounter generation uses bounded deterministic attempts. If constraints cannot be satisfied within the configured deterministic attempt budget, generation returns a fault rather than silently relaxing invariants or looping forever.

## BB-RUN-026 — Save/resume baseline
Run save/resume is required at stable run checkpoints such as before/after encounters and during persisted reward selection. Mid-battle save is not part of this v0.1 run contract; battle interruption/recovery policy is deferred.

## BB-RUN-027 — Meta progression
Meta progression is horizontal. ProfileState may unlock new Mutations, Events, Bosses, encounter templates/modifiers, challenge modes, cosmetic themes, or explicitly specified alternative starting configurations. Base v0.1 forbids permanent raw combat-stat progression as the core meta loop.

Unlocks are preferably driven by explicit achievements/conditions rather than an XP-to-power ladder.

## BB-RUN-028 — Monetization boundary
Core progression integrity baseline:
Allowed examples: cosmetic board themes, cosmetic piece themes, cosmetic effects, cosmetic profile presentation, soundtrack/supporter content.
Forbidden in the base design: paid permanent power, paid combat advantage, paid in-run rerolls, paid extra mutation slots, or paid seed manipulation.

Future paid expansions/content packs require separate product specification and may not silently violate the horizontal-progression contract.

## Structural validation matrix
Run System v0.1 structurally validates deterministic route generation, branching paths, DAG/no cycles, Boss reachability, Elite optionality, battle victory/defeat outcomes, Piece/Rule persistence, Board reset, army restoration, persisted reward offers, no reload reroll, compatibility filtering, capacity replacement, reward classes, Final Boss completion, horizontal meta, seed namespaces, content version/unlock snapshot, bounded generation, save/resume checkpoints, route visibility, complexity budgets, and anti-repetition.

## Deferred details
The following do not reopen this design gate: final run duration, exact region/layer/node counts, final reward offer count, final mutation capacity values, concrete graph generation algorithm/data structures, exact weighting tables, concrete content catalog, final save serialization, mid-battle recovery, final achievement catalog, pricing/commerce implementation, and platform/store integration.
