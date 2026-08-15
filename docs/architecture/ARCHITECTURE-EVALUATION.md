# Architecture Evaluation v0.1

Status: **IN PROGRESS — BB-ARCH-GATE-001**
Evaluation date: 2026-08-15

## Objective
Select the implementation architecture that best satisfies the already-closed BreakBoard design/contracts without introducing unnecessary engine complexity, weak testability, or asset-production burden.

## Hard requirements inherited from previous gates
- deterministic domain resolution independent from rendering timing;
- JSON Schema 2020-12 remains canonical for data-shape contracts;
- RED-first unit/property/scenario/contract verification;
- content remains declarative and may not inject arbitrary code;
- presentation cannot mutate authoritative gameplay state;
- desktop-first launch with touch-compatible architecture;
- procedural/vector-first 2D presentation under Broken Geometry;
- no backend required for the base game;
- stable save/resume at Run checkpoints;
- exact reproducible RNG namespaces and golden vectors;
- implementation must remain practical for coding agents and a small project.

## Candidates reviewed

### A — TypeScript + PixiJS 8 + Tauri 2
Strengths:
- TypeScript domain can be isolated as ordinary pure modules with no renderer or OS dependency.
- JSON Schema 2020-12 can be validated directly with Ajv 8/Ajv2020.
- Vitest + fast-check map directly to the required example/property/metamorphic verification model.
- PixiJS 8 provides a focused 2D renderer, primitive/SVG drawing, scene graph, pointer/touch interaction, and a production-recommended WebGL renderer.
- DOM/CSS can implement menus/cards/accessibility while PixiJS owns only the tactical board/VFX surface.
- Tauri 2 supplies a native desktop shell and distribution path without moving game rules into Rust.
- Tauri 2 preserves an Android/iOS path if mobile becomes a later product target.

Primary risk:
- Tauri uses the operating system WebView, so renderer/browser capability differs by platform/version.

Mitigation:
- v0.1 explicitly targets PixiJS WebGL, not WebGPU;
- gameplay logic never depends on browser timing or rendering;
- browser-mode tests plus packaged Tauri smoke tests are separate gates;
- graphics remain intentionally modest and procedural.

### B — Godot 4.7.x + GDScript
Strengths:
- integrated open-source game engine with mature 2D, UI, input, audio, packaging, and editor workflows;
- strong fit for a board game visually;
- direct desktop/mobile export paths.

Tradeoff against BreakBoard contracts:
- JSON Schema 2020-12, property-based testing/shrinking, and the RED-first pure-domain test model require more custom/plugin infrastructure than the TypeScript option;
- stronger temptation to couple scene nodes and gameplay authority unless discipline is maintained.

### C — Defold 1.13.x + Lua
Strengths:
- very small runtime/editor footprint;
- strong cross-platform focus and good 2D suitability.

Tradeoff against BreakBoard contracts:
- less direct fit for JSON Schema-driven type generation/validation and property-based test tooling;
- more bespoke infrastructure would be needed for our formal contract/test requirements.

## Weighted architecture judgment
The following scores are BreakBoard-specific engineering judgments, not external benchmarks.

| Criterion | Weight | TS + Pixi + Tauri | Godot + GDScript | Defold + Lua |
|---|---:|---:|---:|---:|
| Contract/schema fit | 20 | 20 | 13 | 11 |
| RED/property testing fit | 20 | 20 | 12 | 10 |
| Coding-agent buildability | 15 | 15 | 13 | 11 |
| 2D/procedural visual fit | 15 | 14 | 15 | 13 |
| Accessible UI fit | 10 | 10 | 7 | 6 |
| Desktop/touch portability | 10 | 9 | 10 | 10 |
| Runtime/build simplicity | 5 | 4 | 4 | 5 |
| Isolation of domain from presentation | 5 | 5 | 4 | 4 |
| **Total /100** | **100** | **97** | **78** | **70** |

## Decision
**Selected architecture candidate: TypeScript + PixiJS 8 + Tauri 2.**

This selection is accepted in ADR-001. It does not unlock production implementation until the remaining Architecture micro-gates are closed.

## Current official evidence used for the decision
- Godot official archive: 4.7.1 is the current stable patch line as of this evaluation: https://godotengine.org/download/archive/
- Defold official release/news: 1.13.0 is the current stable release line reviewed: https://defold.com/2026/06/22/Defold-1-13-0/
- Tauri 2 architecture and platform model: https://v2.tauri.app/concept/architecture/
- Tauri 2 cross-platform/distribution documentation: https://v2.tauri.app/distribute/
- PixiJS 8 architecture/renderers: https://pixijs.com/8.x/guides/concepts/architecture and https://pixijs.com/8.x/guides/components/renderers
- Vitest official guide: https://vitest.dev/guide/
- fast-check official docs: https://fast-check.dev/docs/introduction/
- Ajv JSON Schema 2020-12 support: https://ajv.js.org/json-schema.html
- Node.js 24 LTS status: https://nodejs.org/en/blog/release/v24.11.0

## Next architecture work
- finish module/dependency boundaries;
- specify build-time/runtime content pipeline;
- specify atomic persistence adapter;
- specify concrete test harness and RED bootstrap;
- specify browser/Tauri verification matrix and packaging baseline;
- perform final Architecture consistency/unlock audit.
