# Visual Identity & Procedural Art System v0.1

Status: **SPECIFIED — BB-ART-GATE-001 CLOSED**

## Direction
Internal production codename: **Broken Geometry**.

BreakBoard's visual identity combines precise geometric forms, dark structural materials, luminous fractures/rule overlays, iconic silhouettes, and procedural motion. It must be recognizable without relying on high-volume bespoke illustration or frame-by-frame animation.

## BB-ART-001 — Principles
- geometry over detail;
- silhouette first;
- board is primary;
- mutation/rule state is layered over stable base forms;
- destruction/fracture is part of identity;
- procedural/reusable motion over bespoke animation;
- readable at small scale;
- critical state never color-only;
- gameplay cannot depend on decorative AI-generated assets.

## BB-ART-002 — Visual layers
Three conceptual layers:
1. Structure: board, tiles, pieces, walls, topology.
2. Rules: mutations, board features, Core/ownership, global laws.
3. Break: capture, destruction, Collapse, reaction chains, fractures/particles.

## BB-ART-003 — Piece silhouettes
The five base archetypes require distinct silhouettes in monochrome at small size:
- Pawn: smallest/simple forward-weighted form;
- Tower: heavy vertical/pillar silhouette;
- Leaper: asymmetric/inclined silhouette communicating displacement;
- Seer: diamond/lens/eye-like silhouette;
- Core archetype: heavier central/nucleus form.

CoreClassification is independent of archetype. Any Core-classified piece receives a persistent Core Halo/status marker layered around the piece.

## BB-ART-004 — Ownership
Player and Enemy are differentiated redundantly using at least color family plus pattern/shape/orientation treatment. Initial art baseline: Player uses a cool accent family and cleaner/continuous pattern; Enemy uses a warm accent family and segmented/inverted detail. Final numeric colors remain theme tokens subject to accessibility tests.

## BB-ART-005 — Board grammar
A tile has independent visual layers: Base, Feature, State, Interaction. Selection/legal/danger overlays may not erase Board Feature identity. Destroyed tiles are visually absent from topology rather than rendered as merely dark/empty tiles.

## BB-ART-006 — UI visual language
Dark neutral field, geometric panels, thin structural borders, sharp/simple geometry, controlled whitespace, strong typography hierarchy, restrained gradients/glow, and a visually dominant board. UX contracts take precedence over decorative styling.

## BB-ART-007 — Rarity/category encoding
Piece Mutation, Rule Mutation, and Board Feature glyphs use different container shapes. COMMON/RARE distinction must use geometry/border treatment in addition to any color. Rule cards use a distinct law/hexagonal visual grammar so they read as changes to global laws rather than normal buffs.

## BB-ART-008 — Boss identity
Bosses require emblem, title treatment, encounter rule glyph(s), board/theme parameter variation, and signature procedural VFX. Bespoke character models or gameplay-critical key art are not required. Optional AI-generated splash/key art is decorative only.

## BB-ART-009 — Region variation
Regions reuse the same core board/piece system and vary through environment/background, surface parameters, ambient effects, and secondary accents. Region I is cleaner, Region II more interconnected/energized, Region III more fractured/unstable. Separate full tilesets are not required.

## BB-ART-010 — Cosmetic integrity
Cosmetics may replace piece material/geometry skin, board surface, environment, particle profile, and UI frame style, but must preserve archetype silhouette, ownership, Core state, mutation count, Board Feature identity, Collapse warnings, and legal-target readability.

## BB-ART-011 — Identity test
A battle screenshot with logo/text removed should remain recognizably BreakBoard through geometric silhouettes, segmented dark board topology, Core halos, luminous rule/feature glyphs, and visible fracture/destruction language.

## Deferred
Exact RGB/OKLCH values, font family, glow strengths, border thicknesses, particle counts, final shaders/materials, illustration style for marketing, and renderer-specific techniques remain future token/polish/technology work.
