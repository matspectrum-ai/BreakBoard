# Traceability Matrix

Planning state. Formal contracts and RED-first verification are the current gate; production implementation remains absent.

| Requirement | Source | Formal Contract | Planned Verification | Implementation | Status |
|---|---|---|---|---|---|
| BB-CON-001..010 | Constitution | DOMAIN-MODEL + VERIFICATION-PLAN | governance/traceability audit | — | ACTIVE |
| BB-PRN-001..007 | Principles | DOMAIN-MODEL + CONTENT-CONTRACTS | design/content branch audit | — | ACTIVE |
| BB-KRN-001..007 | Canonical Rules | STATE-CONTRACTS + RESOLUTION-CONTRACT | deterministic/property/termination + RSL-BOUND | — | CONTRACT DRAFT |
| BB-BRD/PCS | Canonical + Battle | DOMAIN-MODEL + STATE-CONTRACTS | CTR-SCHEMA/ORD + board/piece properties | — | CONTRACT DRAFT |
| BB-TURN/ACT/VIC | Battle System | STATE-CONTRACTS + RESOLUTION-CONTRACT | BTL-ACT/STABLE/VIC | — | CONTRACT DRAFT |
| BB-MUT / BB-MSYS | Mutation System | RESOLUTION-CONTRACT + CONTENT-CONTRACTS | RSL-ATOM + canonical mutation scenarios | — | CONTRACT DRAFT / RULE QUERY ALGEBRA OPEN |
| BB-BTL-001..016 | Battle System v0.1 | STATE-CONTRACTS + RESOLUTION-CONTRACT | BTL + RSL scenario/property set | — | CONTRACT DRAFT |
| BB-RUN-001..028 | Run System v0.1 | STATE-CONTRACTS + RNG-PERSISTENCE-CONTRACT | RUN-RNG/GEN/REW + persistence | — | CONTRACT DRAFT |
| BB-CONT-* | Content System + catalogs | CONTENT-CONTRACTS | CNT-REF/COMP/SAT/CPLX/BRANCH | — | CONTRACT DRAFT |
| BB-RNG-001..003 | Canonical + Run | RNG-PERSISTENCE-CONTRACT | RUN-RNG + future golden RNG vectors | — | CONTRACT DRAFT / ALGORITHM DEFERRED |
| BB-META-001..003 | Canonical + Run | STATE-CONTRACTS(Profile/Run) | progression-integrity tests | — | CONTRACT DRAFT |
| BB-UX-001..027 | UX System | domain events/rejection codes + future presentation contracts | interaction/accessibility tests | — | DESIGN SPECIFIED |
| BB-ART-001..011 | Art System | presentation-only boundary in DOMAIN-MODEL | visual/readability audits | — | DESIGN SPECIFIED |
| BB-AUD / BB-MUS / BB-HAP / BB-AAS | Audio specs | presentation-only boundary in DOMAIN-MODEL | mute-all/semantic/accessibility audits | — | DESIGN SPECIFIED |
| BB-PER | Run + Contract gate | RNG-PERSISTENCE-CONTRACT | PER-RT/MIG/VER/XREF/ATOMIC | — | CONTRACT DRAFT / MID-BATTLE POLICY OPEN |
| BB-CONTRACT-GATE-001 | Current gate | all docs/contracts/* | VERIFICATION-PLAN + P0 audit | — | IN_PROGRESS |

## Gate closure rule
Before BB-CONTRACT-GATE-001 closes, every P0 behavioral requirement must have an authoritative source, a formal contract location, and at least one named RED-first verification case. Remaining balance-only questions may stay open only when they cannot change contract semantics. Production implementation stays locked.
