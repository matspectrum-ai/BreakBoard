# Content Authoring, Build & Runtime Pipeline v0.1

Status: **SPECIFIED — BB-ARCH-GATE-001 CLOSED**

## Objective
Bind the already-specified declarative Content System to an auditable build/runtime pipeline without making source serialization or presentation assets part of gameplay authority.

## BB-ARCH-CNT-001 — Authoring source
Authored gameplay content lives under `content-src/` as one definition per file. Accepted source formats are `.json` and `.yaml`/`.yml`, but every document must decode to the same JSON data model governed by `specs/contracts/content.schema.json` and the semantic Content Contracts.

YAML is restricted to a JSON-compatible, audit-friendly subset: duplicate keys, custom tags, merge keys, anchors/aliases, executable extensions, non-finite numbers, and implementation-specific object types are forbidden. A parsing ambiguity is a build error.

## BB-ARCH-CNT-002 — Stable source identity
The file path is organizational only. The authoritative identity is the definition's stable `ContentId`. File rename does not rename content. Duplicate ContentIds across source files are a hard error.

## BB-ARCH-CNT-003 — Pipeline stages
The canonical content build is deterministic and fail-closed:
1. discover authored files in stable path order;
2. decode UTF-8 and parse JSON/restricted YAML;
3. normalize strings to Unicode NFC where the relevant contract calls for normalized semantic text/IDs;
4. validate data shape with Ajv 8's JSON Schema 2020-12 implementation in strict configuration;
5. run semantic validators for cross-references, kinds, compatibility, requires/excludes/overrides, Rule Query hooks/contributions, selector/effect vocabulary, FORCED-removal authority, stacking/lifetime rules, and region/complexity constraints;
6. run whole-catalog validators including reward satisfiability and no-content-specific-engine-branch representability;
7. canonicalize definition ordering by stable ContentId and nested semantic-set ordering;
8. emit the immutable runtime bundle and manifest;
9. rerun runtime-facing validation against emitted artifacts before build success.

No stage silently fixes invalid gameplay content.

## BB-ARCH-CNT-004 — Runtime artifacts
Build output is conceptually:

```text
generated/content/<content_version>/
  content.bundle.json
  content.manifest.json
```

`content.bundle.json` contains the complete normalized gameplay catalog required by the shipped content version. `content.manifest.json` contains at least:
- pipeline format/version;
- `content_version`;
- `ruleset_version` compatibility;
- schema family versions;
- ordered ContentId inventory and count;
- SHA-256 digest of RFC-8785-compatible canonical bytes of the gameplay bundle;
- required presentation-token inventory whose absence would make canonical gameplay state unreadable.

Build timestamps, local paths and machine-specific metadata are excluded from the semantic bundle digest.

## BB-ARCH-CNT-005 — Runtime loading
Production runtime loads only compiled JSON artifacts, not YAML authoring files. Runtime load must:
1. parse manifest/bundle;
2. verify format/version compatibility;
3. recompute and compare bundle digest;
4. run schema validation;
5. run runtime-required semantic/cross-reference validation;
6. construct an immutable `ContentRegistry`;
7. expose the registry only after all checks pass.

Invalid/missing content is a startup/content compatibility failure. Runtime may not invent fallback definitions or silently drop invalid entries.

## BB-ARCH-CNT-006 — Generated TypeScript types
`packages/contract-types` may be generated from canonical JSON Schemas to improve TypeScript ergonomics. Generated types are disposable derivatives; schemas plus normative semantic contracts remain authoritative.

A schema/type generation mismatch fails CI.

## BB-ARCH-CNT-007 — Development reload
Development hot reload may rebuild/reload content, but it must invoke the same validation stages as production. It cannot use a permissive alternate parser or validator.

An active deterministic Run may not silently swap `content_version` or its eligible-content snapshot because source files changed during development.

## BB-ARCH-CNT-008 — Presentation assets
Presentation tokens referenced by content resolve through a presentation asset registry. Missing purely decorative assets may have an explicit neutral fallback. Missing tokens required for canonical readability/accessibility are validation errors. Art, audio and localization never alter gameplay mechanics.

## BB-ARCH-CNT-009 — Deterministic build verification
Two clean content builds from identical repository inputs must produce byte-equivalent canonical gameplay bundles and identical manifest digests. This becomes a CI reproducibility check.

## RED-first implementation obligations
Before implementing the pipeline, tests must fail for at least:
- duplicate ContentId;
- invalid/unknown Rule Query hook;
- content attempting FORCED removal;
- unresolved cross-reference;
- YAML duplicate/merge/custom-tag behavior;
- starter/reward satisfiability failure;
- nondeterministic input file ordering producing identical canonical output;
- tampered runtime bundle digest;
- runtime content-version mismatch;
- identical clean builds producing identical canonical digest.
