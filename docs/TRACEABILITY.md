# Traceability Matrix

Planning bootstrap. Contract/test/implementation bindings intentionally remain absent until those artifacts exist.

| Requirement | Source | Contract | Verification | Implementation | Status |
|---|---|---|---|---|---|
| BB-CON-001..010 | Constitution | — | documentation audit | — | ACTIVE |
| BB-PRN-001..007 | Principles | — | design review | — | ACTIVE |
| BB-KRN-001..007 | Canonical Rules | future Resolution contracts | future deterministic/property tests | — | SPECIFIED |
| BB-BRD-001..003 | Canonical Rules | future BoardState | future board tests | — | PARTIAL |
| BB-PCS-001..006 | Canonical Rules | future Piece/Movement | future movement tests | — | SPECIFIED/PARTIAL |
| BB-TURN | Canonical Rules | future Turn/Resolution | future ordering tests | — | PARTIAL |
| BB-ACT-001 | Canonical Rules | future Action | future action tests | — | SPECIFIED |
| BB-VIC-001..004 | Canonical Rules | future Victory | future termination tests | — | PARTIAL |
| BB-MUT-001..005 | Canonical Rules | future Mutation | future mutation/property tests | — | PARTIAL/current gate |
| BB-RNG-001..002 | Canonical Rules | future RNG | reproducibility tests | — | SPECIFIED |
| BB-META-001 | Canonical Rules | future progression | design/economy verification | — | PRINCIPLE |
| BB-RUN | Canonical Rules | future Run | progression tests | — | PARTIAL |

Before implementation unlock, every P0 implementation requirement must map to authoritative specification, contract, and planned verification. After implementation begins, it must additionally map to code and passing verification.
