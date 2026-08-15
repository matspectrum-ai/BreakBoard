# Audio Asset Policy v0.1

Status: specified under `BB-AUDIO-GATE-001`.

## BB-AAS-001 — Production priority
Preferred order: procedural synthesis; reusable designed SFX; layered/parameterized samples; generated audio with known provenance; bespoke recording/composition.

This is a production preference, not a requirement that every sound be synthesized.

## BB-AAS-002 — Reuse over content-ID assets
Gameplay content should map to semantic families and parameters. Do not require one sound asset for every Mutation, Rule, encounter, or Boss unless a later specification justifies it.

## BB-AAS-003 — Generated audio
Generative tools are acceptable for exploration, ambience, non-critical textures, marketing, and potentially final assets only when rights, license, provenance, and consistency are documented.

## BB-AAS-004 — Provenance
Every production audio/music asset requires at least stable identity, source, license/rights status, version, and generation/tool provenance when applicable. Unknown-origin assets are forbidden in production.

## BB-AAS-005 — Voice acting
Voice acting is not required for v0.1. Boss identity is delivered through title/emblem/mechanics/music motif/VFX. This avoids a hard dependency on VO production and localization.

## Baseline asset-budget principle
A compact family-based library is preferred: roughly single-digit/low-double-digit UI and core-battle families, eight Board Feature families, eight Mutation/Rule semantic families, a small critical-result set, reusable region palettes, and three Boss motifs. Exact file counts/formats remain production parameters.
