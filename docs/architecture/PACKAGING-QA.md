# Desktop Packaging & Platform QA Baseline v0.1

Status: **SPECIFIED — BB-ARCH-GATE-001 CLOSED**

## Objective
Define the desktop-first build/release verification boundary required by the selected Tauri/Pixi architecture without making OS/WebView behavior gameplay authority.

## BB-ARCH-PKG-001 — Offline application
Base v0.1 is packaged as an offline-capable desktop game. Authoritative content bundles, schemas needed at runtime, core presentation assets and application code ship with the app. Normal play does not require a backend/CDN connection.

Network/store/update integrations are future adapters and may not become hidden gameplay dependencies.

## BB-ARCH-PKG-002 — Renderer baseline
PixiJS 8 **WebGL** is the v0.1 production renderer. WebGPU is not a release requirement. Startup performs a WebGL capability preflight before entering gameplay; unsupported rendering produces an explicit compatibility error rather than silently using a semantically different game implementation.

## BB-ARCH-PKG-003 — Distribution targets
Architecture supports the three desktop families through Tauri 2. Initial packaging targets are:
- Windows x86_64: Tauri NSIS installer as baseline distributable;
- Linux x86_64: AppImage plus Debian package baseline;
- macOS: signed/notarized application bundle distributed via DMG baseline.

Additional formats/stores may be added without changing gameplay architecture. Exact signing identities/store accounts are release-operations data, not game semantics.

## BB-ARCH-PKG-004 — Why actual packaged QA is required
Tauri uses system WebViews rather than one identical bundled browser engine across all desktop OS families. Browser CI therefore catches broad DOM/WebGL behavior but does not replace packaged Tauri smoke tests on actual platform WebViews.

## BB-ARCH-PKG-005 — CI/reference matrix
Baseline verification:

### Every pull request
- Node 24 LTS: typecheck, boundary checks, deterministic build, contract/domain/catalog/application tests;
- Vitest Browser + Playwright: Chromium and WebKit instances;
- content bundle build/validation/reproducibility check.

### Main/release candidate native matrix
- Linux x86_64 reference runner: Ubuntu 24.04 LTS class with Tauri/WebKitGTK dependencies;
- Windows x86_64 reference runner: Windows 11 class with system WebView2;
- macOS reference runner: currently supported stable GitHub/macOS runner using WKWebView;
- `cargo test` for native adapter logic;
- packaged Tauri launch/smoke via supported WebdriverIO/Tauri WebDriver integration where CI permits.

Before public release, at least one additional Linux smoke on a Debian-family environment is required for the AppImage/deb path. Reference runner revisions caused by upstream CI/OS retirement require an architecture-maintenance update but do not change game rules.

## BB-ARCH-PKG-006 — Packaged smoke flow
Each platform release candidate proves at minimum:
1. clean install/first launch;
2. WebGL board initialization;
3. keyboard and pointer basic navigation;
4. create deterministic Run from known seed;
5. execute a canonical Battle action and display result;
6. create stable Run save, terminate app, relaunch and resume exactly;
7. unresolved RewardOffer survives restart without reroll;
8. audio mute and reduced-motion settings do not change outcome;
9. content manifest/version validation succeeds offline;
10. clean exit without save corruption.

## BB-ARCH-PKG-007 — Release integrity
Release pipeline records lockfile, content/ruleset version, RNG algorithm IDs, content bundle digest and source commit. Windows/macOS distributables must use the platform signing/notarization required by the selected distribution route. Linux artifacts publish SHA-256 checksums at minimum.

Auto-update is not required for base v0.1 and cannot be silently introduced as a required launch dependency.

## BB-ARCH-PKG-008 — WebView regressions
A platform-specific presentation regression cannot be patched by changing authoritative rules. Compatibility fixes stay in presentation/adapters. If an OS/WebView cannot satisfy required WebGL/accessibility behavior, support policy changes explicitly rather than creating alternate gameplay semantics.

## Release-readiness distinction
This architecture baseline is sufficient to begin implementation after explicit authorization. Passing the packaging matrix itself is a release-readiness requirement and naturally cannot be demonstrated before the app is implemented.
