# Traceability Matrix

Planning state. Contract/test/implementation bindings remain absent until those artifacts exist.

| Requirement | Source | Contract | Verification | Implementation | Status |
|---|---|---|---|---|---|
| BB-CON-001..010 | Constitution | — | documentation audit | — | ACTIVE |
| BB-PRN-001..007 | Principles | — | design review | — | ACTIVE |
| BB-KRN-001..007 | Canonical Rules | future Resolution/Battle contracts | deterministic/property/termination tests | — | SPECIFIED/PARTIAL PARAMETERS |
| BB-BRD-001..003 | Canonical Rules | future BoardState | board/property tests | — | PARTIAL |
| BB-PCS-001..006 | Canonical Rules | future Piece/Movement | movement tests | — | SPECIFIED/PARTIAL |
| BB-TURN | Canonical Rules | future Battle/Turn contract | lifecycle ordering tests | — | PARTIAL / CURRENT NEXT GATE |
| BB-ACT-001..002 | Canonical Rules | future Action/OperationGroup | transaction/interception tests | — | SPECIFIED |
| BB-VIC-001..004 | Canonical Rules | future Victory/BattleLifecycle | termination tests | — | PARTIAL / CURRENT NEXT GATE |
| BB-MUT-001..008 | Canonical Rules + Mutation System | future Mutation/Resolution contracts | mutation composition/property tests | — | DESIGN SPECIFIED |
| BB-MSYS-001..021 | Mutation System v0.1 | future Mutation/Resolution schemas | deterministic queue, interception, atomicity, stacking, selector, modifier tests | — | DESIGN SPECIFIED |
| BB-RNG-001..002 | Canonical Rules | future RNG | reproducibility tests | — | SPECIFIED |
| BB-META-001 | Canonical Rules | future progression | design/economy verification | — | PRINCIPLE |
| BB-RUN | Canonical Rules | future Run | progression tests | — | PARTIAL |

Before implementation unlock, every P0 implementation requirement must map to an authoritative specification, contract, and planned verification. After implementation begins, it must additionally map to code and passing verification.
