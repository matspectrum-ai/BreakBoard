# Content Contracts v0.1

Status: **DRAFT — BB-CONTRACT-GATE-001 IN PROGRESS**

## BB-CC-001 — Canonical content document
Every content definition validates against a JSON Schema selected by `kind`. Shared envelope fields:
- `id: ContentId`;
- `kind`;
- `schema_version`;
- `status`;
- `tags[]`;
- `complexity_cost`;
- region/encounter eligibility;
- unlock/reward eligibility policy where applicable;
- presentation token references;
- mechanics payload expressed through canonical primitives.

Unknown gameplay fields are schema errors unless an explicit extension point exists.

## BB-CC-002 — Content ID immutability
A ContentId is never reused for semantically different behavior. Renaming presentation text does not require a new ID; changing mechanics in a way that changes historical deterministic generation/resolution requires a content/ruleset version decision.

## BB-CC-003 — Piece Mutation definition
A Piece Mutation definition declares at least:
- eligible archetypes/tags or unrestricted target rule;
- rarity;
- lifetime default;
- stacking policy;
- `requires`, `excludes`, optional legal `overrides`;
- zero or more Reactions;
- zero or more Modifiers;
- runtime-state schema when charges/once-per-battle state is required.

A Piece Mutation cannot declare FORCED removal or arbitrary executable code.

## BB-CC-004 — Rule Mutation definition
A Rule Mutation definition declares unique/stacking policy, run/battle lifetime, compatibility, and Modifier/Reaction mechanics. Modifier hooks must belong to the whitelisted Rule Query vocabulary.

## BB-CC-005 — Board Feature definition
Board Feature content declares feature-state schema, coexistence/exclusions, Reactions/Modifiers, link behavior if applicable, and presentation tokens. `Wall`-like non-occupiable semantics must be explicit rather than inferred from art/name.

## BB-CC-006 — EncounterTemplate definition
EncounterTemplate declares allowed regions, risk/reward class constraints, board configuration/features, enemy configuration/mutation budgets, complexity cost/budget contribution, first-side override if any, and compatibility/validity constraints. It cannot inject code branches.

## BB-CC-007 — Elite/Boss definition
Elite/Boss definitions are authored EncounterConfigurations plus identity/presentation metadata and reward policy allowed by Run contracts. Boss mechanics must still compile to canonical Battle/Mutation/Board/Lifecycle primitives; `boss_id` itself carries no executable behavior.

## BB-CC-008 — Event definition
Event content declares preconditions, ordered choices, explicit mechanical outcomes, optional deterministic generation selectors, and presentation keys. Every irreversible choice exposes its effect structurally. Hidden arbitrary script execution is forbidden.

## BB-CC-009 — Cross-reference validation
A content pack/version is invalid if any referenced ContentId, tag contract, Rule Query hook, selector, effect, presentation token required for gameplay readability, encounter template, unlock condition, or Boss/Event reference is unresolved or kind-incompatible.

## BB-CC-010 — Compatibility validation
Validators must prove `requires/excludes/overrides` references are type-valid and detect impossible self-contradictory definitions. Kernel Rules are not legal override targets.

## BB-CC-011 — Reward satisfiability validator
For the canonical starter profile and every reachable standard-run acquisition state under declared v0.1 pacing, validators must establish the minimum candidate guarantees from BB-CNT-009. This is an offline contract/content test, not a runtime fallback generator.

## BB-CC-012 — Complexity validation
Encounter generation must reject authored/generated configurations whose summed/declared complexity exceeds the region/encounter budget. Exact weights are balance data; enforcing the configured budget is contract behavior.

## BB-CC-013 — No content-specific engine branch audit
The verification suite includes a structural audit ensuring catalog behavior is representable through registered primitive kinds/hooks. A definition requiring a new primitive fails validation and triggers specification work; it is never patched with `if content_id == X` behavior.

## BB-CC-014 — Versioned schema families
Schema evolution is explicit. A content document records schema version independently from `content_version`: schema version describes data shape; content version describes the shipped semantic set. Migration between schema versions must preserve ContentId semantics.
