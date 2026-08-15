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
| BB-OQ-007 | Board Mutation capacity? | RESOLVED for v0.1: Board Features/Mutations are encounter configuration, not persistent player inventory |
| BB-OQ-008 | Voluntary mutation removal outside replacement/acquisition flow? | RESOLVED for v0.1: only explicit Event/effect/replacement; no free removal |
| BB-OQ-009 | Resurrection when original tile is missing/occupied? | RESOLVED structurally: explicit selector + invalid-target policy |
| BB-OQ-010 | Portal endpoint destruction semantics? | RESOLVED: incomplete link becomes inert; no recursive transfer within portal transfer |
| BB-OQ-011 | Disconnected board regions? | RESOLVED: valid topology; no implicit reconnection; Pass/Collapse handle stalls |
| BB-OQ-012 | Mutation offer eligibility/weighting? | PARTIAL: eligibility and reward satisfiability specified; final weights remain balance data |
| BB-OQ-013 | Exact run length/encounter graph? | PARTIAL: 3-region layered-DAG baseline; exact counts remain balance parameters |
| BB-OQ-014 | Battle-duration target/anti-stall threshold? | OPEN / playtest; semantics no longer blocked |
| BB-OQ-015 | Which quantities are balance parameters vs immutable? | OPEN / ongoing classification |
| BB-OQ-016 | Formal invalid GameState definition? | RESOLVED baseline by BB-BTL-013; concrete schema validation deferred |
| BB-OQ-017 | Is perfect information permanent? | OPEN / UX/product gate |
| BB-OQ-018 | Exact occupied-tile destruction semantics? | RESOLVED structurally: explicit reject/remove/relocate; Collapse uses privileged forced removal |
| BB-OQ-019 | How may removal/death-prevention mechanics intercept operations? | RESOLVED: pre-commit ALLOW/CANCEL/REPLACE; forced lifecycle removal is non-interceptable |
| BB-OQ-020 | How are delayed effects/historical entities represented? | RESOLVED: ScheduledEffect + explicit snapshot profile |
| BB-OQ-021 | Which Rule Query modifier hooks exist? | RESOLVED baseline in Mutation System v0.1; expansion requires specification |
| BB-OQ-022 | May encounter configuration override standard first side? | RESOLVED: yes; standard is Player-first and consumes no RNG |
| BB-OQ-023 | What is standard initial formation? | RESOLVED by BB-BTL-003; encounter-specific overrides allowed |
| BB-OQ-024 | Does pending future Core resurrection delay defeat? | RESOLVED: no; only active Cores count at stable victory checkpoint |
| BB-OQ-025 | Standard number of Regions? | RESOLVED baseline: 3; balance/product parameter, not Kernel Rule |
| BB-OQ-026 | Region graph size/shape? | RESOLVED baseline: layered DAG, approx. 4–5 intermediate layers and 2–3 nodes/layer |
| BB-OQ-027 | Which encounters reward mutations? | RESOLVED: normal Battle→Piece, Elite→enhanced Piece, Region I/II Boss→Rule, Final Boss→completion |
| BB-OQ-028 | Does a Run start with a Mutation? | RESOLVED: standard starter Piece Mutation offer before Region I |
| BB-OQ-029 | What is the minimum initial systemic content catalog? | RESOLVED: 20 Piece, 8 Rule, 8 Board Features, 10 templates, 6 Events, 6 Elites, 3 Bosses |
| BB-OQ-030 | Can authored enemy/Event content use player-locked mutations? | RESOLVED: yes; unlocks gate normal player reward eligibility only unless explicitly stated |
| BB-OQ-031 | What happens if a mandatory reward has fewer than 3 candidates? | RESOLVED as validation invariant: standard v0.1 reachable states must guarantee 3; violating content changes fail validation rather than silent runtime fallback |
| BB-OQ-032 | How do Board Features coexist? | RESOLVED baseline: composition allowed except explicit exclusions; Wall excludes all other functional features |

## Resolved baseline
The repository now canonically defines game rules, mutation composition, battle lifecycle, run/progression, and an initial systemic content set. Remaining major planning work concerns UX/readability, art/presentation, technical architecture/contracts, and verification before implementation unlock.
