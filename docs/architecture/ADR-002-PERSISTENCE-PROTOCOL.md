# ADR-002 — Stable Run Persistence Protocol v0.1

Status: **ACCEPTED — BB-ARCH-GATE-001 CLOSED**
Date: 2026-08-15

## Decision
Use an immutable-generation save protocol implemented by the Tauri/Rust persistence adapter. Do not overwrite an authoritative save file in place and do not depend on a single platform-specific claim that rename is always atomic.

This protocol implements the P0 stable Run save contract. Mid-battle serialization/recovery remains outside this v0.1 implementation-readiness requirement and retains its separately recorded pre-release obligation.

## BB-ARCH-PER-001 — Storage scope
Authoritative gameplay saves live under the application's native local-data directory in a BreakBoard-controlled subtree. Presentation/settings data is stored separately.

Conceptual layout:

```text
<app-local-data>/breakboard/saves/
  <profile-storage-key>/
    <slot-storage-key>/
      save.<sequence>.json
      .staging.<unique>.tmp
```

File-system paths are never constructed from raw user/content text. Storage keys are derived from semantic IDs using a versioned SHA-256 key derivation or another explicitly equivalent safe encoding.

## BB-ARCH-PER-002 — File envelope
A finalized generation file contains:
- storage format identifier `breakboard-save-v1`;
- decimal-string monotonic `save_sequence`;
- SHA-256 digest of canonical persisted-envelope bytes;
- the persisted envelope required by `RNG-PERSISTENCE-CONTRACT.md` (schema/content/ruleset/RNG versions plus payload).

The digest covers the canonical envelope, not the digest field itself.

## BB-ARCH-PER-003 — Single writer
The native adapter is the only gameplay-save writer. Writes are serialized through a process-level queue/mutex and an exclusive profile/slot writer lock. If exclusive ownership cannot be established, the save operation fails explicitly with a stable persistence error; competing writers never race on sequence allocation.

## BB-ARCH-PER-004 — Commit protocol
For a stable Run checkpoint:
1. application validates that the state is an allowed stable checkpoint;
2. next sequence is allocated from the highest valid finalized generation;
3. adapter encodes the new envelope into canonical bytes and digest;
4. write a uniquely named staging file in the same save directory using create-new semantics;
5. write all bytes, flush, and request native file-data synchronization (`sync_all`/platform equivalent);
6. close and reopen staging; verify length, parse, digest and envelope compatibility;
7. rename staging to a unique finalized `save.<sequence>.json` path that does not already exist;
8. reopen the finalized generation and repeat integrity validation;
9. only after the new generation is valid, garbage-collect older finalized generations while retaining at least the two newest valid compatible generations;
10. remove abandoned staging files opportunistically.

The old generation is never overwritten as part of committing the new one. If power/process failure occurs before a new finalized generation validates, the previous finalized generation remains available.

Where the OS/filesystem supports syncing the containing directory, the adapter should do so after finalization; failure to obtain that extra durability must not cause deletion of the prior valid generation.

## BB-ARCH-PER-005 — Load/recovery
Load does not trust a mutable `current` pointer. It scans finalized generation candidates for the requested slot, orders sequences numerically, and validates newest-to-oldest:
1. storage format/version;
2. outer digest;
3. persisted envelope parse/version recognition;
4. explicit migrations when needed;
5. JSON Schema validation;
6. semantic/cross-reference/content compatibility invariants.

The newest valid compatible generation becomes authoritative. Invalid/truncated generations are never partially accepted.

If the newest generation is invalid but an older retained generation is valid, load succeeds with an explicit `RECOVERED_PREVIOUS_GENERATION` diagnostic so presentation can inform the player. If none is valid, load fails explicitly.

## BB-ARCH-PER-006 — No reroll through recovery
Recovered state is exactly the validated persisted state. Route graphs, RNG cursors, RewardOffers and selected candidates are not regenerated from seed to repair a save. A migration may transform schema shape only under the existing migration contract; it cannot invent decisions.

## BB-ARCH-PER-007 — Browser development adapter
Browser-only development/tests may use an in-memory or test filesystem `PersistencePort` implementation. It must preserve the same application-level semantics (stable checkpoints, versions, no reroll), but it is not release persistence authority.

## BB-ARCH-PER-008 — Tauri boundary
JavaScript/DOM/Pixi code does not directly manipulate authoritative save paths/files. The application calls `PersistencePort`; the Tauri adapter invokes a narrow Rust command surface responsible for local-data path resolution, locking, generation-file I/O and recovery.

Rust persistence code may validate storage integrity but cannot decide game rules, content eligibility or run outcomes.

## Required RED tests
- interrupted staging write leaves previous generation authoritative;
- corrupted newest generation falls back to prior valid generation with recovery diagnostic;
- corrupted digest never becomes authoritative;
- unresolved RewardOffer survives save/load exactly;
- RNG cursors survive round trip exactly;
- concurrent writer attempt fails rather than racing;
- unsupported future version fails explicitly;
- migration fixture is deterministic;
- cleanup never deletes the only valid generation;
- presentation/settings corruption cannot mutate gameplay save state.
