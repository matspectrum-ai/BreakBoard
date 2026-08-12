# Open Questions

Agents must not resolve these implicitly.

| ID | Question | Status |
|---|---|---|
| BB-OQ-001 | Is 6×6 optimal? | OPEN / 6×6 hypothesis |
| BB-OQ-002 | Exact player-relative Pawn forward orientation? | OPEN |
| BB-OQ-003 | Ordering of OnCapture, OnCaptured, OnDeath, board and rule effects? | OPEN / Mutation gate |
| BB-OQ-004 | Bounded effect-resolution mechanism? | OPEN / Mutation gate |
| BB-OQ-005 | Exact Collapse algorithm/threshold? | OPEN |
| BB-OQ-006 | Collapse interaction with occupied Core/invalid state? | OPEN |
| BB-OQ-007 | Board Mutation capacity? | OPEN |
| BB-OQ-008 | Voluntary mutation removal or replacement only? | OPEN / Mutation gate |
| BB-OQ-009 | Resurrection when original tile is missing/occupied? | OPEN / Mutation gate |
| BB-OQ-010 | Portal endpoint destruction semantics? | OPEN / Mutation gate |
| BB-OQ-011 | Disconnected board regions? | OPEN |
| BB-OQ-012 | Mutation offer eligibility/weighting? | OPEN / later balance gate |
| BB-OQ-013 | Exact run length/encounter graph? | OPEN / progression gate |
| BB-OQ-014 | Battle-duration target/anti-stall threshold? | OPEN / playtest |
| BB-OQ-015 | Which quantities are balance parameters vs immutable? | OPEN |
| BB-OQ-016 | Formal invalid GameState definition? | OPEN / contract gate |
| BB-OQ-017 | Is perfect information permanent? | OPEN |

## Resolved baseline
Core movable; final active Core determines victory candidate; mutation capacity exists; Piece/Rule persist by default while Board does not; compatibility is explicit; runs are seeded; meta progression horizontal; BreakBoard defines its own piece semantics.
