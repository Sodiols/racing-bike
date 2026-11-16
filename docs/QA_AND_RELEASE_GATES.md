# Verification, QA and Release Gates

**Status:** Test strategy; no tests or benchmarks have been executed.

## Evidence categories

- **Code health:** typecheck, lint, unit tests, production build.
- **Physics:** fixed-step reproducibility, gears/torque/rpm/energy invariants, handling stability and reference comparisons.
- **Gameplay:** on-screen control, race loop, camera, pause/reset, checkpoints and results.
- **Input:** keyboard rollover, multitouch, shift/clutch interaction, orientation and safe-area handling.
- **Compatibility:** WebGPU path, WebGL fallback, audio activation and resource failures.
- **Visual:** actual game screenshots/video at set checkpoints, not only menu or splash screen.
- **Performance:** hardware named, frame-time distributions, thermal/endurance observations, asset budgets and startup measurements.
- **Security (later):** RLS tests, server authority for currency/rank, rate limits, secret scanning, migration/rollback.

## Device matrix template

| Device ID | Model / OS | Browser and version | Renderer | Graphics preset | Tests | Results |
| --- | --- | --- | --- | --- | --- | --- |
| D1 | TBD desktop | TBD | WebGPU if supported | Auto/High | Keyboard, gameplay, perf | Not tested |
| M1 | TBD budget Android | TBD | fallback as available | Auto/Low | Multitouch, performance | Not tested |
| M2 | TBD mid Android | TBD | as available | Auto/Medium | Touch/thermal | Not tested |
| F1 | TBD fallback browser | TBD | WebGL | Low | startup/recovery | Not tested |

## Minimum test automation by phase

| Phase | Automated examples | Required manual examples |
| --- | --- | --- |
| 01 | startup/state transitions; input action mapping | ride around test track; multitouch and reset |
| 02 | torque/RPM/gear, drag, determinism | manual vs automatic riding, low-speed stability |
| 03 | ordered checkpoint and shortcut rules | legitimate full laps, barrier/seam testing |
| 04 | asset manifests and transform checks | geometry, rig, materials and scale on device |
| 05 | preset application, fallback errors | frame-time/profile per hardware class |
| 06 | audio and camera state transitions | enabled/muted audio, comfort, touch usability |
| 07 | race state and standings | race against AI, restart and result integrity |
| 08 | critical user journey E2E | full desktop and real mobile release playtest |
| 09 | RLS/transactions, concurrency | authorized and unauthorized account paths |
| 10 | session/desync/disconnect/abuse | multiplayer under latency/loss scenarios |

## Severity classification

- **P0:** data corruption, exploit creating assets, unrecoverable crash on target platform, gameplay fundamentally unavailable.
- **P1:** core controls broken, frequent stalls, wrong race/gear outcome, key compatibility failure.
- **P2:** noticeable visual/audio/UI problems with a workaround.
- **P3:** polish or minor documentation issues.

R2 public release candidate cannot ship with known P0 issues or unresolved P1 issues affecting the defined core journey. Other issues need written acceptance and tracking.

## Milestone completion template

```text
Milestone: PXX / Task:
Commit or review reference:
Agent implementer / independent reviewer:
Requirements met:
Tests run and exact result:
Manual browser demo and device model:
Metrics with units and capture method:
Known issues / license uncertainties:
Rollback:
Owner acceptance date:
Status: NOT STARTED | IN REVIEW | ACCEPTED | BLOCKED
```

Never substitute claims like 'all tests pass' without actual command outputs or logs. No device was tested as part of the planning package.
