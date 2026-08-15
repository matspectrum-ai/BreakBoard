# Procedural VFX & Presentation System v0.1

Status: canonical presentation semantics; renderer-independent.

## Separation
Game logic emits deterministic resolved state/events. Presentation consumes them. Animation/VFX never decide rules, timing, RNG, legality, victory, or resolution ordering. Skipping/accelerating animation cannot change GameState.

## VFX vocabulary
- Move: short directional streak + origin feedback.
- Capture: impact line + target fracture/particle burst.
- Death: geometric fragmentation/dissolve.
- Spawn: inverse assembly/convergence.
- Portal: compress/transfer/reconstruct with linked endpoint pulse.
- Tile modify: surface/glyph transition.
- Tile destroy: structural fracture then plate removal.
- Mutation trigger: compact glyph + short label near affected object.
- Rule trigger: law glyph/label near rule/action surface.
- Collapse: pre-warned boundary closes inward, affected ring fractures/removes atomically in presentation.
- Victory/defeat: controlled major transition after canonical terminal state.

## Resolution chains
Presentation follows deterministic event/reaction order rather than firing all effects simultaneously. Long chains may auto-accelerate or use user-configured animation speed while preserving comprehensibility.

## Motion accessibility
Reduced-motion mode replaces large translations/shakes/complex particles with direct slides, fades, short impacts, dissolves, and simple tile removal while preserving semantic feedback.

## High-contrast behavior
High-contrast mode reduces decorative particles/background detail/glow and increases piece/tile/state outline and luminance separation.

## Timing
Exact milliseconds are deferred. Presentation categories are `very_short`, `short`, and `medium`; no normal gameplay feedback requires a long unskippable animation.
