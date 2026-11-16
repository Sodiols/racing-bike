# Phase 01 Execution Plan | First Rideable Prototype

**Status:** PROPOSED, NOT AUTHORIZED. Execute only after the owner approves Phase 00 and this plan.
**Source:** [`milestones/PHASE_01.md`](../milestones/PHASE_01.md). The task IDs below subdivide its P01-01…P01-06 without changing its scope, non-goals or exit gate.
**Assumes:** ADR-001/002/003 approved as proposed. If an ADR is rejected, revise the affected tasks first.

## Goal and boundaries

A player opens the page in a real browser, rides a temporary bike around a simple closed course with keyboard or simultaneous touch controls, sees an honest simulation-derived speed, and can pause and reset. On desktop it uses WebGPU where available, with WebGL2 fallback.

**Out of scope for all of Phase 01:** gearbox/clutch/RPM, calibrated physics, GLB/production models, checkpoints and lap timing (Phase 03), AI, audio, garage, accounts, backend, deployment, graphics presets beyond a DPR cap.

**Rules for every task:** one implementer, one reviewer, a branch named `feature/p01-<task>`; no new dependency outside the list in P01-01A without approval; record real command output in `docs/PROGRESS.md`; simplified physics is labeled in the UI and the code.

## Execution order

```text
P01-00 → P01-01A → P01-03A ─┐
                 → P01-01B ─┼→ P01-02A → P01-02B → P01-03B → P01-05A → P01-05B → P01-06A → P01-06B
                 → P01-04A ─┘                                ↑
                              P01-04A → P01-04B ─────────────┘
```

P01-03A (pure clock) and P01-04A (pure input) have no Babylon dependency. Once the scaffold exists they may run on separate worktrees in parallel with P01-01B, if the owner wants parallelism.

---

### P01-00 Repository prerequisites (owner-gated)
- **Objective:** Make the branch/review workflow possible.
- **Dependencies:** Phase 00 approval.
- **Files:** `.gitignore` (new), repository root.
- **Boundaries:** `git init`, ignore rules, one baseline commit of the existing planning docs. No remote, no push. Resolve the `.agents/skills/bufatechno-webgamedev` clone first (owner decision).
- **Acceptance:** `git status` is clean on `main`; no embedded repository warning.
- **Tests:** `git status`, `git log --oneline -1`.
- **Deliverables:** baseline commit hash recorded in PROGRESS.

### P01-01A Toolchain scaffold
- **Objective:** Minimal Vite + React + TypeScript + Tailwind app with lint and test tooling, and no game code.
- **Dependencies:** P01-00; **owner approval of the dependency list:** `@babylonjs/core`, `@babylonjs/havok`, `react`, `react-dom`, `zustand`, `tailwindcss`, `@tailwindcss/vite`, `vite`, `@vitejs/plugin-react`, `typescript`, `vitest`, `@playwright/test` (using the installed Chrome channel, no browser download), `eslint`, `typescript-eslint`. Exact pins chosen at install.
- **Files:** `package.json`, `package-lock.json`, `tsconfig*.json`, `vite.config.ts`, `eslint.config.js`, `index.html`, `src/main.tsx`, `src/app/App.tsx`, `src/index.css`.
- **Boundaries:** no Babylon usage yet; no UI beyond a placeholder root; TS `strict: true`.
- **Acceptance:** `npm run dev` serves the page; typecheck, lint, test and build all pass; TypeScript version decision recorded (ADR-001 note).
- **Tests:** `npm run typecheck`, `npm run lint`, `npm test` (one trivial test proving the runner works), `npm run build`.
- **Deliverables:** scripts `dev/build/preview/typecheck/lint/test/test:e2e`; baseline bundle size logged.

### P01-01B Engine bootstrap, renderer selection and lifecycle
- **Objective:** Render an empty lit Babylon scene with WebGPU→WebGL2 selection, error overlay, resize and lifecycle handling.
- **Dependencies:** P01-01A.
- **Files:** `src/game/runtime/GameRuntime.ts`, `src/game/rendering/createEngine.ts`, `src/game/telemetry/Diagnostics.ts`, `src/app/GameCanvas.tsx`, `src/app/ErrorOverlay.tsx`.
- **Boundaries:** deep imports only; no physics; diagnostics overlay is dev-visible (FPS, frame ms, backend, DPR).
- **Acceptance:** WebGPU is chosen on this laptop's Chrome/Edge; `?renderer=webgl` forces WebGL2; a forced init failure shows the overlay, not a blank page; resize keeps aspect ratio; DPR capped (default ≤ 2, provisional); StrictMode double-mount creates exactly one engine; `visibilitychange` pauses the runtime; context/device loss is surfaced.
- **Tests:** unit test for the backend-selection decision function (inputs: gpu present/absent, init success/failure/timeout, override); manual check in Chrome and Edge for both backends.
- **Deliverables:** screenshots of both backends with the diagnostics overlay.

### P01-02A Havok integration and stepping spike (ADR-002 gate)
- **Objective:** Load Havok WASM locally through Vite and prove the fixed-step coupling chosen in ADR-002.
- **Dependencies:** P01-01B, P01-03A.
- **Files:** `src/game/physics/PhysicsWorld.ts`, `src/game/physics/havokStepAdapter.ts`, a test or dev scene.
- **Boundaries:** Havok WASM served from the app bundle, no CDN; a single falling and sliding test body on a ground plane; no bike.
- **Acceptance:** the body's simulated trajectory after N steps is the same (within a stated tolerance) when frames are paced at 30, 60 and 144 FPS; the step count equals the clock's step count; pause stops stepping; Option 1 vs 2 is decided and recorded in ADR-002.
- **Tests:** browser test (Playwright, Chrome channel) that drives frame pacing and compares body state; WASM load-failure path shows the error overlay.
- **Deliverables:** evidence table (FPS, steps, final position delta) in PROGRESS.

### P01-02B Test circuit and colliders
- **Objective:** A short closed-loop course with clear visual boundaries and a reliable ground collider.
- **Dependencies:** P01-02A.
- **Files:** `src/game/tracks/testCircuit.ts` (centerline data plus generated road ribbon), `src/game/tracks/buildTrack.ts`.
- **Boundaries:** procedurally generated from a centerline in code (no downloaded assets). Provisional values: roughly 1–1.5 km, 10–12 m wide, flat, with a mix of straights and varied-radius corners, start/finish line, edge markings, low barriers or run-off. Simple primitive colliders or a low-poly road mesh; no checkpoints or lap logic.
- **Acceptance:** no fall-through or seams over a full traversal at speed; spawn point on track; track visible in both backends.
- **Tests:** unit test that the centerline is closed and width is constant; a test body raycast along the centerline hits ground everywhere.
- **Deliverables:** top-down screenshot plus track dimensions recorded.

### P01-03A Fixed-step simulation clock (pure)
- **Objective:** Engine-independent accumulator clock per ADR-002.
- **Dependencies:** P01-01A.
- **Files:** `src/game/runtime/SimClock.ts`, `src/shared/units.ts` (SI ↔ km/h helpers), tests.
- **Boundaries:** no Babylon or DOM imports.
- **Acceptance:** fixed dt; max steps per frame with dropped-time counter; interpolation alpha in [0,1); pause and resume do not create a catch-up burst; sim time = steps × dt.
- **Tests:** Vitest: frame-delta sweeps (30/60/144/irregular) produce identical step counts for identical total time; huge delta is capped; unit conversions round-trip.
- **Deliverables:** test output in PROGRESS.

### P01-03B Provisional bike proxy and simplified model
- **Objective:** A rideable temporary bike whose speed comes from a simple SI-unit model.
- **Dependencies:** P01-02B, P01-04A, `motorcycle-physics` skill (units and labeling rules).
- **Files:** `src/game/bikes/ProvisionalBikeModel.ts` (pure), `src/game/bikes/provisionalBikeConfig.ts`, `src/game/bikes/BikeEntity.ts` (Havok/Babylon adapter), tests.
- **Boundaries:** primitive geometry (body, two wheels, rider block). Model: power-limited drive force, quadratic drag, rolling resistance, front and rear brake forces, speed-limited steering → yaw/curvature, visual lean only (`atan(v²/(R g))`), body kept upright (assist). Parameters labeled *fictional provisional*. No gears, RPM or tire slip.
- **Acceptance:** throttle accelerates, brakes decelerate to a stop without reversing, steering turns at all speeds without spinning out, top speed emerges from drag (no hard speed clamp); collisions with barriers do not launch or tunnel the bike; the model has no Babylon imports.
- **Tests:** Vitest: zero throttle on flat ground decelerates; drag grows with v²; brakes never add energy; same inputs give the same trace; no NaN after 10 min of simulated random inputs.
- **Deliverables:** speed/time trace for a full-throttle run and a braking run.

### P01-04A Input actions and keyboard adapter
- **Objective:** Shared action abstraction (per ARCHITECTURE input table) with a keyboard adapter.
- **Dependencies:** P01-01A.
- **Files:** `src/game/input/actions.ts`, `src/game/input/InputState.ts`, `src/game/input/keyboardAdapter.ts`, tests.
- **Boundaries:** `KeyboardEvent.code`; throttle/brake/steer ramp smoothing is configurable; all actions released on `blur`/`visibilitychange`; Phase 01 binds throttle, front brake, rear brake, steer, pause, reset (clutch and shift reserved, not bound).
- **Acceptance:** simultaneous keys combine correctly (throttle + steer + brake); no stuck key after alt-tab.
- **Tests:** Vitest on synthetic key events: mapping, combination, ramping, blur release.
- **Deliverables:** binding table in the HUD help or README.

### P01-04B Multitouch adapter and touch controls
- **Objective:** Landscape touch controls feeding the same actions.
- **Dependencies:** P01-04A, P01-05B layout shell (can stub).
- **Files:** `src/game/input/touchAdapter.ts`, `src/app/TouchControls.tsx`.
- **Boundaries:** Pointer Events, per-`pointerId` tracking, `setPointerCapture`, `touch-action:none`, no scroll, zoom, callout or context menu; left side steer left/right, right side throttle and brake (provisional layout); portrait prompt; safe-area insets.
- **Acceptance:** throttle + steer held simultaneously works; releasing one finger does not cancel the other; controls shown only on touch-capable devices or by toggle.
- **Tests:** Vitest with synthetic pointer events (two concurrent pointer IDs); Playwright touch emulation smoke; **real device check in P01-06B.**
- **Deliverables:** screenshot of the touch layout in landscape and of the portrait prompt.

### P01-05A Chase camera
- **Objective:** A stable, frame-rate-independent chase camera.
- **Dependencies:** P01-03B.
- **Files:** `src/game/rendering/ChaseCamera.ts`.
- **Boundaries:** follows the interpolated visual transform; exponential smoothing with time constant (not per-frame lerp factors); no camera cycling yet.
- **Acceptance:** no jitter at 30, 60 or 144 FPS; no clipping into the ground on braking; snaps correctly on reset.
- **Tests:** unit test that the smoothing result is frame-rate independent for the same elapsed time.
- **Deliverables:** short capture.

### P01-05B HUD, pause and reset flow
- **Objective:** A minimal honest HUD and game-flow states.
- **Dependencies:** P01-03B, P01-04A.
- **Files:** `src/app/Hud.tsx`, `src/app/PauseMenu.tsx`, `src/app/store.ts` (Zustand), `src/game/runtime/GameFlow.ts`.
- **Boundaries:** speed in km/h derived from the simulation snapshot and published to the store at ~10–20 Hz (not per step); a "simplified physics" label; states loading → riding ↔ paused, reset → respawn at start; Esc/R and on-screen buttons.
- **Acceptance:** displayed speed matches model speed (test hook); pause freezes simulation time; reset restores the spawn state with zero velocity; no React re-render per physics step.
- **Tests:** Vitest for the GameFlow state machine and the store throttle; Playwright: pause and reset via keyboard.
- **Deliverables:** HUD screenshot in both backends.

### P01-06A Automated smoke tests
- **Objective:** A repeatable browser smoke suite.
- **Dependencies:** P01-05B, P01-04B.
- **Files:** `tests/e2e/*.spec.ts`, `playwright.config.ts`.
- **Boundaries:** installed Chrome channel; WebGPU and forced WebGL runs; a dev-only test hook exposes simulation speed (stripped from production builds).
- **Acceptance:** boot with no console errors; holding throttle increases simulated speed; brake reduces it; pause and reset work; both backends pass.
- **Tests:** `npm run test:e2e`, `npm test`, `npm run build`.
- **Deliverables:** test output in PROGRESS.

### P01-06B Manual playtest, mobile check and performance baseline
- **Objective:** Exit-gate evidence on real hardware.
- **Dependencies:** P01-06A; **owner provides at least one Android phone** and approves the access method (`adb reverse`, which needs Android platform-tools installed, or a local HTTPS dev certificate).
- **Files:** `docs/PROGRESS.md` only (evidence).
- **Boundaries:** no deployment; local or LAN only.
- **Acceptance:** one complete human-controlled course traversal on desktop (D1 = this laptop, Chrome 157, WebGPU) and on the phone (backend recorded); simultaneous touch throttle + steer confirmed; frame-time median and p95 over a 2-minute ride recorded per device; no uncaught console errors; limitations listed.
- **Tests:** structured manual checklist (keyboard rollover, multitouch, pause/reset, resize/orientation, tab switch) plus the diagnostics overlay log.
- **Deliverables:** device matrix rows filled, captures, a known-issues list, and owner acceptance requested. **Not self-approved.**

## Phase 01 exit gate (unchanged from milestone)

One complete human-controlled track traversal on desktop and on available real mobile hardware, with no blocked core action and known shortcomings tracked. The owner approves acceptance.
