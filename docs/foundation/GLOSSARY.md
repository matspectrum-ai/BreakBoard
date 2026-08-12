# Glossary

- **Battle** — one tactical board encounter.
- **Run** — sequence of encounters and mutation choices ending in success or failure.
- **Board** — tile topology on which battle occurs.
- **Tile** — board location. A destroyed tile is absent from navigable topology, not merely empty.
- **Piece** — movable battle entity.
- **Core** — objective piece. A side loses when no active Core remains after stabilized resolution, subject to canonical rules.
- **Mutation** — gameplay modifier belonging to Piece, Board, or Rule category.
- **Kernel Rule** — non-mutable invariant preserving deterministic and valid gameplay.
- **Primary Action** — normal player-selected action for a turn.
- **Trigger** — condition scheduling an effect.
- **Effect** — deterministic state transition from an action, rule, or mutation.
- **Collapse Phase** — anti-stall phase that forces battle termination; exact parameters remain open.
- **Canonical** — authoritative approved behavior until deliberately revised.
- **Hypothesis** — design candidate requiring validation.
- **Balance Parameter** — configurable value expected to change without changing the underlying design contract.
