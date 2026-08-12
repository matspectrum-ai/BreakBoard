# Open Questions

Agents must not resolve these implicitly.

| ID | Question | Status |
|---|---|---|
| BB-OQ-001 | Is 6×6 optimal? | OPEN / 6×6 hypothesis |
| BB-OQ-002 | Exact player-relative Pawn forward orientation? | OPEN / Battle gate |
| BB-OQ-003 | Ordering of capture/death/board/rule reactions? | RESOLVED structurally by Mutation v0.1; exact priority ranges deferred to contract |
| BB-OQ-004 | Bounded effect-resolution mechanism? | RESOLVED structurally: finite budget + cycle detection + atomic rollback; exact parameters deferred |
| BB-OQ-005 | Exact Collapse algorithm/threshold? | OPEN / Battle gate |
| BB-OQ-006 | Collapse interaction with occupied Core/invalid state? | OPEN / Battle gate |
| BB-OQ-007 | Board Mutation capacity? | OPEN / balance/content gate |
| BB-OQ-008 | Voluntary mutation removal outside replacement/acquisition flow? | OPEN / progression/product gate |
| BB-OQ-009 | Resurrection when original tile is missing/occupied? | RESOLVED structurally: explicit target selector + invalid-target policy; content defines policy |
| BB-OQ-010 | Portal endpoint destruction semantics? | OPEN / content contract; no implicit behavior allowed |
| BB-OQ-011 | Disconnected board regions? | OPEN / Battle gate |
| BB-OQ-012 | Mutation offer eligibility/weighting? | OPEN / balance/content gate |
| BB-OQ-013 | Exact run length/encounter graph? | OPEN / progression gate |
| BB-OQ-014 | Battle-duration target/anti-stall threshold? | OPEN / playtest + Battle gate |
| BB-OQ-015 | Which quantities are balance parameters vs immutable? | OPEN / ongoing classification |
| BB-OQ-016 | Formal invalid GameState definition? | OPEN / Battle + contract gates |
| BB-OQ-017 | Is perfect information permanent? | OPEN / product design |
| BB-OQ-018 | Exact occupied-tile destruction semantics? | RESOLVED structurally: explicit reject/remove/relocate policy in atomic group; each effect/content defines policy |
| BB-OQ-019 | How may removal/death-prevention mechanics intercept operations? | RESOLVED: pre-commit ALLOW/CANCEL/REPLACE interception; events immutable after commit |
| BB-OQ-020 | How are delayed effects and historical entities represented? | RESOLVED structurally: ScheduledEffect + explicit snapshot profile |
| BB-OQ-021 | Which Rule Query modifier hooks exist? | RESOLVED baseline in Mutation System v0.1; expansion requires specification |

## Resolved baseline
Core movable; final active Core determines victory candidate; mutation capacity exists; Piece/Rule persist by default while Board does not; compatibility is explicit; runs are seeded; meta progression horizontal; BreakBoard defines its own piece semantics; mutations use Reaction/Modifier composition; operations are interceptable before commit; committed events are immutable; compound state changes are atomic.
