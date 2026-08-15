# Rule Query Composition Algebra v0.1

Status: **SPECIFIED candidate — BB-CONTRACT-GATE-001**

## Objective
Give every whitelisted Modifier hook a deterministic hook-specific composition algebra. Generic priority alone is not gameplay semantics.

## BB-RQ-001 — Modifier authority key
When a hook requires winner selection, modifier authority is ordered:
1. Kernel constraint (not content-overridable);
2. Rule Mutation;
3. Board Mutation/Feature;
4. Piece Mutation;
then within the same layer:
5. higher declared integer `priority` first;
6. earlier activation sequence first;
7. stable ContentId lexicographically;
8. stable instance ID as final runtime tie-break.

Content modifier priority is a signed integer in `[-1000, 1000]`, default `0`. Two same-authority `REPLACE/SET` contributions with identical precedence but different semantic values are a validation error; runtime never chooses arbitrarily.

## BB-RQ-002 — Kernel constraints
Each hook may define non-overridable Kernel validity conditions. No `ALLOW`, `ADD`, or `REPLACE` contribution may bypass Kernel atomicity, entity existence, coordinate-domain validity, stable-state requirements, or other BB-KRN concerns.

## BB-RQ-003 — MovementPattern
Pattern is a canonical set/expression of movement primitives.
1. start from archetype base pattern;
2. if active `REPLACE` contributions exist, highest-authority REPLACE becomes working base;
3. collect per-pattern-element `ADD`/`REMOVE` contributions;
4. when the same semantic element is both added and removed, highest-authority contribution for that element wins;
5. canonicalize/deduplicate result.

This supports Ascendant replacement and Sidewinder/Revolution additive movement without name-specific logic.

## BB-RQ-004 — CapturePattern
Same algebra as MovementPattern, using capture primitives. Pattern composition does not itself decide target ownership or Sanctuary-like restrictions; those belong to CaptureValidity.

## BB-RQ-005 — MovementRange
Range value is `FINITE(n>=0)` or `UNBOUNDED`.
1. start from base range;
2. highest-authority `REPLACE` if any;
3. apply all active integer `ADD` deltas (UNBOUNDED remains UNBOUNDED);
4. apply `MIN` constraints using the greatest minimum;
5. apply `MAX` constraints using the least maximum; MAX may cap UNBOUNDED;
6. floor finite result at zero.

If effective MIN exceeds effective MAX, composition is `RULE_QUERY_CONFLICT`; content validation should prevent static conflicts, while conditional runtime conflicts deterministically fault rather than choose arbitrarily.

## BB-RQ-006 — MovementBlocking
Working value is a set of blocker categories relevant to path traversal.
1. start from base blocker categories;
2. for each blocker category, consider active `ADD_BLOCKER` and `REMOVE_BLOCKER` contributions;
3. highest-authority contribution for that category wins;
4. categories with no contribution retain base behavior.

Thus Ghost may remove `PIECE` as a blocker and Riftwalker may remove `MISSING_TILE_GAP` where the movement primitive supports traversal across a coordinate gap. Destination validity remains separate.

## BB-RQ-007 — DestinationValidity
Final result uses **deny-wins constraints**:
`kernel_valid AND (base_valid OR any(ALLOW_IF)) AND NOT(any(DENY_IF))`.

ALLOW_IF may create an explicit exception to modifiable base rules but never Kernel constraints. Any active DENY_IF makes the destination invalid regardless of lower/higher content priority.

## BB-RQ-008 — CaptureValidity
Same deny-wins algebra as DestinationValidity:
`kernel_valid AND (base_valid OR any(ALLOW_IF)) AND NOT(any(DENY_IF))`.

This permits Blood Price to ALLOW allied non-Core capture while Sanctuary contributes DENY_IF. Sanctuary therefore wins without a bespoke interaction rule.

## BB-RQ-009 — AvailableActions
Working value is a set of action descriptors/kinds.
1. start from base available actions;
2. union all `ADD_ACTION` contributions;
3. remove every action matched by any `REMOVE_ACTION` contribution.

Removal is deny-wins. Battle Lifecycle may independently synthesize Pass when no legal non-Pass Primary Action exists; Pass is not made permanently available by this hook.

## BB-RQ-010 — PrimaryActionCount
Working value is a nonnegative integer allowance baseline.
1. base count;
2. highest-authority `SET` if any;
3. sum all `ADD` deltas;
4. apply greatest `MIN` constraint;
5. apply least `MAX` constraint;
6. floor at zero.

`GrantAction` is a state-changing Operation that adjusts current action allowance and is not retroactively reinterpreted as a Modifier contribution.

## BB-RQ-011 — TileTraversability
For an existing Tile, final traversability uses deny-wins:
`kernel_valid AND (base_traversable OR any(ALLOW_IF)) AND NOT(any(DENY_IF))`.

Missing topology is not an existing Tile and cannot be made present by this hook. Gap-crossing mechanics are expressed through MovementPattern/MovementBlocking, not fake tile creation.

## BB-RQ-012 — TileOccupancy
Occupancy policy contains capacity plus optional occupant predicates.
1. start from base capacity (v0.1 baseline 1);
2. highest-authority `SET_CAPACITY` if any;
3. sum `ADD_CAPACITY` deltas;
4. apply greatest `MIN_CAPACITY` and least `MAX_CAPACITY`;
5. floor capacity at zero;
6. apply all `DENY_OCCUPANT_IF` predicates; denial wins.

Contradictory MIN/MAX is `RULE_QUERY_CONFLICT`.

## BB-RQ-013 — CoreClassification
Final classification is:
`(base_core OR any(ADD_CORE)) AND NOT(any(REMOVE_CORE))`.

`REMOVE_CORE` is deny-wins. This allows Crowned/Twin Crown to add Core classification while future explicit declassification rules remain deterministic. Victory always evaluates the resolved classification at the stable checkpoint.

## BB-RQ-014 — Composition purity
Rule Query evaluation is pure: no RNG draw, no state mutation, no Event emission, no charge consumption. If content requires one-time consumption, the consumption occurs through a separate Reaction/Operation after the relevant committed event.

## BB-RQ-015 — Validation obligations
Content validation must reject unknown contribution types, out-of-range priority, incompatible payloads, static contradictory range/capacity constraints, illegal Kernel override attempts, and ambiguous identical-precedence REPLACE/SET values.

Runtime conditional conflicts return stable `RULE_QUERY_CONFLICT` and transition through deterministic fault policy rather than arbitrary resolution.
