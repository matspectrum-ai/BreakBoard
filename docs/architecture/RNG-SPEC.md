# RNG Algorithm & Namespace Derivation v0.1

Status: **SPECIFIED under BB-ARCH-GATE-001**

This document binds BB-RNGC to an exact portable algorithm and golden vectors.

## BB-ARCH-RNG-001 — Algorithm identities
- `rng_algorithm_id = xoshiro128ss-v1`
- `namespace_derivation_version = sha256-generation-v1`
- output primitive: unsigned 32-bit integer
- PRNG: xoshiro128** (star-star), four uint32 state words
- hashing/derivation: SHA-256

The generator is deterministic gameplay infrastructure, **not** cryptographic security infrastructure.

## BB-ARCH-RNG-002 — Canonical RunSeed
Authoritative `run_seed` is exactly 16 bytes serialized as 32 lowercase hexadecimal ASCII characters.

A randomly-created new run may obtain those initial 16 bytes from OS cryptographic entropy **before** GenerationIdentity is constructed. After construction, gameplay uses only the persisted deterministic seed/context.

## BB-ARCH-RNG-003 — Manual seed text normalization
If the user enters arbitrary seed text, convert it to authoritative RunSeed as follows:
1. remove only leading/trailing ASCII whitespace bytes corresponding to TAB U+0009, LF U+000A, CR U+000D and SPACE U+0020;
2. Unicode-normalize the remaining text to NFC;
3. encode UTF-8;
4. hash `"breakboard-seed-text-v1\0" || utf8_text` with SHA-256;
5. RunSeed is the first 16 digest bytes, serialized lowercase hex.

Case is preserved. Internal whitespace is preserved.

## BB-ARCH-RNG-004 — Generation root
Let:
- `S` = 16 RunSeed bytes;
- `C` = UTF-8 `content_version`;
- `R` = UTF-8 `ruleset_version`;
- `U` = 32-byte SHA-256 digest of canonical unlock/eligible-content snapshot JSON.

Then:

`generation_root = SHA256("breakboard-generation-v1\0" || S || 0x00 || C || 0x00 || R || 0x00 || U)`

Generation root is 32 bytes and is part of reproducibility diagnostics.

## BB-ARCH-RNG-005 — Namespace state derivation
For a canonical UTF-8 namespace string `N`:

`D = SHA256("breakboard-rng-namespace-v1\0" || generation_root || 0x00 || UTF8(N))`

The first 16 bytes of `D` become four little-endian uint32 words `s[0..3]`.

If all four words are zero, set `s[0] = 0x9E3779B9` and leave the remaining words zero. No other seed repair is allowed.

A namespace owns its own state/cursor. Drawing from one namespace cannot advance another.

## BB-ARCH-RNG-006 — xoshiro128** nextU32
All operations below are modulo 2^32 unsigned arithmetic.

```text
result = rotl32(s1 * 5, 7) * 9
t = s1 << 9

s2 ^= s0
s3 ^= s1
s1 ^= s2
s0 ^= s3
s2 ^= t
s3 = rotl32(s3, 11)

return result
```

Implementations must mask/coerce to uint32 at each semantic boundary so JavaScript signed bitwise representation cannot change results.

## BB-ARCH-RNG-007 — Uniform bounded integer
For `n` in `1..2^32` and one uniform uint32 source:
1. `limit = floor(2^32 / n) * n` using exact integer arithmetic;
2. draw `x = nextU32()` until `x < limit`;
3. return `x mod n`.

Modulo without rejection is forbidden for gameplay selection because it introduces modulo bias.

Weighted random selection uses nonnegative integer weights and the same bounded-integer primitive over total weight. Floating-point random weights are forbidden for authoritative selection.

## BB-ARCH-RNG-008 — Golden-vector fixture context
Fixture unlock snapshot digest:

`ecf3391a3cb9689d0d59be0d0c7eacfdbc8048f099f971731f074de8b8f565cf`

Fixture content/ruleset versions are both `0.1.0`.

### Vector A
Seed text: `BreakBoard`

Authoritative RunSeed:
`64e634e34fdb27461cd405f0e128b08a`

Generation root:
`593a1c63bff14fb60da7be33ef6b2c5d083aeff188e99553e126b70475415abe`

Namespace: `route.region.1`

Namespace SHA-256:
`73f197af0d0d1a3c33536cc7068a492b33d8c1672e7b9bc664a8fcde9637ab7c`

Initial state words, hex:
`af97f173 3c1a0d0d c76c5333 2b498a06`

First eight `nextU32()` outputs, hex:
`4a25a546 d5e84774 4290ca81 cce30f5b 02ca2ada a4bcc194 0537f59d 3d3ba326`

### Vector B
Same RunSeed/root; namespace: `reward.node-001`

Namespace SHA-256:
`6ac06550532ccf9365b36f559b3283af7b5b08bbe8d03d830c64f0f8f46b01de`

Initial state:
`5065c06a 93cf2c53 556fb365 af83329b`

First eight outputs:
`b5654f79 58e19a38 3a773294 89f322c7 39c7b693 84973326 b772a747 60e00909`

### Vector C — normalization
Input seed text is Greek alpha followed by ` BreakBoard` and two trailing ASCII spaces: `α BreakBoard  `.

Authoritative RunSeed:
`bcd581f7d891c3e1df66253fc9867172`

Generation root:
`5ef383df6ed43e86c6840da146dbc2edee277861b409eac65f970dbfd276b10c`

Namespace: `battle.node-final`

Initial state:
`c3f40245 a79667bb d392291e 19ac28fa`

First eight outputs:
`b81df0b2 9ec1b2a3 4da6cd28 7fd58aed 9e6786dc 862c3f37 9415c02b bc5df0c3`

## BB-ARCH-RNG-009 — Required implementation RED tests
Before the PRNG implementation exists:
- all three vectors above must be encoded as failing golden tests;
- namespace A and B states must differ while sharing the same GenerationIdentity;
- additional draw from reward namespace must not change route namespace output;
- seed-text normalization vector C must match exactly;
- all-zero state repair must have an explicit fixture;
- bounded selection must property-test result range and rejection behavior.

## Reference
xoshiro/xoroshiro reference authors and 32-bit generator guidance: https://prng.di.unimi.it/
