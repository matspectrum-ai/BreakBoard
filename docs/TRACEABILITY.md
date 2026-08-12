# Traceability Matrix

Planning state. Contract/test/implementation bindings remain absent until those artifacts exist.

| Requirement | Source | Contract | Verification | Implementation | Status |
|---|---|---|---|---|---|
| BB-CON-001..010 | Constitution | — | documentation audit | — | ACTIVE |
| BB-PRN-001..007 | Principles | — | design review | — | ACTIVE |
| BB-KRN-001..007 | Canonical Rules | future Resolution/Battle contracts | deterministic/property/termination tests | — | DESIGN SPECIFIED |
| BB-BRD-001..003 | Canonical Rules | future BoardState | board/property tests | — | PARTIAL / size hypothesis |
| BB-PCS-001..006 | Canonical Rules | future Piece/Movement | movement tests | — | SPECIFIED/PARTIAL balance |
| BB-TURN | Canonical Rules + Battle System | future Battle/Turn | lifecycle ordering tests | — | DESIGN SPECIFIED |
| BB-ACT-001..002 | Canonical Rules + Battle System | future Action/OperationGroup | transaction/interception tests | — | DESIGN SPECIFIED |
| BB-VIC-001..004 | Canonical Rules + Battle System | future Victory/BattleLifecycle | stable-checkpoint/termination tests | — | DESIGN SPECIFIED/PARAMETERS |
| BB-MUT-001..008 | Canonical Rules + Mutation System | future Mutation/Resolution | composition/property tests | — | DESIGN SPECIFIED |
| BB-MSYS-001..021 | Mutation System v0.1 | future Mutation/Resolution schemas | deterministic queue/interception/atomicity tests | — | DESIGN SPECIFIED |
| BB-BTL-001..016 | Battle System v0.1 | future BattleState/Action/Victory contracts | 24-scenario battle matrix + property tests | — | DESIGN SPECIFIED |
| BB-RNG-001..002 | Canonical Rules | future RNG | reproducibility tests | — | SPECIFIED |
| BB-META-001 | Canonical Rules | future progression | design/economy verification | — | PRINCIPLE / NEXT GATE |
| BB-RUN | Canonical Rules | future Run | seeded progression/run lifecycle tests | — | PARTIAL / NEXT GATE |

Before implementation unlock, every P0 implementation requirement must map to an authoritative specification, contract, and planned verification. After implementation begins, it must additionally map to code and passing verification.
