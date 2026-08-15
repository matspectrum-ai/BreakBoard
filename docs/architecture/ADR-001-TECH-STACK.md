# ADR-001 — BreakBoard implementation stack

Status: **ACCEPTED under BB-ARCH-GATE-001**
Date: 2026-08-15

## Decision
Use a web-native, renderer-separated architecture:

- **Language:** TypeScript in strict mode for all authoritative game/application logic.
- **Build/dev runtime:** Node.js 24 LTS line, Vite, pnpm.
- **2D renderer:** PixiJS 8 using **WebGL** as the v0.1 production renderer.
- **Menus/cards/accessibility UI:** semantic HTML + CSS + TypeScript DOM components; no SPA framework is required by the architecture.
- **Desktop shell:** Tauri 2.
- **Native shell language:** Rust only for OS integration, persistence/filesystem, packaging/native bridges. Rust must not contain game-rule authority.
- **Schema validation:** Ajv 8 using the draft-2020-12 class for canonical JSON Schema contracts.
- **Unit/contract runner:** Vitest 4 line.
- **Property/model testing:** fast-check.
- **Audio:** Web Audio/presentation layer behind the existing AudioIntent contract.
- **Backend:** none required for base v0.1.

Exact patch versions will be lockfile-pinned when the implementation bootstrap is authorized. Architecture decisions refer to stable major lines rather than floating `latest` dependencies.

## Why PixiJS rather than a full scene-centric game engine
BreakBoard gameplay advances by discrete commands and deterministic resolution boundaries, not by physics or frame-driven world simulation. A focused 2D renderer keeps the authoritative domain outside the render loop.

PixiJS owns:
- board/tile/piece display objects;
- vector/procedural graphics;
- animation/VFX playback;
- pointer/touch board interaction;
- render layers.

PixiJS does **not** own:
- legal move calculation;
- GameState;
- RNG;
- Battle/Run lifecycle;
- mutation/reaction resolution;
- victory;
- persistence semantics.

## Why DOM UI alongside PixiJS
BreakBoard contains text-heavy cards, rule explanations, menus, settings, collection views and accessibility requirements. Semantic DOM provides keyboard/focus/text scaling/accessibility behavior without forcing those surfaces into a canvas scene graph.

The tactical board remains one PixiJS surface embedded inside the application layout.

## Why Tauri 2
Tauri is a shell/OS adapter only. The TypeScript application can run in ordinary browser development mode, while Tauri supplies desktop windowing, app data paths, native persistence commands and packaging.

No game outcome may differ between browser-development mode and Tauri mode for the same authoritative input/state.

## Renderer compatibility rule
PixiJS WebGL is the v0.1 production baseline. WebGPU is explicitly **not** a v0.1 requirement because browser/WebView implementation differences would expand the verification matrix without gameplay benefit.

## Dependency policy
The authoritative domain package may not import:
- PixiJS;
- Tauri APIs;
- DOM/browser APIs;
- Web Audio;
- timers/wall clock;
- ambient random APIs;
- persistence adapters.

Any dependency that violates this rule requires a new ADR.

## Rejected alternatives
### Godot + GDScript
Not rejected as incapable. It is rejected for this project because the formal JSON Schema/property-testing workflow would require more custom infrastructure, while BreakBoard does not need physics or a frame-authoritative engine.

### Defold + Lua
Not rejected as incapable. It is rejected because the TypeScript ecosystem gives a materially cleaner fit for our contract validation, property testing, data tooling and accessible DOM UI requirements.

### Rust-native game stack
A Rust-native renderer/game engine would strengthen type safety but increase compile/tooling complexity and agent implementation burden without solving a game requirement that the selected architecture cannot solve.

## Consequences
Positive:
- one main language across domain, content tooling, application and presentation;
- strong property/schema tooling;
- browser-fast development loop;
- small native shell responsibility;
- natural touch and accessible UI path.

Costs/risks:
- OS WebView variation becomes a packaging QA concern;
- browser event/timing APIs must never leak into deterministic domain logic;
- Tauri/Rust toolchain is still needed for final desktop packaging;
- canvas and DOM focus/input integration needs explicit testing.
