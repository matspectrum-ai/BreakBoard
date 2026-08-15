# Open Questions

Agents must not resolve open questions implicitly.

| ID | Question | Status |
|---|---|---|
| BB-OQ-001 | Is 6×6 optimal? | OPEN / balance playtest; 6×6 standard hypothesis |
| BB-OQ-005 | Exact Collapse numeric threshold/cadence? | OPEN / balance playtest |
| BB-OQ-012 | Final reward weighting/Rare rates? | OPEN / balance data |
| BB-OQ-014 | Battle-duration target? | OPEN / playtest |
| BB-OQ-015 | Final classification of all balance parameters vs immutable rules? | OPEN / ongoing |
| BB-OQ-033 | Mid-battle interruption/recovery player-facing policy? | OPEN P1 / required before release-readiness; not an implementation-entry blocker; no implicit retry/loss behavior |
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
| BB-OQ-044 | Exact palette/font/material/shader values? | OPEN / visual tokens + production polish |
| BB-OQ-045 | Audio vocabulary/adaptive music model? | RESOLVED by Audio/Music v0.1 |
| BB-OQ-046 | Are haptics required at launch? | RESOLVED: optional presentation abstraction; desktop baseline does not require hardware |
| BB-OQ-047 | Exact audio formats/levels/mastering? | OPEN / production polish |
| BB-OQ-048 | Canonical machine-readable formal schema notation? | RESOLVED: JSON Schema 2020-12 + normative semantic invariants/tests |
| BB-OQ-049 | Persistence priority classification? | RESOLVED: stable Run persistence P0; mid-battle policy P1 before release-readiness |
| BB-OQ-050 | Exact composition algebra for each v0.1 Rule Query hook? | RESOLVED by RULE-QUERY-ALGEBRA.md |
| BB-OQ-051 | Exact resolution budget accounting and bound? | RESOLVED: 512 deterministic work units per resolution boundary |
| BB-OQ-052 | Canonical cycle-signature inputs? | RESOLVED: canonical projection + RFC-8785-compatible JSON + SHA-256 lookup/byte equality |
| BB-OQ-053 | Exact RNG/namespace derivation? | RESOLVED: xoshiro128ss-v1 + sha256-generation-v1; golden vectors published |
| BB-OQ-054 | Exact atomic stable Run save/recovery protocol? | RESOLVED by ADR-002: immutable generation files, digest validation, newest-valid recovery, minimum two valid generations retained |
| BB-OQ-055 | Exact content authoring/build/runtime validation pipeline? | RESOLVED by CONTENT-PIPELINE.md: restricted YAML/JSON -> Ajv/schema -> semantic/whole-catalog validation -> deterministic canonical bundle/manifest -> fail-closed runtime load |
| BB-OQ-056 | Minimum WebView/browser/Tauri QA architecture? | RESOLVED by PACKAGING-QA.md: Node + Chromium/WebKit browser CI plus actual packaged Tauri platform smoke/E2E; WebGL v0.1 baseline |
| BB-OQ-057 | Exact mid-battle crash/recovery gameplay policy? | OPEN P1 / must close before release-readiness; architecture isolates it from stable Run persistence |

## Readiness baseline
Architecture is closed. All P0 semantic and architecture questions required to **begin** implementation are resolved or explicitly parameterized. Remaining open items are balance/playtest, production polish, or P1 release-readiness work. Implementation is nevertheless LOCKED until explicit user authorization.
