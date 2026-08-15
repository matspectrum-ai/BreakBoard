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
| BB-BTL-001..016 | Battle System v0.1 | future BattleState/Action/Victory | 24-scenario battle matrix + property tests | — | DESIGN SPECIFIED |
| BB-RUN-001..028 | Run System v0.1 | future Run/Profile/Reward contracts | route/reward/seed/lifecycle tests | — | DESIGN SPECIFIED |
| BB-CNT-001..015 | Content System v0.1 | future content schema/contracts | schema/reference/reward-satisfiability/composition tests | — | DESIGN SPECIFIED |
| BB-PM-001..020 | Piece Mutation catalog | future declarative mutation definitions | eligibility + representative composition tests | — | CONTENT SPECIFIED |
| BB-RM-001..008 | Rule Mutation catalog | future declarative mutation definitions | Rule Query/composition/reward-pool tests | — | CONTENT SPECIFIED |
| BB-BF-001..008 | Board Feature catalog | future board-feature definitions | feature coexistence/portal/occupancy tests | — | CONTENT SPECIFIED |
| BB-ENC-001..010 | Encounter templates | future encounter config schema | region/complexity/generation tests | — | CONTENT SPECIFIED |
| BB-EVT-001..006 | Event catalog | future event schema | precondition/choice/atomic-outcome tests | — | CONTENT SPECIFIED |
| BB-ELT-001..006 | Elite presets | future encounter config schema | configuration validity tests | — | CONTENT SPECIFIED |
| BB-BOSS-001..003 | Boss catalog | future encounter/lifecycle config | boss-mechanic/termination/reward tests | — | CONTENT SPECIFIED |
| BB-RNG-001..003 | Canonical Rules + Run System | future RNG | reproducibility/namespace tests | — | DESIGN SPECIFIED |
| BB-META-001..003 | Canonical Rules + Run/Content | future Profile/Unlock | horizontal-progression/unlock tests | — | DESIGN + CONTENT SPECIFIED |

Before implementation unlock, every P0 implementation requirement must map to authoritative specification, contract, and planned verification. After implementation begins, it must additionally map to code and passing verification.
