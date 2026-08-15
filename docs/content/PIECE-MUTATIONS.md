# Initial Piece Mutation Catalog v0.1

All definitions use run lifetime by default, same-Piece stacking `prohibited`, and may repeat across different eligible Pieces unless stated otherwise.

| ID | Name | Rarity | Eligible target | Complexity | Core mechanic |
|---|---|---|---|---:|---|
| BB-PM-001 | Armored | COMMON | any Piece | 1 | First ordinary RemovePiece against self each Battle is CANCELLED; charge consumed. Forced removal unaffected. |
| BB-PM-002 | Explosive | COMMON | any Piece | 2 | PieceDied(self) → ordinary RemovePiece on all adjacent Pieces, ally or enemy. |
| BB-PM-003 | Ghost | COMMON | Tower, Seer | 1 | MovementBlocking ignores Pieces; DestinationValidity/occupancy unchanged. |
| BB-PM-004 | Blink | COMMON | any Piece | 2 | Once/Battle Ability: teleport to valid empty existing tile within Chebyshev distance 2; no capture. |
| BB-PM-005 | Chain | COMMON | any Piece | 2 | First committed Capture by self each turn grants self one additional Primary Action. |
| BB-PM-006 | Anchor | COMMON | any Piece | 1 | Non-voluntary MovePiece against self is CANCELLED; player-selected moves still work; forced removal unaffected. |
| BB-PM-007 | Breaker | COMMON | any Piece | 2 | After self commits a Capture, destroy the vacated origin tile. |
| BB-PM-008 | Swapper | COMMON | any Piece | 2 | Once/Battle Ability: atomically swap with eligible allied Piece within distance 2 if both resulting positions are valid. |
| BB-PM-009 | Sidewinder | COMMON | Pawn | 1 | Add one-tile lateral non-capture movement vectors; capture pattern unchanged. |
| BB-PM-010 | Riftwalker | COMMON | Tower, Seer | 2 | Path may cross destroyed/missing coordinates; final destination must exist and satisfy normal validity. |
| BB-PM-011 | Fragile Trail | COMMON | any Piece | 2 | After voluntary Move/Capture, add Fragile to origin tile if it still exists. |
| BB-PM-012 | Sentinel | COMMON | non-Core Piece at install time | 2 | While active, adjacent allied Core Pieces cannot be ordinary Capture targets. Sentinel itself receives no self-protection. |
| BB-PM-013 | Afterlife | RARE | any Piece | 3 | On death schedule resurrection at owner TurnStart +2 using explicit death snapshot; OriginalTile target; invalid/occupied → skip; does not delay defeat. |
| BB-PM-014 | Split | RARE | non-Core Piece | 3 | On death spawn up to 2 mutationless allied Pawns on deterministic adjacent empty valid tiles. |
| BB-PM-015 | Crowned | RARE | non-Core Piece | 2 | CoreClassification=true while retaining original archetype/movement. |
| BB-PM-016 | Wallmaker | RARE | any Piece | 3 | Once/Battle after voluntary move, convert origin tile to Wall using explicit occupied-target validation (normally origin is empty). |
| BB-PM-017 | Parasite | RARE | non-Core Piece | 4 | After capturing mutated target, copy one compatible target Mutation for Battle lifetime; choose highest priority then stable ID. |
| BB-PM-018 | Echo Step | RARE | any Piece | 3 | First voluntary Move each turn grants self one additional movement-only action, fully revalidated. |
| BB-PM-019 | Portalist | RARE | any Piece | 4 | Once/Battle Ability: create Portal pair between current tile and selected valid empty tile. |
| BB-PM-020 | Ascendant | RARE | Pawn | 2 | Override Pawn base MovementPattern/CapturePattern to one tile in any direction; archetype remains Pawn. |

## Compatibility notes
- Ghost and Riftwalker are compatible: one changes Piece blocking, the other missing-topology path traversal.
- Chain and Echo Step may coexist; their grants are bounded by first-event-per-turn conditions and normal resolution budget.
- Afterlife and Split may coexist and independently react to the same death under deterministic reaction ordering.
- Crowned may coexist with other mutations. If a Crowned Piece is the last active Core and dies, pending Afterlife does not delay victory evaluation.
- Sidewinder + Ascendant is legal. Ascendant applies the movement override; Sidewinder's additive lateral vectors deduplicate against the already-available lateral moves.
- Sentinel install requires non-Core status at installation. If the Piece later becomes a Core through Crowned, Sentinel still protects other adjacent allied Cores but never itself.

## Starter Piece pool
BB-PM-001..012 are unlocked in a fresh profile. BB-PM-013..020 are meta-unlocked through explicit conditions in `EVENTS-AND-UNLOCKS.md`.
