# Open Questions

Agents must not resolve open questions implicitly.

| ID | Question | Status |
|---|---|---|
| BB-OQ-001 | Is 6×6 optimal? | OPEN / balance playtest; 6×6 standard hypothesis |
| BB-OQ-005 | Exact Collapse numeric threshold/cadence? | OPEN / balance playtest |
| BB-OQ-012 | Final reward weighting/Rare rates? | OPEN / balance data |
| BB-OQ-014 | Battle-duration target? | OPEN / playtest |
| BB-OQ-015 | Final classification of all balance parameters vs immutable rules? | OPEN / ongoing |
| BB-OQ-033 | Mid-battle interruption/recovery policy? | OPEN; must be classified P0/P1 before Contract gate closure |
| BB-OQ-034 | Final mutation capacity values? | OPEN / balance playtest |
| BB-OQ-035 | Final run duration and route counts? | OPEN / playtest |
| BB-OQ-036 | Is perfect information permanent for v0.1? | RESOLVED: yes; hidden-information content requires a new contract |
| BB-OQ-037 | Full deterministic outcome preview or direct consequences? | RESOLVED: full legality/direct consequences; final reaction-chain state hidden by default |
| BB-OQ-038 | Resolution feed always visible? | RESOLVED: latest concise summary visible; full history expandable |
| BB-OQ-039 | Is mobile a launch target? | RESOLVED baseline: desktop launch target; touch-compatible architecture required; mobile release uncommitted |
| BB-OQ-040 | What final visual art direction defines BreakBoard's identity? | RESOLVED v0.1: Broken Geometry |
| BB-OQ-041 | Ownership/Core accessible encoding? | RESOLVED: redundant ownership encoding; Core Halo independent of archetype |
| BB-OQ-042 | Icon grammar? | RESOLVED baseline by Visual Grammar |
| BB-OQ-043 | AI/procedural/manual asset boundary? | RESOLVED by Art Asset Policy |
| BB-OQ-044 | Exact palette/font/material/shader values? | OPEN / visual tokens + technology/polish |
| BB-OQ-045 | Audio vocabulary/adaptive music model? | RESOLVED by Audio/Music v0.1 |
| BB-OQ-046 | Are haptics required at launch? | RESOLVED: optional presentation abstraction; desktop baseline does not require hardware |
| BB-OQ-047 | Exact audio formats/middleware/voice budget/mastering? | OPEN / technology + production polish |
| BB-OQ-048 | Canonical machine-readable formal schema notation? | RESOLVED: JSON Schema 2020-12 for data shape + normative semantic invariants/tests; YAML/JSON documents may decode to same model |
| BB-OQ-049 | Exact persistence/migration recovery guarantees for interrupted battles? | PARTIAL: stable Run saves and explicit migrations specified; mid-battle interruption semantics still open |
| BB-OQ-050 | Exact composition algebra for each v0.1 Rule Query hook? | OPEN / blocking Contract micro-gate |
| BB-OQ-051 | Exact resolution budget accounting and minimum bound? | OPEN / blocking Contract micro-gate |
| BB-OQ-052 | Canonical cycle-signature inputs for deterministic cycle detection? | OPEN / blocking Contract micro-gate |
| BB-OQ-053 | Exact RNG algorithm and namespace derivation algorithm? | DEFERRED to Architecture gate, but stable `rng_algorithm_id`, derivation version and golden vectors are mandatory before implementation unlock |

## Contract baseline now resolved
Formal contracts use typed semantic identities; stable fallback ordering is authored order → activation/lifecycle sequence → row/column coordinate order → stable ID; `PieceSlotId` persists across a Run while Battle `PieceId` is battle-local; GameState/BattleState/RunState/ProfileState have explicit aggregate boundaries; legality evaluation is pure; OperationGroup is the atomic mutation boundary; FORCED removal is Kernel/Lifecycle-only; committed Events are immutable; RewardOffers persist after generation; saves must validate/migrate explicitly before becoming authoritative; RED-first verification IDs now exist.
