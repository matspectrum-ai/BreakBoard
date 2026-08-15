# Concrete RED-First Test Harness v0.1

Status: **SPECIFIED — BB-ARCH-GATE-001 CLOSED**

## Objective
Bind the technology-neutral Verification Plan to concrete runners without turning renderer/platform integration into a prerequisite for testing game rules.

## BB-ARCH-TST-001 — Vitest project split
The TypeScript workspace uses Vitest projects with separate environments:
- `contracts`: Node — JSON Schema fixtures, generated-type consistency, canonicalization/golden formats;
- `domain`: Node — rule queries, legality, OperationGroups, reactions, bounded resolution, RNG, battle/run logic;
- `catalog`: Node — content parsing/build/semantic/reference/satisfiability validators;
- `application`: Node — use cases, ports, stable checkpoint orchestration;
- `browser`: real browser — DOM/Pixi presentation bridge, focus/input/accessibility and rendering integration.

Node projects must run without loading PixiJS, DOM or Tauri.

## BB-ARCH-TST-002 — Browser runner
Vitest Browser Mode uses the Playwright provider. Baseline CI instances are Chromium and WebKit. Browser tests interact through semantic roles/real browser interaction where possible rather than synthetic DOM-only assumptions.

Browser tests verify presentation adapters; they do not reimplement canonical game rules as expected values.

## BB-ARCH-TST-003 — Tauri E2E
Packaged/native-shell verification is a separate WebdriverIO suite using the Tauri-supported WebDriver service. It verifies native IPC, actual application launch, persistence round trip, window/app lifecycle, WebView/Pixi startup and a small number of end-to-end player flows.

Tauri E2E is never required to run a domain unit/property test.

## BB-ARCH-TST-004 — Rust adapter tests
`src-tauri` runs `cargo test` for native persistence/path/locking/recovery helpers. Rust tests may use temporary directories and fault injection. They may not encode game-rule expectations beyond persistence envelopes/port semantics.

## BB-ARCH-TST-005 — fast-check policy
Property-based tests use fast-check with failure reproducibility treated as an artifact:
- every failure records seed and counterexample path;
- minimized counterexamples that reveal distinct bugs are promoted to permanent regression fixtures;
- P0 property suites use at least 500 generated cases in normal CI unless the property contract specifies a stronger bound;
- scheduled/nightly verification targets at least 5,000 cases for high-value determinism/state/resolution properties;
- reducing run counts to make a failure disappear is forbidden without an explicit test-plan change.

Property generators distinguish valid-state generation from near-valid invalid-state generation so schema/invariant failures are intentional.

## BB-ARCH-TST-006 — RED evidence
After implementation is explicitly authorized, behavior work follows:
1. identify authoritative spec/contract and test ID;
2. add/materialize a test that executes correctly and fails because required behavior is absent/wrong;
3. record the expected RED failure reason;
4. implement minimum behavior to GREEN;
5. refactor while preserving GREEN;
6. run impacted property/scenario/metamorphic suites.

A syntax error, missing test runtime, broken fixture loader or unconfigured environment does not count as RED.

P0 tests cannot be committed as skipped/TODO once their behavior implementation begins.

## BB-ARCH-TST-007 — Naming/placement
Test names include the canonical verification ID, e.g. `RSL-ATOM-003 capture cancellation prevents relocation`. Conceptual layout:

```text
tests/
  contracts/
  domain/
  catalog/
  application/
  browser/
  fixtures/
  regression/
  e2e-tauri/
```

Physical test placement may be colocated by package if test IDs remain searchable and project boundaries remain equivalent.

## BB-ARCH-TST-008 — Coverage
V8 coverage may be collected for TypeScript projects as a diagnostic. A percentage target is not a substitute for named P0 contract/scenario/property obligations. CI fails on missing/disabled required test IDs even if line coverage is high.

## BB-ARCH-TST-009 — Determinism checks
CI includes golden/metamorphic tests for:
- RNG vectors and namespace isolation;
- canonical content bundle digest;
- representation/order independence;
- audio/animation/settings changes not affecting domain result;
- browser/Tauri adapter mode not changing authoritative results;
- persistence round-trip semantic equality.

## BB-ARCH-TST-010 — CI tiers
Baseline tiers:
- **fast / every change:** format/lint/typecheck/boundaries, contract/domain/catalog/application tests, core content validation;
- **browser / every pull request:** Chromium + WebKit presentation tests;
- **native / main and release candidates:** Rust tests plus Tauri packaged smoke/E2E on supported desktop matrix;
- **extended / scheduled:** larger fast-check runs, generation/satisfiability sweeps, reproducibility builds and long-chain resolution stress.

Implementation bootstrap must create these harnesses before production behavior beyond the smallest RED/Green slice is allowed.
