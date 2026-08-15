# RED-First Verification Plan v0.1

Status: **DRAFT — BB-CONTRACT-GATE-001 IN PROGRESS**

## Objective
Define tests before implementation. Test identifiers and expected behavior are contract artifacts; implementation technology is intentionally absent.

## BB-VRF-001 — Test layers
Required verification layers:
1. schema/contract validation;
2. pure unit tests for value objects/rule queries;
3. property tests for invariants/determinism;
4. transaction tests for OperationGroups/interception;
5. scenario tests for canonical Battle/Mutation interactions;
6. generation tests for Run/RNG/content;
7. persistence/migration tests;
8. presentation-contract audits where gameplay readability depends on semantic output rather than engine rendering.

## BB-VRF-002 — RED rule
Before a production behavior is implemented, at least one authoritative test for that behavior must exist and fail for the expected reason. A test that errors because the test harness itself is incomplete does not count as RED acceptance.

## BB-VRF-003 — Schema RED set
Initial tests:
- `CTR-SCHEMA-001` valid minimal Board/Side/Piece states accepted;
- `CTR-SCHEMA-002` ACTIVE Piece without Coordinate rejected;
- `CTR-SCHEMA-003` REMOVED Piece with Coordinate rejected semantically;
- `CTR-SCHEMA-004` unresolved ContentId rejected by cross-reference validation;
- `CTR-SCHEMA-005` content attempting FORCED RemovePiece rejected;
- `CTR-SCHEMA-006` unknown Rule Query hook rejected;
- `CTR-SCHEMA-007` ScheduledEffect with executable callback/script field rejected.

## BB-VRF-004 — Deterministic ordering properties
- `CTR-ORD-001`: same unordered storage insertion order produces same canonical coordinate/entity traversal;
- `CTR-ORD-002`: Reaction ordering follows layer/priority/activation/content ID tie-breaks;
- `CTR-ORD-003`: Capture emitted event order is invariant;
- `CTR-ORD-004`: selector results using deterministic fallback order are invariant across repeated runs.

## BB-VRF-005 — Action legality/no-op
- `BTL-ACT-001`: illegal Move changes no GameState field;
- `BTL-ACT-002`: illegal action consumes zero gameplay RNG draws;
- `BTL-ACT-003`: illegal action consumes zero turn/action allowance;
- `BTL-ACT-004`: Pass legal iff zero non-Pass Primary Actions exist;
- `BTL-ACT-005`: deterministic rejection reason code for same state/intent.

## BB-VRF-006 — Atomicity/interception
- `RSL-ATOM-001`: successful OperationGroup commits all operations;
- `RSL-ATOM-002`: validation failure commits none;
- `RSL-ATOM-003`: capture target RemovePiece CANCEL prevents attacker relocation;
- `RSL-ATOM-004`: valid REPLACE result revalidates before commit;
- `RSL-ATOM-005`: invalid replacement produces deterministic fault/no partial state;
- `RSL-ATOM-006`: ordinary Armored interception cannot block Collapse FORCED removal;
- `RSL-ATOM-007`: content cannot construct FORCED removal.

## BB-VRF-007 — Stable-state/victory
- `BTL-STABLE-001`: no input while Reaction queue nonempty;
- `BTL-STABLE-002`: no victory evaluation mid-OperationGroup;
- `BTL-VIC-001`: Player cores 0/Enemy >0 => defeat;
- `BTL-VIC-002`: Enemy cores 0/Player >0 => victory;
- `BTL-VIC-003`: both zero => Double Break/Player defeat;
- `BTL-VIC-004`: future Afterlife resurrection does not delay final-Core defeat.

## BB-VRF-008 — Canonical mutation scenarios
Golden scenario fixtures must include at least:
- Explosive death chain;
- Armored cancellation/charge consumption;
- Ghost movement blocking only, not destination occupancy;
- Portal transfer recursion prevention;
- Fragile TileLeft destruction;
- Afterlife snapshot + invalid-target SKIP;
- Crowned/Twin Crown active-Core classification;
- Blood Price + Explosive allied capture;
- Sanctuary vs ordinary Capture and Collapse forced removal;
- Split + Explosive deterministic reaction ordering.

## BB-VRF-009 — Resolution bounds
Before gate closure define and test:
- `RSL-BOUND-001` finite chain under budget reaches stable state;
- `RSL-BOUND-002` known cycle triggers cycle fault;
- `RSL-BOUND-003` budget overflow restores exact previous stable snapshot;
- `RSL-BOUND-004` rollback restores RNG cursors/charges/action allowance as if failed resolution never committed;
- `RSL-BOUND-005` same cyclic input yields same fault/signature.

These tests are currently specified but blocked on the cycle/budget micro-contract.

## BB-VRF-010 — Run/RNG
- `RUN-RNG-001`: same complete GenerationIdentity => same region graph;
- `RUN-RNG-002`: reward namespace draw changes do not alter route namespace output;
- `RUN-RNG-003`: Event namespace draw changes do not alter unrelated battle seed;
- `RUN-GEN-001`: every generated reachable node can reach Boss;
- `RUN-GEN-002`: no cycles/orphan reachable nodes;
- `RUN-GEN-003`: Elite is never the sole forward route;
- `RUN-GEN-004`: bounded impossible generation returns explicit fault;
- `RUN-REW-001`: opening/reloading unresolved RewardOffer preserves exact ordered candidates.

## BB-VRF-011 — Content validation
- `CNT-REF-001`: every catalog reference resolves to correct kind;
- `CNT-COMP-001`: requires/excludes contradictions fail validation;
- `CNT-SAT-001`: fresh starter Piece reward has >=3 valid candidates;
- `CNT-SAT-002`: every reachable standard normal/Elite acquisition state preserves configured minimum candidates;
- `CNT-SAT-003`: Region I/II Boss Rule reward preserves configured minimum candidates;
- `CNT-CPLX-001`: encounter above complexity budget rejected;
- `CNT-BRANCH-001`: no initial content definition requires content-ID engine branch.

## BB-VRF-012 — Persistence
- `PER-RT-001`: stable Run save/load round trip is semantically equal;
- `PER-RT-002`: unresolved RewardOffer round trip preserves order/status;
- `PER-MIG-001`: migration fixture output deterministic;
- `PER-MIG-002`: migration never rerolls reward/route content;
- `PER-VER-001`: unsupported future schema version rejected explicitly;
- `PER-XREF-001`: invalid/missing content version cannot silently load active run;
- `PER-ATOMIC-001`: simulated partial write never becomes authoritative.

## BB-VRF-013 — Property/fuzz obligations
Property-based generation should target valid and near-valid Board/GameState instances to establish:
- stable committed state always satisfies invariants;
- successful Move/Capture never duplicates a Piece;
- existing Tile coordinates remain unique;
- deterministic selectors never return out-of-domain references;
- all bounded resolution attempts terminate with stable state or deterministic fault;
- Collapse monotonically reduces remaining topology when it commits.

## BB-VRF-014 — Metamorphic determinism
Metamorphic tests must verify that semantically irrelevant representation changes do not alter results, including map insertion order, presentation settings, animation/audio enabled state, and serialization object-key order.

## BB-VRF-015 — Traceability requirement
Every P0 contract requirement must map to at least one named verification ID before the contract gate closes. Every production implementation change after unlock must identify the contract/test IDs it satisfies or modifies.

## BB-VRF-016 — Implementation unlock criteria
This gate alone does not unlock implementation. To complete BB-CONTRACT-GATE-001:
- all P0 domain/state/content contracts are specified;
- Rule Query composition algebra is complete;
- resolution budget/cycle signature is complete;
- all P0 requirements have named RED-first verification cases;
- no unresolved P0 semantic question remains;
- schema/reference validation strategy is explicit;
- persistence interruption question is classified P0 or explicitly deferred P1 with product impact recorded.

After this gate closes, the next allowed phase is Architecture & Technology Selection. Production implementation remains locked until that subsequent gate establishes implementation boundaries and test-harness strategy.
