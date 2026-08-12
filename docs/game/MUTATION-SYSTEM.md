# Mutation System v0.1

Status: **DESIGN GATE — NOT YET SPECIFIED**

## Objective
Determine whether Piece, Board, and Rule Mutations can use a compact deterministic composable vocabulary rather than bespoke code per mutation.

## Required outputs
1. trigger taxonomy;
2. target taxonomy;
3. effect taxonomy;
4. duration/lifetime semantics;
5. stacking semantics;
6. compatibility (`requires`, `excludes`, `overrides`);
7. priority/deterministic ordering;
8. effect queue/chain resolution;
9. recursion protection;
10. invalid-target behavior;
11. composition rules;
12. capacity semantics;
13. replacement/removal semantics;
14. seeded-randomness boundaries;
15. declarative-definition validation.

## Candidate triggers — NON-CANONICAL
OnBattleStart, OnTurnStart, OnMove, OnCapture, OnCaptured, OnDeath, OnTurnEnd, OnBattleEnd.

## Candidate targets — NON-CANONICAL
Self, Piece, Tile, Adjacent, Row, Column, Board.

## Candidate effects — NON-CANONICAL
Move, Destroy, Spawn, Transform, Teleport, Resurrect, ModifyMovement, ModifyTile, AddMutation, RemoveMutation.

## Validation set
The model must conceptually express: Explosive, Armored, Ghost, Portal, Fragile, capture-destroys-tile, Afterlife, multiple-Core, and Collapse interactions.

Frequent one-off engine branches mean this model fails the gate and must be redesigned before implementation.
