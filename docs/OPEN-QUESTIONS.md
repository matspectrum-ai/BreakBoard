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
| BB-OQ-051 | Exact resolution budget accounting and minimum bound? | RESOLVED: v0.1 = 512 deterministic work units per resolution boundary |
| BB-OQ-052 | Canonical cycle-signature inputs? | RESOLVED: canonical projection + RFC 8785-compatible JSON + SHA-256 lookup with canonical-byte equality confirmation |
| BB-OQ-053 | Exact RNG algorithm and namespace derivation algorithm? | RESOLVED by Architecture: `xoshiro128ss-v1` + `sha256-generation-v1`; golden vectors in architecture/RNG-SPEC.md |
| BB-OQ-054 | What exact atomic save/recovery protocol is used by the Tauri adapter? | OPEN / Architecture persistence micro-gate |
| BB-OQ-055 | What exact content authoring/build/runtime validation pipeline is canonical? | OPEN / Architecture content-pipeline micro-gate |
| BB-OQ-056 | What minimum WebView/browser/Tauri QA matrix gates desktop release? | OPEN / Architecture packaging micro-gate |

## Architecture baseline now resolved
The implementation candidate is TypeScript strict + PixiJS 8/WebGL + semantic DOM/CSS + Tauri 2. Authoritative game rules stay in a pure TypeScript domain independent of renderer/DOM/Tauri. JSON Schema validation uses Ajv2020; RED-first tests use Vitest plus fast-check. RNG is xoshiro128** with SHA-256-derived independent namespaces and published golden vectors. Production implementation remains locked while Architecture is IN_PROGRESS.
