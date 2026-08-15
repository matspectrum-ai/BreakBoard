# BreakBoard Master Specification

Status: **Planning / Implementation LOCKED**

This is the canonical index, not a duplicate of every specification.

## Foundation
- [Constitution](foundation/CONSTITUTION.md)
- [Vision](foundation/VISION.md)
- [Principles](foundation/PRINCIPLES.md)
- [Glossary](foundation/GLOSSARY.md)
- [Non-goals](foundation/NON-GOALS.md)

## Game design
- [Paper Prototype v0.1](game/PAPER-PROTOTYPE.md)
- [Canonical Game Rules v0.1](game/CANONICAL-RULES.md)
- [Mutation System v0.1](game/MUTATION-SYSTEM.md) — specified
- [Battle System v0.1](game/BATTLE-SYSTEM.md) — specified
- [Run & Progression System v0.1](game/RUN-SYSTEM.md) — specified

## Content
- [Content System v0.1](content/CONTENT-SYSTEM.md) — specified
- [Piece Mutations](content/PIECE-MUTATIONS.md)
- [Rule Mutations & Board Features](content/RULE-BOARD-CATALOG.md)
- [Encounters, Elites & Bosses](content/ENCOUNTERS-AND-BOSSES.md)
- [Events & Unlocks](content/EVENTS-AND-UNLOCKS.md)

## UX
- [UX & Interaction System v0.1](ux/UX-SYSTEM.md) — specified
- [Interaction Model](ux/INTERACTION-MODEL.md)
- [Accessibility Baseline](ux/ACCESSIBILITY.md)

## Art
- [Visual Identity / Art Direction v0.1](art/ART-DIRECTION.md) — specified
- [Visual Grammar](art/VISUAL-GRAMMAR.md)
- [Asset Policy](art/ASSET-POLICY.md)
- [Procedural VFX System](art/VFX-SYSTEM.md)

## Audio
- [Audio System v0.1](audio/AUDIO-SYSTEM.md) — specified
- [Music System](audio/MUSIC-SYSTEM.md)
- [Haptic Feedback](audio/HAPTICS.md)
- [Audio Asset Policy](audio/ASSET-POLICY.md)

## Formal contracts — specified
- [Domain Model v0.1](contracts/DOMAIN-MODEL.md)
- [State Contracts v0.1](contracts/STATE-CONTRACTS.md)
- [Resolution Contract v0.1](contracts/RESOLUTION-CONTRACT.md)
- [Rule Query Composition Algebra v0.1](contracts/RULE-QUERY-ALGEBRA.md)
- [Bounded Resolution & Cycle Detection v0.1](contracts/BOUNDED-RESOLUTION.md)
- [RNG & Persistence Contract v0.1](contracts/RNG-PERSISTENCE-CONTRACT.md)
- [Content Contracts v0.1](contracts/CONTENT-CONTRACTS.md)
- [RED-First Verification Plan v0.1](contracts/VERIFICATION-PLAN.md)
- [P0 Traceability Audit v0.1](contracts/P0-TRACEABILITY-AUDIT.md) — PASS

Machine-readable contract schemas:
- [`specs/contracts/state.schema.json`](../specs/contracts/state.schema.json)
- [`specs/contracts/resolution.schema.json`](../specs/contracts/resolution.schema.json)
- [`specs/contracts/content.schema.json`](../specs/contracts/content.schema.json)

## Architecture — current gate IN_PROGRESS
- [Architecture Evaluation v0.1](architecture/ARCHITECTURE-EVALUATION.md)
- [ADR-001 — TypeScript + PixiJS 8 + Tauri 2](architecture/ADR-001-TECH-STACK.md) — ACCEPTED
- [RNG Algorithm & Namespace Derivation v0.1](architecture/RNG-SPEC.md) — specified
- [Module & Dependency Boundaries v0.1](architecture/MODULE-BOUNDARIES.md) — specified candidate

Selected stack: TypeScript strict + PixiJS 8/WebGL + semantic DOM/CSS + Tauri 2 shell; Ajv2020 + Vitest + fast-check; Node 24 LTS build line. Production implementation remains locked.

## Control
- [Current State](CURRENT-STATE.md)
- [Open Questions](OPEN-QUESTIONS.md)
- [Traceability](TRACEABILITY.md)

## Machine-readable state
- [`specs/project.yaml`](../specs/project.yaml)

## Remaining Architecture work
Content build/validation pipeline, atomic persistence adapter, concrete RED-first harness/bootstrap, browser/Tauri adapter verification, packaging/QA baseline, boundary enforcement, and implementation-unlock audit.
