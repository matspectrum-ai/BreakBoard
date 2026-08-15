# Traceability Matrix

Planning state. Product/system design, P0 formal contracts and Architecture v0.1 are closed. Production implementation remains absent and explicitly unauthorized.

| Requirement | Source | Contract / Architecture | Planned Verification | Implementation | Status |
|---|---|---|---|---|---|
| BB-CON-001..010 | Constitution | Domain/Architecture governance | governance + traceability audit | — | CONTRACTED |
| BB-PRN-001..007 | Principles | Domain + Content contracts | design/content branch audit | — | CONTRACTED |
| BB-KRN-001..007 | Canonical Rules | STATE + RESOLUTION + BOUNDED | deterministic/property/termination | — | CONTRACTED |
| BB-BRD/PCS | Canonical + Battle | DOMAIN-MODEL + STATE | schema/order/state properties | — | CONTRACTED |
| BB-TURN/ACT/VIC | Battle System | STATE + RESOLUTION | BTL-ACT/STABLE/VIC | — | CONTRACTED |
| BB-MUT / BB-MSYS | Mutation System | RESOLUTION + RULE-QUERY + CONTENT | RSL-ATOM + RQ-* + scenarios | — | CONTRACTED |
| BB-BTL-001..016 | Battle System | STATE + RESOLUTION + BOUNDED | battle/scenario/property suites | — | CONTRACTED |
| BB-RUN-001..028 | Run System | STATE + RNG/Persistence | RUN-RNG/GEN/REW + persistence | — | CONTRACTED |
| BB-CONT-* | Content System | CONTENT-CONTRACTS + CONTENT-PIPELINE | schema/xref/satisfiability/reproducible bundle | — | CONTRACTED + ARCHITECTED |
| BB-RNG | Canonical + Run | RNG-PERSISTENCE + RNG-SPEC | golden vectors + namespace isolation | — | CONTRACTED + ARCHITECTED |
| BB-META | Canonical + Run | Profile/Run boundaries | progression-integrity | — | CONTRACTED |
| BB-UX | UX System | PRESENTATION-BRIDGE + DOM/Pixi boundary | browser interaction/accessibility | — | SPECIFIED + ARCHITECTED |
| BB-ART | Art/VFX | presentation-only boundaries | visual/readability audits | — | SPECIFIED + ARCHITECTED |
| BB-AUD/MUS/HAP | Audio specs | presentation bridge | mute/aggregation/accessibility | — | SPECIFIED + ARCHITECTED |
| BB-PER stable Run | Run + Persistence contract | ADR-002 immutable-generation protocol | Rust fault injection + round trip/recovery | — | P0 ARCHITECTED |
| BB-PER mid-battle | Open P1 policy | isolated adapter boundary | future pre-release tests | — | P1 OPEN |
| Architecture boundaries | MODULE-BOUNDARIES + BOUNDARY-ENFORCEMENT | TS refs + dependency-cruiser + lint | boundary CI | — | ARCHITECTED |
| Test execution | Verification Plan | TEST-HARNESS | Vitest/fast-check/browser/Tauri suites | — | ARCHITECTED |
| Desktop distribution | ADR-001 + PACKAGING-QA | Tauri/WebGL platform boundary | packaged platform smoke/E2E | — | ARCHITECTED |
| BB-CONTRACT-GATE-001 | contracts/* + schemas | P0 audit | P0-TRACEABILITY-AUDIT | — | CLOSED / PASS |
| BB-ARCH-GATE-001 | architecture/* | Implementation Readiness Audit | architecture consistency/readiness audit | — | CLOSED / PASS |
| BB-IMPL-GATE-001 | next gate | all above | begin with named RED tests | — | READY_NOT_STARTED / USER HOLD |

## Readiness result
No P0 implementation-entry blocker remains. `implementation_ready=true`, but `implementation_authorized=false` and `implementation_unlock=false`. The absence of implementation is intentional, not an incomplete Architecture gate.
