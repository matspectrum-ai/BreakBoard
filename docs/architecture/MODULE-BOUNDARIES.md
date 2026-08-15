# Module & Dependency Boundaries v0.1

Status: **SPECIFIED — BB-ARCH-GATE-001 CLOSED**

## Principle
BreakBoard is command/resolution driven. The renderer is a projection of authoritative state/events; it is not the world model.

## Canonical implementation layout

```text
specs/contracts/                 canonical JSON Schemas
content-src/                     authored declarative JSON/restricted-YAML
generated/                       derived content/types; never source of truth
packages/
  contract-types/                generated TS shapes
  domain/                        pure deterministic game domain
  catalog/                       content build/load/validation
  application/                   Run/Battle use cases + ports
apps/
  game/                          semantic DOM + PixiJS presentation
tests/                           contract/domain/browser/e2e fixtures/suites
src-tauri/                       native shell/adapters only
```

Physical names can change only through architecture maintenance while preserving the normative dependencies below.

## BB-ARCH-MOD-001 — Domain
`domain` owns typed identities, Game/Battle/Run/Profile state, Rule Query algebra, legality, operations/transactions, reactions/scheduled effects, bounded resolution, deterministic RNG/namespaces, lifecycle/victory and deterministic generation primitives.

It cannot import PixiJS, Tauri, DOM/Web Audio, filesystem/database/network, wall-clock/timer authority or ambient randomness. The narrow portable SHA-256 implementation allowed by `BOUNDARY-ENFORCEMENT.md` is deterministic infrastructure and remains golden-vector constrained.

## BB-ARCH-MOD-002 — Contract types
`contract-types` is generated from canonical JSON Schemas. It contains no gameplay behavior and never outranks schemas/normative prose contracts.

## BB-ARCH-MOD-003 — Catalog
`catalog` owns restricted-YAML/JSON parsing for authoring/build tooling, Ajv2020 shape validation, semantic/reference/compatibility/primitive validation, reward satisfiability, content-version registry, deterministic bundle production and immutable runtime ContentRegistry construction.

Domain defines the minimal read-only ContentRegistry interface it consumes; catalog implements it. Domain never knows source files/paths.

## BB-ARCH-MOD-004 — Application
`application` orchestrates create/resume Run, route choice, Battle construction/action submission, reward choice/replacement, battle/run completion, profile transactions and persistence requests. It depends on domain abstractions and ports, not renderer/native implementation details.

## BB-ARCH-MOD-005 — Presentation
`apps/game` owns semantic DOM, Pixi board, input adapters, presentation state, domain-event-to-VFX/Audio/Haptic mapping, animation/feed playback and accessibility projection. It calls application commands and never mutates authoritative state.

Pixi ticker/frame delta controls presentation only; no frame-driven authoritative game loop exists.

## BB-ARCH-MOD-006 — Tauri shell
`src-tauri` owns app-local paths, persistence locking/generation files, filesystem durability/recovery, window/native integration and packaging bridges. Rust may verify persistence-envelope integrity but cannot decide movement, mutations, RNG selection, reward eligibility, victory or run outcomes.

## BB-ARCH-MOD-007 — Ports
Minimum ports include `PersistencePort`, validated `ContentRegistry` access, optional diagnostics and an OS-entropy source used only to create a new RunSeed before GenerationIdentity exists. No gameplay ClockPort exists.

## BB-ARCH-MOD-008 — Dependency direction

```text
JSON Schemas -> generated contract-types -> domain -> application -> presentation
                                          ^
                                          |
                                       catalog

application -> PersistencePort <- Tauri adapter
```

Composition root owns concrete wiring. Platform/presentation point inward; domain never points outward.

## BB-ARCH-MOD-009 — Event boundary
A completed domain/application command yields authoritative result plus immutable ordered DomainEvents. Persistence/presentation may consume that result; animation/audio duration cannot change it.

## BB-ARCH-MOD-010 — Verification boundary
Domain and most catalog/application tests run in Node/Vitest without browser/Pixi/Tauri. Browser presentation and packaged Tauri adapter tests are separate projects/suites. A renderer failure cannot prevent deterministic rules tests from executing.

## Enforcement
The exact static enforcement is normative in `BOUNDARY-ENFORCEMENT.md`; content, persistence, testing and presentation boundaries are further specified by their Architecture documents.
