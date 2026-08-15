# Haptic Feedback v0.1

Status: specified under `BB-AUDIO-GATE-001`.

## BB-HAP-001 — Intent abstraction
Domain and game systems never request platform-specific vibration duration/amplitude. Presentation emits semantic haptic intents and adapters decide whether/how to render them.

Baseline intents: `SELECT`, `CONFIRM`, `INVALID`, `MOVE_COMMIT`, `CAPTURE`, `MUTATION_TRIGGER`, `RULE_TRIGGER`, `CORE_EVENT`, `COLLAPSE_WARNING`, `COLLAPSE_STEP`, `VICTORY`, `DEFEAT`.

## BB-HAP-002 — Optional channel
Haptics are optional, disableable, and presentation-only. A platform with no haptic capability may ignore all intents with no gameplay or UX-state change.

## BB-HAP-003 — User control
Baseline preference levels are `OFF`, `LOW`, and `NORMAL`; exact platform amplitudes/durations are deferred to platform/technology contracts.

## BB-HAP-004 — Platform scope
Desktop launch baseline does not require haptic hardware. The abstraction exists to support future controller/touch adapters without coupling canonical game logic to Android, iOS, gamepad, or any engine API.
