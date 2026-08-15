# Traceability Matrix

Planning state. Contract/test/implementation bindings remain absent until those artifacts exist.

| Requirement | Source | Contract | Verification | Implementation | Status |
|---|---|---|---|---|---|
| BB-CON-001..010 | Constitution | — | documentation audit | — | ACTIVE |
| BB-PRN-001..007 | Principles | — | design review | — | ACTIVE |
| BB-KRN-001..007 | Canonical Rules | future Resolution/Battle contracts | deterministic/property/termination tests | — | DESIGN SPECIFIED |
| BB-BRD/PCS/TURN/ACT/VIC | Canonical Rules + Battle System | future domain contracts | movement/lifecycle/victory tests | — | DESIGN SPECIFIED/PARAMETERS |
| BB-MUT / BB-MSYS | Canonical Rules + Mutation System | future Mutation/Resolution schemas | composition/interception/atomicity tests | — | DESIGN SPECIFIED |
| BB-BTL-001..016 | Battle System v0.1 | future BattleState/Action/Victory contracts | battle scenario/property tests | — | DESIGN SPECIFIED |
| BB-RUN-001..028 | Run System v0.1 | future Run/Profile/Generation contracts | seeded DAG/reward/progression tests | — | DESIGN SPECIFIED |
| BB-CONT-* | Content System + catalogs | future content schemas | schema + satisfiability + combinatorial validation | — | DESIGN SPECIFIED |
| BB-UX-001..027 | UX System v0.1 | future presentation/input contracts | interaction-state, legibility, accessibility, usability tests | — | DESIGN SPECIFIED |
| BB-ART-001..011 | Art Direction + Visual Grammar + Asset/VFX policies | future visual-token/presentation contracts | silhouette, grayscale, contrast, screenshot, cosmetic-integrity audits | — | DESIGN SPECIFIED |
| BB-RNG-001..003 | Canonical Rules + Run System | future RNG contracts | reproducibility tests | — | DESIGN SPECIFIED |
| BB-META-001..003 | Canonical Rules + Run System | future Profile contracts | progression-integrity tests | — | DESIGN SPECIFIED |
| BB-AUDIO (future) | Audio gate | future audio/haptic presentation contracts | semantic/audio-off/accessibility audits | — | CURRENT NEXT GATE |

Before implementation unlock, every P0 implementation requirement must map to an authoritative specification, contract, and planned verification. After implementation begins, it must additionally map to code and passing verification.
