# Traceability Matrix

Planning state. Formal contract/test/implementation bindings are the current gate.

| Requirement | Source | Contract | Verification | Implementation | Status |
|---|---|---|---|---|---|
| BB-CON-001..010 | Constitution | current Contract gate | documentation/governance audit | — | ACTIVE |
| BB-PRN-001..007 | Principles | current Contract gate | design review | — | ACTIVE |
| BB-KRN-001..007 | Canonical Rules | current Resolution/Battle contracts | deterministic/property/termination tests | — | DESIGN SPECIFIED |
| BB-BRD/PCS/TURN/ACT/VIC | Canonical Rules + Battle System | current domain contracts | movement/lifecycle/victory tests | — | DESIGN SPECIFIED/PARAMETERS |
| BB-MUT / BB-MSYS | Canonical Rules + Mutation System | current Mutation/Resolution contracts | composition/interception/atomicity tests | — | DESIGN SPECIFIED |
| BB-BTL-001..016 | Battle System v0.1 | current BattleState/Action/Victory contracts | battle scenario/property tests | — | DESIGN SPECIFIED |
| BB-RUN-001..028 | Run System v0.1 | current Run/Profile/Generation contracts | seeded DAG/reward/progression tests | — | DESIGN SPECIFIED |
| BB-CONT-* | Content System + catalogs | current content schemas | schema + satisfiability + combinatorial validation | — | DESIGN SPECIFIED |
| BB-UX-001..027 | UX System v0.1 | future presentation/input implementation contracts | interaction-state, legibility, accessibility, usability tests | — | DESIGN SPECIFIED |
| BB-ART-001..011 | Art Direction + Visual Grammar + Asset/VFX policies | future visual-token/presentation contracts | silhouette, grayscale, contrast, screenshot, cosmetic-integrity audits | — | DESIGN SPECIFIED |
| BB-AUD-001..010 / BB-MUS / BB-HAP / BB-AAS | Audio/Music/Haptic specs | future presentation/audio contracts | semantic, aggregation, mute-all, mono, accessibility audits | — | DESIGN SPECIFIED |
| BB-RNG-001..003 | Canonical Rules + Run System | current RNG namespace contract | reproducibility/isolation tests | — | DESIGN SPECIFIED |
| BB-META-001..003 | Canonical Rules + Run System | current Profile contracts | progression-integrity tests | — | DESIGN SPECIFIED |
| BB-CONTRACT (current) | Contract gate | canonical formal contracts | RED-first verification plan | — | CURRENT GATE |

Before implementation unlock, every P0 implementation requirement must map to an authoritative specification, formal contract, and planned verification. Implementation remains locked throughout this gate.
