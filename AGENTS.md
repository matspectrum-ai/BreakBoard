# BreakBoard Agent Protocol

## Authority
The repository is the canonical source of truth. Never infer requirements from conversation history or external assumptions.

Read, in order:
1. `docs/MASTER-SPEC.md`
2. `docs/CURRENT-STATE.md`
3. active task specification
4. relevant contracts and decisions when they exist
5. `docs/OPEN-QUESTIONS.md`

## Current execution lock
Implementation is **LOCKED** while `docs/CURRENT-STATE.md` reports planning/LOCKED. Do not create production source code, select technologies, or silently resolve design questions.

## Mandatory pipeline after unlock
1. Problem Analysis
2. Specification
3. Contracts
4. Tests (Red)
5. Minimal Implementation (Green)
6. Refactor
7. Technical Explanation / verification

## Invariants
- Do not invent unspecified behavior.
- Do not silently resolve an open question.
- Do not alter a canonical rule merely to simplify implementation.
- Do not implement future scope prematurely.
- Distinguish canonical decisions, hypotheses, balance parameters, and open questions.
- If authoritative documents conflict, stop and report the conflict.
- Canonical behavior changes require impact analysis across specifications, contracts, tests, traceability, and implementation plans.

## Documentation precedence
1. `docs/foundation/CONSTITUTION.md`
2. `docs/foundation/VISION.md`
3. `docs/game/CANONICAL-RULES.md`
4. approved product/system specifications (future)
5. contracts (future)
6. ADRs / decisions
7. implementation plans
8. tests
9. implementation

A lower layer may clarify but must not contradict a higher layer.
