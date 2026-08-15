# Audio System v0.1

Status: **SPECIFIED — BB-AUDIO-GATE-001 CLOSED**

## Objective
Define an engine-independent semantic audio model for BreakBoard. Audio reinforces canonical state and causality but is never required to understand or play the game.

## BB-AUD-001 — Presentation-only audio
Domain logic emits canonical events. A presentation mapper derives `AudioIntent`, `VFXIntent`, and optional `HapticIntent`. Audio playback, duration, skipped sounds, or mute state may never mutate GameState, consume gameplay RNG, alter event ordering, or delay logical resolution.

## BB-AUD-002 — Semantic taxonomy
Canonical audio-intent families are `UI`, `BATTLE`, `SYSTEM`, `WORLD`, and `RESULT`.

Baseline intents include selection/confirm/cancel/invalid; Move/Capture/Death/Spawn/Teleport/Pass; Mutation/Rule triggers and action gain; Board Feature and Collapse feedback; Victory/Defeat/Double Break/Unlock/Run Complete.

## BB-AUD-003 — Battle vocabulary
Move uses a short origin/motion/destination grammar. Capture is stronger than Move and combines impact plus target break. Death communicates geometric fragmentation; Spawn uses an inverse assembly grammar; Portal transfer uses phase/transfer/arrival grammar. Archetype differences may parameterize shared families rather than require bespoke systems.

## BB-AUD-004 — Mutation and Rule sonic grammar
Piece/Rule content does not require one bespoke sound per content ID. Semantic trigger families are:
- `PROTECTION`;
- `DESTRUCTION`;
- `MOVEMENT`;
- `DUPLICATION`;
- `TRANSFORMATION`;
- `RESURRECTION`;
- `ACTION_GAIN`;
- `RULE_REWRITE`.

Mutation-specific identity may vary parameters within a family. Rule triggers use a broader/systemic presentation than local Piece Mutation triggers.

## BB-AUD-005 — Board Feature grammar
Baseline semantic cues:
- Fragile: sharp interior crack;
- Wall: dense structural impact;
- Portal: phase pulse;
- Conveyor: directional mechanical pulse;
- Void: low unstable texture;
- Sanctuary: restrained protective harmonic cue;
- Brittle: granular perimeter crumble;
- Beacon: rising pulse.

Fragile and Brittle must remain perceptually distinguishable without being unique one-off sound systems.

## BB-AUD-006 — Aggregation and priority
Reaction chains must remain legible. Presentation may coalesce repeated low-level sounds while preserving canonical facts in visual/history presentation.

Priority classes: `CRITICAL`, `HIGH`, `NORMAL`, `LOW`.
Critical includes Core death, Collapse step, major Boss rule, Victory, Defeat, and Double Break. High includes Capture and Mutation/Rule triggers. Normal includes Move/Spawn/tile modification. Low includes non-essential UI and ambience.

A bounded audio voice budget is required. Lower-priority voices may be dropped/coalesced first; critical semantic cues must not be displaced by decorative playback.

## BB-AUD-007 — Collapse and Boss semantics
Collapse has a recognizable warning cue and an aggregated structural Collapse-step cue rather than a full destruction sound for every tile. Core events retain critical priority during Collapse.

Boss identity uses reusable music/audio vocabulary plus authored motifs, not mandatory bespoke voice acting or character sound systems.

## BB-AUD-008 — UI and result feedback
UI audio is minimal: select, confirm, cancel, invalid, panel transition, reward reveal, unlock. Hover/micro-navigation must not create continuous sonic clutter by default.

Piece reward, Rare reward, and Rule reward may differ in hierarchy while remaining visually understandable without sound. Victory, Defeat, Double Break, and Run Complete have distinct result semantics.

## BB-AUD-009 — Audio-independent gameplay
With all audio muted, the player must still perceive turn authority, legality, Mutation/Rule triggers, Portal state, Collapse warning, Core events, Boss mechanics, Victory, Defeat, and Double Break through canonical visual/UX channels.

Required settings baseline: Master, Music, SFX, and UI volume; mute-all; normal/reduced dynamic-range modes. Playback must remain semantically usable in mono; spatial panning is optional reinforcement only.

## BB-AUD-010 — Accessibility combinations
`audio disabled + reduced motion` remains a supported gameplay configuration. Removing these presentation channels cannot remove critical information. Failure of this condition is a UX/accessibility defect, not justification to make audio mandatory.

## Structural validation
Validated cases include Explosive multi-death chain aggregation, Collapse with multiple removals/Core death priority, three simultaneous Mutation triggers, Boss rule feedback, mute-all playability, mono playback, reduced dynamic range, and reduced-motion + mute operation.
