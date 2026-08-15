# Open Questions

Agents must not resolve open questions implicitly.

| ID | Question | Status |
|---|---|---|
| BB-OQ-001 | Is 6×6 optimal? | OPEN / balance playtest; 6×6 standard hypothesis |
| BB-OQ-005 | Exact Collapse numeric threshold/cadence? | OPEN / balance playtest |
| BB-OQ-012 | Final reward weighting/Rare rates? | OPEN / balance data |
| BB-OQ-014 | Battle-duration target? | OPEN / playtest |
| BB-OQ-015 | Final classification of all balance parameters vs immutable rules? | OPEN / ongoing |
| BB-OQ-033 | Mid-battle interruption/recovery policy? | OPEN / persistence/product contract |
| BB-OQ-034 | Final mutation capacity values? | OPEN / balance playtest |
| BB-OQ-035 | Final run duration and route counts? | OPEN / playtest |
| BB-OQ-036 | Is perfect information permanent for v0.1? | RESOLVED: yes; hidden-information content requires a new contract |
| BB-OQ-037 | Full deterministic outcome preview or direct consequences? | RESOLVED: full legality/direct consequences; final reaction-chain state hidden by default |
| BB-OQ-038 | Resolution feed always visible? | RESOLVED: latest concise summary visible; full history expandable |
| BB-OQ-039 | Is mobile a launch target? | RESOLVED baseline: desktop launch target; touch-compatible architecture required; mobile release uncommitted |
| BB-OQ-040 | What final visual art direction defines BreakBoard's identity? | RESOLVED v0.1: Broken Geometry |
| BB-OQ-041 | Which visual properties encode player/enemy ownership and Core status while remaining accessible? | RESOLVED: redundant ownership encoding; Core Halo independent of archetype |
| BB-OQ-042 | What icon grammar represents Piece Mutations, Rules, and Board Features coherently? | RESOLVED baseline by Visual Grammar |
| BB-OQ-043 | Which visual assets may be AI-generated vs procedural/code-generated vs manually curated? | RESOLVED by Art Asset Policy |
| BB-OQ-044 | Exact palette/font/material/shader values? | OPEN / visual tokens + technology/polish |
| BB-OQ-045 | What audio event vocabulary and adaptive music model are required? | RESOLVED by Audio/Music v0.1: semantic intents, family grammar, priority/coalescing, adaptive presentation states/stems |
| BB-OQ-046 | Are haptics required at launch or merely supported by abstraction? | RESOLVED: optional presentation abstraction; desktop launch baseline does not require hardware |
| BB-OQ-047 | Exact audio file formats, middleware, voice budget, levels, and mastering values? | OPEN / technology + production polish |
| BB-OQ-048 | What machine-readable formal schema notation becomes canonical for domain/content contracts? | OPEN / Contract gate; do not choose a programming language implicitly |
| BB-OQ-049 | What exact persistence/migration recovery guarantees are required for active runs and interrupted battles? | OPEN / Contract + product planning |

## Resolved baseline
Canonical mechanics and presentation intent are repository-defined. Mutation, Battle, Run, Content, UX, Art, and Audio v0.1 design gates are closed. Audio and haptics are presentation-only; critical information remains available with audio disabled. Music is adaptive presentation rather than a gameplay controller. Production audio uses semantic families and documented provenance rather than one bespoke asset per content ID.
