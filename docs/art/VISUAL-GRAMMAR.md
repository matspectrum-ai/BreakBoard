# BreakBoard Visual Grammar v0.1

Status: canonical companion to `ART-DIRECTION.md`.

## Board Features
Each feature must remain distinguishable in grayscale/high-contrast contexts through structure/pattern:

| Feature | Canonical visual grammar |
|---|---|
| Fragile | fracture lines originating inside tile surface |
| Wall | elevated/solid block replacing normal traversable surface impression |
| Portal | linked-ring glyph/pulse; endpoints share stable link marker; incomplete link is visibly inert |
| Conveyor | directional arrow integrated into tile surface |
| Void | negative-space/distorted center with strong border discontinuity |
| Sanctuary | protective concentric/shield-like rings distinct from Portal |
| Brittle | fractured/perforated perimeter rather than center cracks |
| Beacon | radiating/starburst signal pattern |

Destroyed tile: no plate/surface exists at that topology coordinate.

## Piece state overlays
- Core: persistent Core Halo, independent of archetype.
- Mutation count: compact pips/slots; full mutation icons appear only in Inspect/details.
- Owner: redundant accent + surface/pattern/orientation semantics.
- Selected: dedicated outline/marker not confused with ownership/Core.

## Icon grammar
Containers:
- Piece Mutation: circular family.
- Rule Mutation: hexagonal/law family.
- Board Feature: diamond/square structural family.

Initial glyph semantics:

### Piece Mutations
Armored=shield; Explosive=fractured burst; Ghost=broken outline; Blink=double-position; Chain=linked arrows; Anchor=locked base; Breaker=broken tile; Swapper=crossing arrows; Sidewinder=lateral arrow; Riftwalker=line crossing gap; Fragile Trail=trail+crack; Sentinel=shield beside Core halo; Afterlife=returning broken ring; Split=one-to-two; Crowned=Core halo/crown status; Wallmaker=piece-to-wall; Parasite=nested/copy; Echo Step=repeated movement arrow; Portalist=linked rings; Ascendant=Pawn with radial directions.

### Rule Mutations
Revolution=Pawn+lateral arrows; Momentum=capture arrow+plus; Scorched Origins=capture+broken origin; Twin Crown=two Core halos; Open Lines=line through allied blockers; Afterlife Pact=multiple returning rings; Blood Price=allied-capture mark; Shattered Law=board+fracture.

## Interaction states
Legal move, capture, ability target, danger, selected, disabled/illegal, and Collapse warning require distinct shape/pattern/icon semantics; color may reinforce but never be sole carrier.

## Core asset budget
Launch target: 5 base piece shapes, 20 Piece Mutation glyphs, 8 Rule glyphs, 8 Board Feature glyphs/patterns, 4 encounter-category icons, 3 Boss emblems: approximately 48 core reusable definitions plus procedural materials/VFX/UI components.
