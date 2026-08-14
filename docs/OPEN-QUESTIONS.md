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
| BB-OQ-007 | Board Mutation capacity? | RESOLVED for run baseline: Board Mutations are encounter-scoped, not persistent player inventory |
| BB-OQ-008 | Voluntary mutation removal outside replacement/acquisition flow? | RESOLVED baseline: no free removal; explicit replacement/Event/effect only |
| BB-OQ-009 | Resurrection when original tile is missing/occupied? | RESOLVED structurally: explicit selector + invalid-target policy |
| BB-OQ-010 | Portal endpoint destruction semantics? | OPEN / content contract; no implicit behavior |
| BB-OQ-011 | Disconnected board regions? | RESOLVED: valid topology; no implicit reconnection; Pass/Collapse handle stalls |
| BB-OQ-012 | Mutation offer eligibility/weighting? | PARTIAL: eligibility pipeline and deterministic weighting specified; exact weights/content pools remain Content/Balance work |
| BB-OQ-013 | Exact run length/encounter graph? | PARTIAL: 3-region layered-DAG baseline; exact counts remain balance parameters |
| BB-OQ-014 | Battle-duration target/anti-stall threshold? | OPEN / playtest; semantics no longer blocked |
| BB-OQ-015 | Which quantities are balance parameters vs immutable? | OPEN / ongoing classification |
| BB-OQ-016 | Formal invalid GameState definition? | RESOLVED baseline by BB-BTL-013; concrete schema validation deferred |
| BB-OQ-017 | Is perfect information permanent? | OPEN / product design |
| BB-OQ-018 | Exact occupied-tile destruction semantics? | RESOLVED structurally: explicit reject/remove/relocate; Collapse uses privileged forced removal |
| BB-OQ-019 | How may removal/death-prevention mechanics intercept operations? | RESOLVED: pre-commit ALLOW/CANCEL/REPLACE; forced lifecycle removal is non-interceptable |
| BB-OQ-020 | How are delayed effects/historical entities represented? | RESOLVED: ScheduledEffect + explicit snapshot profile |
| BB-OQ-021 | Which Rule Query modifier hooks exist? | RESOLVED baseline in Mutation System v0.1; expansion requires specification |
| BB-OQ-022 | May encounter configuration override standard first side? | RESOLVED: yes; standard is Player-first and consumes no RNG |
| BB-OQ-023 | Standard initial formation? | RESOLVED by BB-BTL-003; encounter-specific overrides allowed |
| BB-OQ-024 | Does pending future Core resurrection delay defeat? | RESOLVED: no; only active Cores count at stable victory checkpoint |
| BB-OQ-025 | How many regions exist in a standard run? | RESOLVED baseline: 3; balance parameter |
| BB-OQ-026 | What route-graph structure is used? | RESOLVED: deterministic layered DAG; 4–5 intermediate layers and 2–3 nodes/layer are balance baselines |
| BB-OQ-027 | Which encounters grant persistent mutation rewards? | RESOLVED baseline: normal Battle → Piece; Elite → enhanced Piece; non-final Boss → Rule; Final Boss → completion |
| BB-OQ-028 | Does a run start with a mutation? | RESOLVED baseline: starter Piece Mutation offer before Region I |
| BB-OQ-029 | Do pieces have inter-battle permadeath? | RESOLVED baseline: no |
| BB-OQ-030 | What constitutes full deterministic run identity? | RESOLVED: run seed + content version + unlock/eligible-content snapshot |
| BB-OQ-031 | Can paid systems increase combat power or in-run choice quality? | RESOLVED baseline: no paid power/rerolls/slots/seed manipulation |
| BB-OQ-032 | What exact mutation weights, Rare rates, complexity costs, and starter pool content launch with v1? | OPEN / Content gate |
| BB-OQ-033 | What is the mid-battle interruption/recovery policy? | OPEN / persistence/product gate |

## Resolved baseline
BreakBoard now has canonical Mutation, Battle, and Run/Progression design systems. Persistent builds use Piece and Rule Mutations; Board Mutations remain encounter-scoped. Runs use deterministic layered route graphs, independent RNG namespaces, persisted reward offers, optional Elites, Boss-gated regions, horizontal meta-progression, and content snapshots for reproducibility.
