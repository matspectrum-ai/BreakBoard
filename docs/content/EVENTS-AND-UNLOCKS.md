# Events, Unlocks & Reward Validation v0.1

## Events

### BB-EVT-001 — Broken Altar
Regions: 1–3. Generation condition: player has at least one removable Piece Mutation.
Choices:
- `Rewrite Flesh`: choose/remove one Piece Mutation, then generate/persist a standard Piece Mutation offer and resolve normal target/replacement flow.
- `Leave`: no state change.
The removal is an explicit Event exception to the no-free-removal baseline.

### BB-EVT-002 — Mirror Forge
Regions: 1–3. Generation condition: there exists a currently owned Piece Mutation and at least one other eligible target Piece that can receive that mutation (including explicit replacement if needed).
Choices:
- choose source Mutation and target Piece; copy through normal compatibility/capacity validation;
- Leave.
Does not create a profile unlock.

### BB-EVT-003 — Rewrite Chamber
Regions: 2–3. Generation condition: at least one active Rule Mutation.
Choices:
- choose one active Rule Mutation to remove, persist a 3-candidate eligible Rule Mutation offer, choose replacement, then commit removal+addition atomically;
- Leave.
Cancelling before final confirmation leaves state unchanged.

### BB-EVT-004 — Survey Beacon
Regions: 1–3. Always eligible.
Choices:
- `Survey`: reveal exact encounter-template IDs for currently visible next-route nodes in addition to existing route metadata;
- Leave.
No combat-power change.

### BB-EVT-005 — Broken Crown
Regions: 2–3. Generation condition: at least one eligible non-Core owned Piece exists.
Choices:
- choose non-Core Piece; grant BB-PM-015 Crowned for the active Run even if not profile-unlocked; set pending next-Battle override `first_side=enemy`;
- Leave.
This authored grant does not unlock Crowned for future reward pools.

### BB-EVT-006 — Rift Bargain
Regions: 2–3. Generation condition: at least one future eligible Battle encounter remains in current route context.
Choices:
- generator creates and persists two valid bounded BoardConfiguration options for the next eligible Battle; player chooses one;
- chosen configuration is applied to that Battle;
- if Player wins, its normal Piece reward class is upgraded to `enhanced`;
- Leave leaves future encounter unchanged.
Randomness selects valid offered configurations; player chooses outcome.

## Rare Piece Mutation unlocks

| Mutation | Unlock condition |
|---|---|
| BB-PM-013 Afterlife | Win a Battle in which at least 6 of the player's Pieces died during resolution history. |
| BB-PM-014 Split | Cause one deterministic reaction chain containing at least 3 PieceDied events before stabilization. |
| BB-PM-015 Crowned | Defeat BB-BOSS-002 The Chain Sovereign. |
| BB-PM-016 Wallmaker | Defeat BB-BOSS-001 The Architect. |
| BB-PM-017 Parasite | Capture an enemy Piece that had at least 2 active Mutations at capture commit. |
| BB-PM-018 Echo Step | Execute at least 3 Player Primary Actions within one turn and win that Battle. |
| BB-PM-019 Portalist | Complete at least 3 portal transfers by Player Pieces in one Battle and win. |
| BB-PM-020 Ascendant | A Player Pawn commits the Capture that removes the opponent's final active Core and the Battle ends in Player victory. |

## Advanced Rule Mutation unlocks

| Mutation | Unlock condition |
|---|---|
| BB-RM-004 Twin Crown | Win a Battle while at least 2 Player active Cores exist at the final victory checkpoint. |
| BB-RM-006 Afterlife Pact | BB-PM-013 is profile-unlocked and a Player Piece successfully returns through a scheduled resurrection in any Battle. |
| BB-RM-007 Blood Price | A Player-owned ordinary effect causes an allied PieceDied event and the Battle subsequently resolves normally. |
| BB-RM-008 Shattered Law | Cause at least 10 distinct tiles to become destroyed during one Battle. |

Unlock conditions are profile achievements and do not grant permanent raw combat stats.

## Reward satisfiability contracts

### Starter
Fresh profile unlocks 12 Common Piece Mutation IDs (BB-PM-001..012). A fresh standard army has eligible targets for more than three distinct IDs. Starter `3 choose 1` therefore has a hard content-validation requirement of three distinct eligible candidates.

### Normal/Elite Piece rewards
Candidate identity is mutation ID; targets are selected after candidate choice. A candidate remains meaningful if at least one eligible Piece can receive it directly or through explicit replacement and does not already carry that same ID. Standard v0.1 run pacing/acquisition bounds plus 12 starter Common IDs must be validated so every reachable required normal/Elite reward state has at least three distinct eligible Piece Mutation candidates. Meta unlocks only enlarge this pool.

### Boss Rule rewards
Fresh profile starts with four unlocked Rule IDs: BB-RM-001, BB-RM-002, BB-RM-003, BB-RM-005. Region I Boss occurs before any standard Boss Rule reward, so at least four are eligible initially. After selecting one, Region II Boss still has at least three distinct starter Rule candidates not already active. Standard Events cannot invalidate this guarantee because Rewrite Chamber performs atomic replacement and cannot create duplicate active Rule IDs.

A future content change that makes a mandatory reward pool contain fewer than three candidates is a content validation failure, not a reason for runtime to reroll outside the persisted deterministic offer contract.

## Representative composition validation
- Explosive + Split: both react to PieceDied under deterministic ordering; no exception.
- Armored + Void: first ordinary Void removal may be cancelled; later TurnEnd can remove after charge is consumed.
- Crowned + Afterlife: future resurrection does not postpone final-Core defeat.
- Blood Price + Explosive: allied capture commits normally and Explosive reacts to allied death.
- Portal + Fragile: portal transfer causes TileLeft; Fragile may destroy endpoint; remaining endpoint becomes inert.
- Sanctuary + Collapse: Sanctuary affects ordinary Capture only; lifecycle forced removal still commits.
- Twin Crown + Crowned: any number of active Core-classified Pieces is handled by active-core count.
- Breaker + Riftwalker: Breaker may create a gap later traversable by Riftwalker.
- Chain + Momentum + Echo Step: action grants remain finite because each definition is bounded by first-event/turn semantics and Resolution System budget.
