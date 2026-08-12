# Design Principles

## BB-PRN-001 — Complexity by composition
Prefer a small vocabulary of interoperable mechanics over many isolated mechanics.

## BB-PRN-002 — Agency over resolution RNG
Randomness may shape offers/generated content. Core movement, capture, and effect resolution must not depend on hidden arbitrary outcomes.

## BB-PRN-003 — Break rules, not the engine
Mutations may alter gameplay rules but cannot violate deterministic resolution, valid state, or termination guarantees.

## BB-PRN-004 — Mechanics are content
Boss and encounter identity should preferentially emerge from mechanics rather than expensive bespoke visual production.

## BB-PRN-005 — Horizontal meta-progression
Persistent progression should primarily unlock possibilities rather than permanent raw-stat advantages.

## BB-PRN-006 — Explicit interactions
Compatibility, requirements, exclusions, overrides, triggers, and ordering must be specifiable and testable.

## BB-PRN-007 — No external game knowledge required
The repository defines every BreakBoard rule; implementers never need chess knowledge.
