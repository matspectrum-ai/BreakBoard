# BreakBoard Interaction Model v0.1

Status: canonical UX companion to `UX-SYSTEM.md`.

## Primary flow
`MAIN_MENU → RUN_SETUP → STARTER_REWARD → ROUTE_MAP → ENCOUNTER_PREVIEW → BATTLE|EVENT → RESULT/REWARD → ROUTE_MAP → … → RUN_RESULT`.

## Battle flow
`OBSERVING → PIECE_SELECTED → ACTION_PREVIEW → COMMIT → RESOLUTION_PRESENTATION → OBSERVING`.

Auxiliary battle states are INSPECTING, ABILITY_TARGETING, PAUSED, and TERMINAL.

## Select / preview / commit
1. Select piece.
2. Show legal moves/captures/abilities and illegality context.
3. Select valid destination/target.
4. Show direct consequence preview and known trigger warnings.
5. Commit immediately for standard Move/Capture; use explicit confirmation only where UX-SYSTEM requires it.
6. Lock action input during resolution presentation.
7. Return to OBSERVING only after stable state/presentation boundary.

## Inspect semantics
Inspect never mutates state or consumes turn/RNG. It must be available for pieces, tiles, rules, board features, encounter mechanics, reward cards, and route nodes where relevant.

## Reward flow
`OFFER → CANDIDATE_SELECTED → TARGET_SELECTED (Piece only) → CAPACITY_CHECK → OPTIONAL_REPLACEMENT → CONFIRM → APPLIED`.

Cancel before APPLIED returns to a prior reward state with the same persisted offer.

## Route flow
Only outgoing nodes from current node are selectable. Node presentation exposes category/risk/reward before selection. Choosing a node commits route progression according to Run contract; no route Undo is assumed.

## Event flow
`EVENT_PRESENTED → CHOICE_PREVIEW → OPTIONAL_CONFIRM → OUTCOME → EVENT_RESULT`.

Irreversible choices require confirmation. Mechanical outcome must be displayed before commit.

## Input intents
- Select
- Confirm
- Cancel
- Inspect
- OpenBuild
- OpenRules
- OpenHistory
- Pause
- Pan
- Zoom

Physical bindings remain platform adapters.

## Presentation lock
During battle resolution the presentation layer may animate queued events, but no new gameplay input is accepted. Fast-forward/reduced-motion may compress presentation while preserving logical ordering.
