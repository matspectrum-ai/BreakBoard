# UX & Interaction System v0.1

Status: **SPECIFIED — BB-UX-GATE-001 CLOSED**

## Objective
Make BreakBoard's systemic depth legible, controllable, inspectable, and consistent across desktop and touch-oriented layouts without coupling UX contracts to a rendering engine or visual art style.

## BB-UX-001 — Information architecture
Primary surfaces are: Main Menu, Run Setup, Route Map, Encounter, Reward, and Run Result. Overlays are Build, Rules, Inspect, Settings, and Pause. Additional screens require explicit specification rather than ad-hoc proliferation.

## BB-UX-002 — State before style
UX requirements define what must be communicated and controlled; Art Direction decides how it looks. Example: Core status must be unmistakable, but the final icon/halo/shape is an Art decision.

## BB-UX-003 — Battle board priority
The board is the primary battle surface. Panels may not obscure critical topology, occupancy, Core status, legal targets, active hazards, or Collapse warnings.

## BB-UX-004 — Piece readability
Each piece must communicate owner, archetype, selected state, Core classification, and whether mutations are present. Mutation count may use compact badges/pips; exact mutations are available through Inspect.

Core classification is visual state, not archetype identity. A Crowned Seer must communicate both Seer and Core status.

## BB-UX-005 — Tile readability
Every tile must communicate existence, occupancy, board feature, selection state, legal-target state, and danger/warning state when relevant. Missing/destroyed topology must never be visually confused with an empty playable tile.

## BB-UX-006 — Input abstraction
Canonical input intents are: Select, Confirm, Cancel, Inspect, OpenBuild, OpenRules, OpenHistory, Pause, Pan, and Zoom. Mouse/keyboard/touch bindings are adapters; domain and battle logic never depend on gestures or physical controls.

Baseline mappings may include click/tap for Select, right-click/hover or long-press for Inspect, Escape/back/tap-outside for Cancel, wheel/pinch for Zoom, and drag for Pan.

## BB-UX-007 — Battle interaction state machine
Battle interaction states are conceptually OBSERVING, PIECE_SELECTED, ACTION_PREVIEW, ABILITY_TARGETING, INSPECTING, RESOLUTION_PRESENTATION, PAUSED, and TERMINAL.

No gameplay Action may be submitted during RESOLUTION_PRESENTATION. This mirrors the stable-state Battle contract.

## BB-UX-008 — Legal action feedback
Selecting a piece exposes legal destinations/actions before commit. Move, Capture, Ability target, dangerous-but-legal state, and illegal state require distinct non-color-only visual semantics.

Illegal attempts surface a deterministic reason from the rules layer (for example blocker, missing destination, Sanctuary Capture denial, invalid Pawn direction) and do not mutate state.

## BB-UX-009 — Preview policy
Before commit, UX exposes complete legality and direct deterministic consequences that are already known at the action boundary, plus relevant known trigger warnings. The final state of an arbitrary reaction chain is hidden by default rather than auto-solving the tactical puzzle.

This policy may be revisited only by explicit product specification.

## BB-UX-010 — Inspect
Inspect is universally available without committing an Action. Piece Inspect includes archetype, owner, Core status, movement summary, mutations, temporary states, and relevant active rules. Tile Inspect includes feature, links, hazards, and mechanical text. Rule Inspect includes exact scope/source and mechanical description.

## BB-UX-011 — Perfect information baseline
Base v0.1 UX is perfect-information: mechanically relevant enemy mutations, encounter rules, board features, and visible state are inspectable before the player commits. Hidden-information content is not part of v0.1 and would require a new product/content contract.

## BB-UX-012 — Encounter and Boss preview
Before battle, Encounter Preview exposes encounter identity/template-level description, known enemy mechanics, board features, and special rules. Boss mechanics are explicitly disclosed and remain visible through Encounter Rules during battle; core boss behavior may not be hidden only in flavor text.

## BB-UX-013 — Active rules presentation
Player Rule Mutations and encounter/enemy rules are persistently accessible and visually distinguished by source. Rule trigger feedback identifies the rule that caused the visible consequence.

## BB-UX-014 — Collapse telegraphing
Current turn/round context and Collapse status are visible. Before a Collapse step, the affected ring/tiles and occupants receive a non-color-only warning. Collapse is strategic pressure, not surprise punishment.

## BB-UX-015 — Resolution feedback
The presentation layer receives resolved state plus deterministic event sequence; animation never drives logic. A concise latest-resolution summary is visible, while full grouped action/event history is expandable.

Trigger feedback should associate visible consequences with their causes (for example EXPLOSIVE, MOMENTUM +1 ACTION) without exposing technical engine logs.

## BB-UX-016 — Animation semantics
Animation categories include movement, capture, death, spawn, teleport, tile modification/destruction, mutation trigger, rule trigger, Collapse, and victory/defeat. Skipping, speeding up, or reducing animation never changes GameState or event ordering.

## BB-UX-017 — Mutation reward flow
Reward cards show name, rarity/category, concise mechanical summary, eligible targets, and relevant compatibility information. Selection is non-destructive until explicit target and confirmation requirements are satisfied.

Starter reward: choose Mutation card → choose eligible owned piece → preview → confirm.

## BB-UX-018 — Capacity replacement
When a target/category is at capacity, UX requires explicit replacement selection and shows REMOVE X / ADD Y before confirmation. Cancel leaves persisted RewardOffer unresolved and state unchanged.

Rule replacement uses the same semantic contract and may use distinct presentation such as REWRITE A LAW.

## BB-UX-019 — Route map
Route nodes expose category, risk class, reward class, completion state, and available outgoing paths. Baseline risk vocabulary is STANDARD, DANGEROUS, ELITE, BOSS. Survey-type effects may reveal exact encounter templates using already-authorized RunState information.

## BB-UX-020 — Event choices
Events show mechanical consequences alongside flavor. Irreversible Event choices require explicit confirmation. Flavor text may enrich but never substitute for the mechanical outcome.

## BB-UX-021 — Confirmation policy
Confirmation is required for mutation/rule replacement, irreversible Event choices, and run abandonment. Ordinary battle movement/capture uses contextual preview and direct second input without modal confirmation unless a future mechanic explicitly requires it.

Base v0.1 has no Undo for committed Actions.

## BB-UX-022 — Desktop/touch scope
Desktop is the current launch-target baseline. Touch-compatible input abstraction and responsive layout are required architectural constraints, but a mobile release is not yet committed.

On narrow layouts, side panels become contextual/bottom-sheet surfaces while preserving the same information semantics. The standard board should fit by default; pan/zoom are auxiliary rather than mandatory to play.

## BB-UX-023 — Accessibility baseline
Required from initial UX architecture: non-color-only state encoding, sufficient contrast, text scaling, keyboard navigation, remappable controls, reduced motion, animation speed control, screen-shake toggle, and high-contrast support.

No critical mechanic may depend exclusively on hover.

## BB-UX-024 — Text precision levels
Content has at least concise gameplay text and an exact mechanical description. Concise text optimizes play; exact text is available through Details/Inspect. Keywords may abbreviate recurring concepts only when definitions/tooltips are readily available.

## BB-UX-025 — Build view
Between encounters the player can inspect army composition, Piece Mutations, and active Rule Mutations. The same build information is available read-only during battle.

## BB-UX-026 — Terminal explanations
Defeat/victory screens explain the mechanical terminal cause, including Final Core loss and Double Break semantics. Run Result summarizes route progress, battles, acquired mutations/rules, bosses, and new unlocks without requiring a currency/XP presentation.

## BB-UX-027 — Visual production constraint
All mandatory gameplay information must be representable with reusable geometry, typography, iconography, procedural/state-based effects, and constrained assets. UX must not depend on bespoke frame-by-frame character animation or high-volume handcrafted art.

## Structural validation
The UX contract covers legal/illegal actions, Explosive warnings, Armored inspection, multiple Cores, Portal linking/inert state, Fragile hazards, Collapse preview, Pass, Rule visibility, Boss rules, chain-resolution history, capacity replacement, Events, persisted rewards, touch use, color accessibility, reduced motion, and Double Break explanation.

## Deferred
Final visual style, final typography, concrete layout pixels, exact controller bindings, final mobile release commitment, audio design, concrete UI framework, rendering engine, tutorial implementation, localization implementation, and usability-test-derived tuning are separate gates.
