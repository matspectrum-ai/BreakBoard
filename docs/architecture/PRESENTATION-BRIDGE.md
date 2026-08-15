# Application-to-Presentation Bridge v0.1

Status: **SPECIFIED — BB-ARCH-GATE-001 CLOSED**

## Objective
Guarantee that DOM, PixiJS, audio, haptics, animation timing and Tauri execution remain projections/adapters around already-resolved authoritative state.

## BB-ARCH-PRS-001 — Input direction
Canonical interaction path:

```text
mouse/touch/keyboard
  -> presentation input adapter
  -> typed application command / ActionIntent
  -> application use case
  -> pure domain validation/resolution
  -> authoritative result + ordered DomainEvents
  -> presentation bridge
  -> DOM/Pixi/Audio/Haptic projections
```

Presentation never writes GameState/RunState directly.

## BB-ARCH-PRS-002 — Application result envelope
A state-changing application command returns one of:
- `Rejected`: stable semantic rejection/fault information and unchanged authoritative state reference/read model;
- `Resolved`: new authoritative state/read model, immutable ordered DomainEvents, lifecycle/result metadata, and persistence recommendation when a stable checkpoint was reached.

Presentation copy, animation instructions and sound files are not embedded in domain Events.

## BB-ARCH-PRS-003 — Projection identities
Pixi objects are keyed from semantic Piece/Tile/link identities. DOM screens/cards consume application read models. Renderer object identity is disposable and never becomes a domain identifier.

After every presentation batch, the board/UI can be reconciled from the current authoritative read model rather than relying on animation history to remain correct.

## BB-ARCH-PRS-004 — Presentation queue
For a `Resolved` command, ordered DomainEvents are mapped into presentation intents/batches. The presentation queue may coalesce audio/VFX as specified by presentation contracts, but may not reorder causal information in the Resolution Feed or feed transformed events back into domain resolution.

Skipping, accelerating, reducing motion, muting or failing an animation changes no authoritative state.

## BB-ARCH-PRS-005 — Input barrier
During resolution presentation the UI may hold a presentation-only input barrier so the player cannot submit a second gameplay command before the resolved batch is visually acknowledged. This is not a domain `RESOLVING` state: the authoritative resolution has already reached its canonical stable result.

Skip/fast-forward releases the barrier by reconciling directly to the final read model. Presentation duration never advances turns or RNG.

## BB-ARCH-PRS-006 — Failure recovery
If Pixi/Web Audio/haptic playback throws or a presentation batch is interrupted, the app:
1. reports a presentation diagnostic;
2. discards/rebuilds transient presentation objects as needed;
3. reconciles DOM/Pixi from the latest authoritative read model;
4. never reruns the domain command merely to recreate animation.

A renderer failure may become a product-level fatal UI error, but it cannot create an alternate game outcome.

## BB-ARCH-PRS-007 — Tauri parity
Browser-development mode and Tauri mode share the same TypeScript domain/application/catalog packages. Tauri IPC is limited to platform ports such as persistence and OS integration.

Given the same content registry, authoritative state and command, browser and Tauri execution must produce identical serialized domain/application results. Platform persistence timing may differ, but persistence success/failure is reported through the port rather than changing the command's game rules.

## BB-ARCH-PRS-008 — Board renderer
PixiJS WebGL renders board/pieces/VFX and handles board pointer/touch hit-testing. It does not calculate legal targets. Legal/preview data comes from application/domain queries and is merely visualized.

The Pixi ticker controls animation only. No authoritative `update(delta)` gameplay loop exists.

## BB-ARCH-PRS-009 — DOM boundary
Menus, mutation cards, route map shell, rules/help, settings and accessibility surfaces use semantic DOM/CSS. Focus order and accessible names cannot be inferred solely from canvas graphics. Board interaction must expose equivalent contextual information/actions through the application/DOM accessibility layer required by UX contracts.

## Verification matrix
- same command in browser adapter and Tauri-backed app yields equal authoritative result;
- Pixi disabled/mock mode still permits domain/application tests;
- reduced-motion vs normal animation yields equal authoritative result hash;
- audio/haptics off vs on yields equal authoritative result hash;
- skipped presentation batch reconciles to same board/read model;
- illegal action produces rejection reason without mutation/presentation fake commit;
- Capture/Explosive chain preserves domain event order in Resolution Feed while audio may aggregate;
- keyboard/touch adapters submit semantically identical commands for equivalent interaction.
