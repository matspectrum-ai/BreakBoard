# BreakBoard Agent Protocol

## Authority
The repository is the canonical source of truth. Never infer requirements from conversation history or external assumptions.

Read, in order:
1. `docs/MASTER-SPEC.md`
2. `docs/CURRENT-STATE.md`
3. active task specification
4. relevant approved specs/contracts/ADRs
5. `docs/OPEN-QUESTIONS.md`

## Current execution lock
Architecture is complete and the project is **READY** for implementation, but implementation is still explicitly **LOCKED** and **NOT AUTHORIZED**.

Readiness is not authorization. While `docs/CURRENT-STATE.md` says `implementation: LOCKED` or `implementation_authorized: false`, do not:
- create `package.json`, lockfiles, workspace/scaffold files or generated types;
- create production/source/test-harness code;
- create PixiJS or Tauri application scaffolds;
- materialize RED tests as executable implementation artifacts;
- transition project phase/status to implementation;
- change the selected technology/architecture without a new approved ADR.

Only an explicit later user authorization may open `BB-IMPL-GATE-001`.

## Mandatory pipeline after explicit unlock
1. Problem Analysis
2. Specification impact check
3. Contracts / traceability check
4. Tests (RED with intended failure)
5. Minimal Implementation (GREEN)
6. Refactor
7. Technical Explanation / verification
8. Commit with referenced requirement/test IDs

## Invariants
- Do not invent unspecified behavior.
- Do not silently resolve an open question.
- Do not alter a canonical rule merely to simplify implementation.
- Do not implement future scope prematurely.
- Distinguish canonical decisions, hypotheses, balance parameters and open questions.
- If authoritative documents conflict, stop and report the conflict.
- Canonical behavior changes require impact analysis across specifications, contracts, tests, traceability and architecture.
- A presentation/platform limitation must not be repaired by changing authoritative game semantics.

## Documentation precedence
1. `docs/foundation/CONSTITUTION.md`
2. `docs/foundation/VISION.md`
3. `docs/game/CANONICAL-RULES.md`
4. approved product/system specifications
5. formal contracts
6. accepted ADRs / Architecture specifications
7. implementation plans
8. tests
9. implementation

A lower layer may clarify but must not contradict a higher layer.
