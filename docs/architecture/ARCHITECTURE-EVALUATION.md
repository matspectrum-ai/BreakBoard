# Architecture Evaluation v0.1

Status: **SPECIFIED — BB-ARCH-GATE-001 CLOSED**
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
- pure TypeScript domain can be isolated from renderer and OS;
- direct JSON Schema 2020-12 validation with Ajv2020;
- Vitest + fast-check directly satisfy RED/property/metamorphic requirements;
- PixiJS is a focused 2D renderer with a production WebGL path;
- semantic DOM/CSS handles text-heavy accessible UI while Pixi owns the board/VFX surface;
- Tauri provides desktop shell/native persistence/packaging without moving game rules to Rust.

Primary risk: Tauri relies on system WebViews, so platform rendering behavior is not byte-identical. Mitigation is strict separation of gameplay authority plus real-browser and packaged-platform QA; WebGPU is excluded from v0.1.

### B — Godot 4.x + GDScript
Capable integrated game engine with mature 2D and exports, but the BreakBoard contract/schema/property-test workflow would require more custom infrastructure and creates more pressure toward scene/gameplay coupling than the selected architecture needs.

### C — Defold + Lua
Small capable 2D runtime with broad platform reach, but requires more bespoke JSON Schema/property-test/type tooling for this contract-heavy project.

## Weighted BreakBoard-specific judgment
| Criterion | Weight | TS + Pixi + Tauri | Godot + GDScript | Defold + Lua |
|---|---:|---:|---:|---:|
| Contract/schema fit | 20 | 20 | 13 | 11 |
| RED/property testing fit | 20 | 20 | 12 | 10 |
| Coding-agent buildability | 15 | 15 | 13 | 11 |
| 2D/procedural visual fit | 15 | 14 | 15 | 13 |
| Accessible UI fit | 10 | 10 | 7 | 6 |
| Desktop/touch portability | 10 | 9 | 10 | 10 |
| Runtime/build simplicity | 5 | 4 | 4 | 5 |
| Domain/presentation isolation | 5 | 5 | 4 | 4 |
| **Total /100** | **100** | **97** | **78** | **70** |

Scores are architecture judgments for BreakBoard, not external performance benchmarks.

## Final decision
**TypeScript strict + PixiJS 8/WebGL + semantic DOM/CSS + Tauri 2** is the accepted v0.1 architecture. Node 24 LTS/pnpm/Vite provide build tooling; Ajv2020 validates canonical schemas; Vitest/fast-check provide core verification; Rust remains platform-adapter-only.

RNG is bound separately by `RNG-SPEC.md`. Persistence is bound by ADR-002. Content pipeline, test harness, presentation bridge, packaging QA and dependency enforcement are now specified in the other Architecture documents.

## Architecture closure result
All Architecture micro-gates are closed. The implementation-readiness audit is PASS, but the execution lock remains in force by explicit user instruction. No implementation artifacts are created until a later explicit authorization.

## Evidence reviewed
Official project documentation was used for the architecture choice, including Tauri 2 architecture/distribution/testing, PixiJS 8 renderers, Ajv JSON Schema 2020-12 support, Vitest projects/browser mode, fast-check property testing, and the xoshiro/xoroshiro reference.
