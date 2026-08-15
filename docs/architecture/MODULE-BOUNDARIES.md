# Module & Dependency Boundaries v0.1

Status: **SPECIFIED candidate — BB-ARCH-GATE-001 IN PROGRESS**

## Principle
BreakBoard is command/resolution driven. The renderer is a projection of authoritative state/events; it is not the world model.

## Proposed repository implementation layout

```text
specs/contracts/                 canonical JSON Schemas
content-src/                     authored declarative content (JSON/YAML -> JSON model)
packages/
  contract-types/                generated TypeScript types; never source of truth
  domain/                        pure deterministic game domain
  catalog/                       content loading + schema/semantic validation
  application/                   Run/Battle use cases and ports
apps/
  game/                          DOM UI + PixiJS presentation
src-tauri/                       native shell/adapters only
```

Exact physical folder names may change only through an architecture update; the dependency semantics below are normative.

## BB-ARCH-MOD-001 — Domain package
`domain` owns:
- value objects/typed IDs;
- GameState/BattleState/RunState/ProfileState domain structures;
- rule query algebra;
- action legality;
- Operation/OperationGroup resolution;
- reaction/scheduled-effect resolver;
- bounded resolution and cycle projection;
- RNG implementation/namespaces;
- victory/lifecycle rules;
- deterministic generation primitives.

Forbidden imports:
- PixiJS;
- Tauri;
- DOM/Web APIs;
- Web Audio;
- wall clock/timers;
- ambient random APIs;
- filesystem/database APIs.

## BB-ARCH-MOD-002 — Contract types
`contract-types` is generated from canonical repository JSON Schemas. Generated TypeScript is disposable build output/source-derived code and never outranks the schemas plus normative semantic contracts.

No gameplay behavior lives here.

## BB-ARCH-MOD-003 — Catalog/content package
`catalog` owns:
- JSON Schema validation through Ajv2020;
- semantic cross-reference validation;
- compatibility validation;
- primitive/hook vocabulary validation;
- reward satisfiability offline validation;
- content-version registry;
- conversion of approved authored content into an immutable runtime `ContentRegistry` interface.

The `domain` package defines the minimal read-only `ContentRegistry` port it consumes; `catalog` implements that port. Domain does not know file paths or source serialization format.

## BB-ARCH-MOD-004 — Application package
`application` orchestrates use cases:
- create/resume Run;
- choose route;
- construct encounter/battle;
- submit ActionIntent;
- resolve reward choice/replacement;
- finish battle/run;
- evaluate profile unlock transactions;
- request persistence through ports.

It may depend on `domain` and validated content registry interfaces. It must not implement alternative game rules.

## BB-ARCH-MOD-005 — Presentation app
`apps/game` owns:
- semantic DOM screens/cards/settings;
- PixiJS board scene;
- input abstraction adapters;
- presentation state machine;
- mapping domain Events to VFX/Audio/Haptic intents;
- animation/resolution feed playback;
- accessibility presentation.

It submits commands/use cases to `application`. It never edits authoritative state objects directly.

PixiJS ticker/frame delta controls only presentation animation. Domain transitions occur only because an application command/lifecycle step is explicitly invoked.

## BB-ARCH-MOD-006 — Tauri shell
`src-tauri` owns only OS/platform concerns such as:
- application data/config directories;
- atomic save file adapter;
- file open/write/rename/fsync-equivalent recovery protocol selected later;
- window/native integration;
- packaging/update/store/native bridges if later required.

Rust shell code cannot decide movement, mutation behavior, rewards, victory, RNG selection, content eligibility, or run outcomes.

## BB-ARCH-MOD-007 — Ports
Application-defined ports include at minimum:
- `PersistencePort`;
- `ContentRegistryPort`/validated registry access;
- optional diagnostic/log sink;
- platform seed-creation entropy source used only before GenerationIdentity creation.

No gameplay ClockPort exists for ordinary resolution because wall-clock time is not game authority.

## BB-ARCH-MOD-008 — Dependency direction
Normative direction:

```text
JSON Schemas
   ↓ generate
contract-types
   ↓
domain ← validated ContentRegistry implementation (catalog)
   ↓
application
   ↓
apps/game presentation

application → PersistencePort ← Tauri adapter
```

Presentation and platform adapters point inward. Domain never points outward.

## BB-ARCH-MOD-009 — Domain event boundary
A completed resolution returns authoritative state/result plus immutable ordered domain Events. Application may persist stable state and forwards events to presentation. Presentation consumption, animation duration, skipped frames or audio state cannot alter the already-resolved domain outcome.

## BB-ARCH-MOD-010 — Test boundary
The entire `domain` and most `catalog/application` verification must execute under Node/Vitest without launching PixiJS, a browser or Tauri.

Browser/Tauri tests verify adapter/presentation integration separately. A failing renderer must not prevent deterministic domain unit/property tests from running.

## Remaining micro-gates
- exact content authoring/build pipeline;
- exact persistence protocol;
- concrete test harness/bootstrap conventions;
- browser/Tauri packaging/QA matrix;
- final dependency enforcement mechanism.
