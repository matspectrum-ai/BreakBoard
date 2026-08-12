# Open Questions

Agents must not resolve these implicitly.

| ID | Question | Status |
|---|---|---|
| BB-OQ-001 | Is 6×6 optimal? | OPEN / balance playtest; 6×6 standard hypothesis |
| BB-OQ-002 | Exact player-relative Pawn forward orientation? | RESOLVED: Player +row, Enemy -row |
| BB-OQ-003 | Ordering of capture/death/board/rule reactions? | RESOLVED structurally by Mutation/Battle v0.1; exact priority ranges deferred |
| BB-OQ-004 | Bounded effect-resolution mechanism? | RESOLVED structurally: finite budget + cycle detection + atomic rollback; exact parameters deferred |
| BB-OQ-005 | Exact Collapse algorithm/threshold? | PARTIAL: outer-ring inward algorithm specified; numeric threshold/cadence remain balance parameters |
| BB-OQ-006 | Collapse interaction with occupied Core/invalid state? | RESOLVED: atomic lifecycle forced removal + tile destruction; stable victory afterward |
| BB-OQ-007 | Board Mutation capacity? | OPEN / balance/content gate |
| BB-OQ-008 | Voluntary mutation removal outside replacement/acquisition flow? | OPEN / Run gate |
| BB-OQ-009 | Resurrection when original tile is missing/occupied? | RESOLVED structurally: explicit selector + invalid-target policy |
| BB-OQ-010 | Portal endpoint destruction semantics? | OPEN / content contract; no implicit behavior |
| BB-OQ-011 | Disconnected board regions? | RESOLVED: valid topology; no implicit reconnection; Pass/Collapse handle stalls |
| BB-OQ-012 | Mutation offer eligibility/weighting? | OPEN / Run + balance/content gates |
| BB-OQ-013 | Exact run length/encounter graph? | OPEN / Run gate |
| BB-OQ-014 | Battle-duration target/anti-stall threshold? | OPEN / playtest; semantics no longer blocked |
| BB-OQ-015 | Which quantities are balance parameters vs immutable? | OPEN / ongoing classification |
| BB-OQ-016 | Formal invalid GameState definition? | RESOLVED baseline by BB-BTL-013; concrete schema validation deferred |
| BB-OQ-017 | Is perfect information permanent? | OPEN / product design |
| BB-OQ-018 | Exact occupied-tile destruction semantics? | RESOLVED structurally: explicit reject/remove/relocate policy; Collapse uses privileged forced removal |
| BB-OQ-019 | How may removal/death-prevention mechanics intercept operations? | RESOLVED: pre-commit ALLOW/CANCEL/REPLACE; forced lifecycle removal is non-interceptable |
| BB-OQ-020 | How are delayed effects/historical entities represented? | RESOLVED: ScheduledEffect + explicit snapshot profile |
| BB-OQ-021 | Which Rule Query modifier hooks exist? | RESOLVED baseline in Mutation System v0.1; expansion requires specification |
| BB-OQ-022 | May encounter configuration override the standard first side? | RESOLVED: yes; standard is Player-first and consumes no RNG |
| BB-OQ-023 | What is the standard initial formation? | RESOLVED by BB-BTL-003; encounter-specific overrides allowed |
| BB-OQ-024 | Does pending future Core resurrection delay defeat? | RESOLVED: no; only active Cores count at stable victory checkpoint |

## Resolved baseline
Core movable; final active Core determines victory candidate; mutation capacity exists; Piece/Rule persist by default while Board does not; compatibility is explicit; runs are seeded; meta progression horizontal; BreakBoard defines its own piece semantics; mutations use Reaction/Modifier composition; operations are interceptable before commit; committed events are immutable; compound state changes are atomic; battle input occurs only at stable checkpoints; disconnected topology is valid; Collapse guarantees finite battle termination.
