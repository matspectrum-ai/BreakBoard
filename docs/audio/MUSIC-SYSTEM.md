# Music System v0.1

Status: specified under `BB-AUDIO-GATE-001`.

## BB-MUS-001 — Direction
Music follows Broken Geometry: restrained electronic/synthetic material, precise pulses, fractured rhythm, and progressively unstable texture. Generic medieval-chess scoring and constant high-intensity EDM are outside the baseline direction.

## BB-MUS-002 — Presentation states
Canonical conceptual music states are `MAIN_MENU`, `ROUTE`, `ENCOUNTER_PREVIEW`, `BATTLE_LOW`, `BATTLE_HIGH`, `COLLAPSE`, `BOSS`, `VICTORY`, `DEFEAT`, and `RUN_COMPLETE`. These are presentation states, not gameplay states, and need not map one-to-one to separate audio files.

## BB-MUS-003 — Adaptive layering
Battle music should support reusable layers/stems such as `HARMONIC`, `PULSE`, and `PERCUSSION`. Collapse and Boss presentation may add/transform layers without restarting the logical battle. Music transitions never control gameplay timing.

## BB-MUS-004 — Run escalation
Region I (BUILD) is sparse/precise; Region II (SYNERGY) adds interlocking layers; Region III (BREAK) increases fracture/instability. Reuse of motifs is preferred over requiring complete independent scores for every region.

## BB-MUS-005 — Boss motifs
The Architect uses precise grid/mechanical motifs; Chain Sovereign uses linked rhythmic impulses; The Void uses subtraction, filtering, low unstable texture, and Collapse vocabulary. Boss identity requires a motif/system treatment, not necessarily a full exclusive composition.

## BB-MUS-006 — Accessibility and settings
Music can be independently muted. No music state carries exclusive gameplay information. Adaptive intensity may consider presentation-safe inputs such as boss active, Collapse state, region, battle phase, or active-Core pressure, but may not influence domain logic.
