# Traceability Matrix

Planning state. P0 formal contracts and RED-first verification planning are closed; production implementation remains absent.

| Requirement | Source | Formal Contract | Planned Verification | Implementation | Status |
|---|---|---|---|---|---|
| BB-CON-001..010 | Constitution | DOMAIN-MODEL + VERIFICATION-PLAN | governance/traceability audit | — | CONTRACTED |
| BB-PRN-001..007 | Principles | DOMAIN-MODEL + CONTENT-CONTRACTS | design/content branch audit | — | CONTRACTED |
| BB-KRN-001..007 | Canonical Rules | STATE + RESOLUTION + BOUNDED | deterministic/property/termination + RSL-BOUND | — | CONTRACTED |
| BB-BRD/PCS | Canonical + Battle | DOMAIN-MODEL + STATE | CTR-SCHEMA/ORD + board/piece properties | — | CONTRACTED |
| BB-TURN/ACT/VIC | Battle System | STATE + RESOLUTION | BTL-ACT/STABLE/VIC | — | CONTRACTED |
| BB-MUT / BB-MSYS | Mutation System | RESOLUTION + RULE-QUERY-ALGEBRA + CONTENT-CONTRACTS | RSL-ATOM + RQ-* + mutation scenarios | — | CONTRACTED |
| BB-BTL-001..016 | Battle System v0.1 | STATE + RESOLUTION + BOUNDED | BTL + RSL scenario/property set | — | CONTRACTED |
| BB-RUN-001..028 | Run System v0.1 | STATE + RNG-PERSISTENCE | RUN-RNG/GEN/REW + persistence | — | CONTRACTED |
| BB-CONT-* | Content System + catalogs | CONTENT-CONTRACTS + content schema | CNT-REF/COMP/SAT/CPLX/BRANCH | — | CONTRACTED |
| BB-RNG-001..003 | Canonical + Run | RNG-PERSISTENCE | RUN-RNG + Architecture golden RNG vectors | — | CONTRACTED / ALGORITHM NEXT GATE |
| BB-META-001..003 | Canonical + Run | STATE Profile/Run boundaries | progression-integrity tests | — | CONTRACTED |
| BB-UX-001..027 | UX System | domain events/rejection codes + future presentation implementation contracts | interaction/accessibility tests | — | DESIGN SPECIFIED |
| BB-ART-001..011 | Art System | presentation-only boundary in DOMAIN-MODEL | visual/readability audits | — | DESIGN SPECIFIED |
| BB-AUD / BB-MUS / BB-HAP / BB-AAS | Audio specs | presentation-only boundary in DOMAIN-MODEL | mute-all/semantic/accessibility audits | — | DESIGN SPECIFIED |
| BB-PER | Run + Contract gate | RNG-PERSISTENCE | PER-RT/MIG/VER/XREF/ATOMIC | — | P0 CONTRACTED; MID-BATTLE P1 |
| BB-CONTRACT-GATE-001 | Contract gate | docs/contracts/* + specs/contracts/* | P0-TRACEABILITY-AUDIT | — | CLOSED / PASS |
| BB-ARCH-GATE-001 | Architecture & Technology Selection | next gate | architecture decision verification + RNG golden vectors + harness proof | — | CURRENT NEXT GATE |

## Gate closure result
P0 audit is PASS. Remaining open values are balance/playtest parameters, presentation/production details, P1 mid-battle recovery, or architecture decisions such as exact RNG algorithm. No unresolved P0 game-semantic question remains.
