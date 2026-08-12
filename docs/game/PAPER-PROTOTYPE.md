# Paper Prototype v0.1

Status: completed design exploration. `CANONICAL-RULES.md` supersedes it where they differ.

## Hypothesis
A compact deterministic board game becomes a roguelite through three composable mutation domains: pieces, board, and global rules.

## Baseline
Two sides; turn-based; perfect information; candidate 6×6 board; compact armies; one primary action per turn; capture-oriented objective; mutations primarily acquired between battles.

## Mutation experiments
**Piece:** Vampire, Explosive, Blink, Armored, Berserker, Ghost, Evolution, Chain.

**Board:** destroyed tile, Wall, Portal, Lava, Ice, Conveyor, Fragile.

**Rule:** Afterlife, capture destroys tile, lateral Pawn movement, multiple Cores, center restrictions, last-stand mutation.

These are examples, not production content.

## Representative interaction
A rule that destroys the capture tile changes topology after every capture, breaking movement lanes, rerouting attackers, isolating regions, and interacting with fragile tiles or explosive pieces. This strongly validates the BreakBoard thesis.

## Findings
1. Mutation randomness should primarily determine offers; players choose.
2. Rule mutation needs deterministic conflict/precedence semantics.
3. Board destruction creates strong mechanical/visual identity.
4. Unlimited persistent board damage risks unusable late-run boards.
5. Immutable kernel rules are required beneath mutable gameplay rules.
6. Bosses can be expressed mechanically rather than with expensive bespoke assets.
7. Edge cases must be contract-defined, including no moves, simultaneous Core loss, resurrection to invalid tiles, portal destruction, recursive effects, and disconnected topology.

## Outcome
Advanced to Canonical Game Rules v0.1. Mutation semantics are the next design gate.
