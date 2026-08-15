# Content System & Initial Content Set v0.1

Status: **SPECIFIED — BB-CONTENT-GATE-001 CLOSED**
Content version: `0.1.0`

## Objective
Define the minimum systemic launch content required for BreakBoard to be a complete roguelite without introducing content-specific engine branches.

## BB-CNT-001 — Content envelope
Every content definition has a stable ID, kind, status, complexity cost, tags, region eligibility, unlock policy, presentation tokens, and mechanics expressed only through existing system primitives.

Canonical kinds: `piece_mutation`, `rule_mutation`, `board_feature`, `encounter_template`, `event`, `elite`, `boss`.

Stable IDs are never reused for semantically different content. Any material semantic change that breaks deterministic historical generation requires a content-version change.

## BB-CNT-002 — No bespoke engine branches
No content item may require engine logic equivalent to `if content_id == X`. Content is composed from Mutation, Battle, Run, Board, Reward, Unlock, ScheduledEffect, Rule Query, and Operation primitives already specified.

## BB-CNT-003 — Presentation independence
Content may reference declarative `icon_token`, `vfx_token`, `palette_token`, text/localization keys, and other presentation tokens, but gameplay behavior cannot depend on bespoke art assets. Missing cosmetic presentation must not alter mechanics.

## BB-CNT-004 — Rarity
Persistent Piece Mutation rarity vocabulary is `COMMON` and `RARE`. Rare means more transformative/specialized/complex, not guaranteed numerical superiority.

## BB-CNT-005 — Mutation uniqueness/stacking
All initial Piece Mutations use `stacking: prohibited` on the same Piece unless explicitly stated otherwise. The same Piece Mutation ID may exist on multiple different eligible Pieces. All initial Rule Mutations are unique within one Run and use `stacking: prohibited`.

## BB-CNT-006 — Eligibility
Content eligibility is explicit and deterministic. Piece Mutation candidate eligibility requires at least one owned Piece target satisfying archetype/scope constraints and compatibility, including explicit replacement when capacity is full. Unlock status controls player reward eligibility, not authored enemy/encounter use unless an authored definition explicitly says otherwise.

## BB-CNT-007 — Board-feature coexistence
Board Features coexist unless an explicit exclusion applies. `Wall` excludes all features requiring occupiable/traversable tile semantics. Other initial features may compose, including Portal+Fragile, Sanctuary+Void, and Beacon+Void. Composition resolves through existing event/operation ordering.

## BB-CNT-008 — Portal safety
Portal endpoints share a `link_id`. Voluntary entry emits a portal transfer MovePiece to the linked valid endpoint. The transfer carries `cause=portal_transfer` and cannot trigger another Portal transfer during that same transfer operation. If a link has fewer than two valid endpoints, remaining endpoints are inert until the link is valid again.

## BB-CNT-009 — Reward satisfiability
Starter reward validation must guarantee at least three distinct eligible Piece Mutation IDs for a fresh standard profile/run. Standard normal/Elite reward validation must guarantee at least three meaningful eligible Piece Mutation candidates for every reachable standard-run acquisition state under v0.1 pacing. Region I/II Boss reward validation must guarantee at least three eligible Rule Mutation candidates under the starter Rule pool and all standard event flows.

If future content/progression changes invalidate these guarantees, the content gate must be revisited; runtime must not silently invent fallback content.

## BB-CNT-010 — Region complexity
Baseline normal-encounter complexity budgets: Region I `0–4`, Region II `3–7`, Region III `5–10`. Exact numeric tuning remains a balance parameter, but generation may not exceed configured budget.

## BB-CNT-011 — Initial launch set
Initial systemic definitions:
- 20 Piece Mutations;
- 8 Rule Mutations;
- 8 Board Features;
- 10 Encounter Templates;
- 6 Events;
- 6 Elite presets;
- 3 Bosses;
- 5 base Piece archetypes defined by Canonical/Battle rules.

The systemic content catalog totals 61 non-archetype content definitions across the categories above.

## BB-CNT-012 — Starter profile
Initial player reward pool unlocks Piece Mutations BB-PM-001..012 and Rule Mutations BB-RM-001, BB-RM-002, BB-RM-003, BB-RM-005. Board Features, encounter templates, and standard Bosses are world/content availability, not player reward unlocks.

## BB-CNT-013 — Event grants vs unlocks
An authored Event may explicitly grant a Mutation that is not yet in the player's normal reward pool. This does not automatically unlock that Mutation for future reward generation unless the Event separately grants the profile unlock.

## BB-CNT-014 — Authored enemy access
Enemy, Elite, and Boss configurations may use valid content regardless of whether the player has unlocked that content for rewards. This supports mechanical discovery without creating permanent power advantage.

## BB-CNT-015 — Content validation
Before implementation unlock, each content definition must pass schema/reference validity, target eligibility, compatibility, deterministic ordering, region/complexity constraints, and representative interaction tests. Definitions that require a new primitive are rejected or trigger a prior-system specification change; they are not implemented as exceptions.

## Catalogs
- `PIECE-MUTATIONS.md`
- `RULE-BOARD-CATALOG.md`
- `ENCOUNTERS-AND-BOSSES.md`
- `EVENTS-AND-UNLOCKS.md`

## Structural acceptance
Validated areas include stable IDs, rarity, stacking, target eligibility, compatibility, Portal recursion safety, occupied-tile semantics, Event outcomes, Boss configurations, starter reward satisfiability, standard Rule reward satisfiability, region eligibility, complexity budgets, enemy access to locked content, and composition across representative Piece+Rule+Board interactions.

## Deferred details
Concrete serialization schema, localization copy, final icon/VFX assets, final balance weights, exact numeric reward weights, final coordinates for every procedural board pattern, and accessibility/UX presentation do not reopen this gate unless they change gameplay semantics.
