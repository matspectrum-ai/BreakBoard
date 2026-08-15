# Dependency & Determinism Boundary Enforcement v0.1

Status: **SPECIFIED — BB-ARCH-GATE-001 CLOSED**

## Objective
Make module boundaries machine-enforceable instead of relying only on agent discipline/code review.

## BB-ARCH-BND-001 — Workspace projects
The implementation uses pnpm workspaces plus TypeScript Project References/composite package boundaries for `contract-types`, `domain`, `catalog`, `application` and presentation/tooling projects. Package `exports` expose only supported public entry points; cross-package relative-path imports are forbidden.

## BB-ARCH-BND-002 — Dependency graph
Normative runtime direction:

```text
contract-types
    ↑
  domain
   ↑  ↑
catalog application
      ↑
 apps/game composition/presentation

application -> PersistencePort <- Tauri adapter
```

`catalog` may depend on domain port/types to implement the read-only ContentRegistry interface; domain never imports catalog. The application depends on domain abstractions; a composition root wires validated catalog and persistence adapters.

Circular package/module dependencies are forbidden.

## BB-ARCH-BND-003 — Static dependency check
`dependency-cruiser` is the architecture dependency graph checker. CI treats as errors at least:
- any circular dependency;
- domain -> application/catalog/apps/src-tauri;
- domain -> PixiJS/Tauri/browser/audio/filesystem packages;
- application -> PixiJS/DOM/Tauri concrete APIs;
- catalog -> presentation/Tauri;
- production packages -> tests/fixtures;
- unresolvable imports;
- cross-layer bypasses not included in the allow graph.

An exception requires an ADR/architecture change, not an inline ignore added solely to make CI green.

## BB-ARCH-BND-004 — TypeScript environment isolation
`domain` compiles under a TypeScript lib/environment that does not expose DOM or Node globals as ambient authority. It must compile/typecheck independently from `apps/game`.

Node-only types belong to tooling/catalog build code where needed; DOM types belong to presentation. Tauri APIs belong to adapter/composition code.

## BB-ARCH-BND-005 — Forbidden ambient authority
Domain source is linted/checked against authoritative use of:
- `Math.random`;
- `Date`, `Date.now` or wall-clock-derived decisions;
- browser `crypto.getRandomValues` except outside-domain initial RunSeed entropy adapter;
- timers as game progression;
- filesystem/network reads;
- environment variables;
- renderer state/ticker/delta time.

OS cryptographic entropy is permitted only through the explicit seed-creation adapter before GenerationIdentity exists.

## BB-ARCH-BND-006 — SHA-256 implementation dependency
For portable synchronous deterministic SHA-256 used by namespace derivation/canonical digests, the implementation baseline permits the audited/minimal `@noble/hashes` v2 line, imported narrowly from its SHA-2 module. Its outputs must still satisfy BreakBoard golden vectors; library behavior is not trusted in place of those tests.

Changing hash library is allowed only if golden behavior remains identical and dependency review passes. The algorithm identity cannot silently change.

## BB-ARCH-BND-007 — Lint defense in depth
ESLint/AST restrictions supplement dependency-cruiser for forbidden globals/imports and direct private-package imports. A lint disable that weakens a Kernel determinism boundary requires documented justification and architecture review.

## BB-ARCH-BND-008 — CI gate
Conceptual `check:boundaries` fails before tests/build packaging when architecture rules are violated. The implementation bootstrap must establish:
- workspace/package graph;
- TS project references;
- package exports;
- dependency-cruiser rules;
- deterministic-domain lint restrictions;
- no-circular check.

No production feature implementation may normalize a failing boundary check as technical debt.
