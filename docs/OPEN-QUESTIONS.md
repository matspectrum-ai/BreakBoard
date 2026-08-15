# Open Questions

Agents must not resolve open questions implicitly.

| ID | Question | Status |
|---|---|---|
| BB-OQ-001 | Is 6×6 optimal? | OPEN / balance playtest; 6×6 standard hypothesis |
| BB-OQ-005 | Exact Collapse numeric threshold/cadence? | OPEN / balance playtest |
| BB-OQ-012 | Final reward weighting/Rare rates? | OPEN / balance data |
| BB-OQ-014 | Battle-duration target? | OPEN / playtest |
| BB-OQ-015 | Final classification of all balance parameters vs immutable rules? | OPEN / ongoing |
| BB-OQ-033 | Mid-battle interruption/recovery policy? | RESOLVED for Contract gate classification: P1 for implementation unlock, mandatory before release-readiness; no implicit retry/loss policy canonical yet |
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
| BB-OQ-049 | Exact persistence/migration recovery guarantees for interrupted battles? | RESOLVED classification: stable Run save/migration is P0; mid-battle recovery is P1 before implementation unlock and required before release-readiness |
| BB-OQ-050 | Exact composition algebra for each v0.1 Rule Query hook? | RESOLVED by RULE-QUERY-ALGEBRA.md |
| BB-OQ-051 | Exact resolution budget accounting and minimum bound? | RESOLVED: v0.1 = 512 deterministic work units per resolution boundary; work-unit definitions in BOUNDED-RESOLUTION.md |
| BB-OQ-052 | Canonical cycle-signature inputs for deterministic cycle detection? | RESOLVED: canonical cycle projection + RFC 8785-compatible canonical JSON + SHA-256 lookup with canonical-byte equality confirmation |
| BB-OQ-053 | Exact RNG algorithm and namespace derivation algorithm? | DEFERRED to Architecture gate; stable algorithm IDs, derivation version and golden vectors mandatory before production implementation unlock |

## Contract baseline now resolved
Formal contracts use typed semantic identities; stable fallback ordering is authored order → lifecycle/activation sequence → coordinate order → stable ID. `PieceSlotId` persists across a Run while Battle `PieceId` is battle-local. GameState/BattleState/RunState/ProfileState have explicit aggregate boundaries. Action legality is pure. OperationGroup is atomic. FORCED removal is Kernel/Lifecycle-only. Events are immutable after commit. All v0.1 Rule Query hooks have explicit algebra. Bounded resolution uses a 512-unit budget plus canonical cycle detection and full rollback. RewardOffers persist after generation. Saves validate/migrate explicitly before authority. RED-first verification and P0 traceability artifacts exist.
