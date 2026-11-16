# Phase 00 Technical Assessment

**Task:** P00-02 / P00-06 | **Date:** 2026-11-17 | **Agent:** Claude Code | **Status:** Proposed; awaiting Codex review and owner approval
**Inputs:** existing planning docs, [`ENVIRONMENT_AUDIT.md`](ENVIRONMENT_AUDIT.md), and official Babylon.js docs and source read on 2026-11-17 (listed in §6).
**Verdict:** The proposed stack is **suitable and is preserved**. No competing engine is recommended. The findings below are gaps and decisions that the existing docs leave open. ADRs: [`ADR_001`](../decisions/ADR_001_core_runtime_stack.md), [`ADR_002`](../decisions/ADR_002_simulation_clock_and_havok_stepping.md), [`ADR_003`](../decisions/ADR_003_motorcycle_dynamics_approach.md).

## 1. Architecture evaluation

| Area | Assessment | Weakness / missing decision | Recommendation |
| --- | --- | --- | --- |
| 1. Rendering performance | Babylon 9.x supports WebGPU and WebGL2 side by side; this laptop exposes a hardware WebGPU adapter | `@babylonjs/core` is 74 MB unpacked. Barrel imports will bloat the bundle. WebGPU pipeline creation can hitch the first time a material is seen (Babylon lists async pipeline creation as not done) | Use deep ES module imports only. Measure bundle size from P01-01A. Warm up materials behind the loading screen. Track draw calls and frame time from the first scene |
| 2. Motorcycle physics architecture | Havok handles contacts and collisions; custom dynamics planned | Babylon docs and source contain no vehicle or wheel helper. A free rigid body on two contact points will not balance | ADR-003: one chassis body, ray/shape-cast wheel contacts, custom tire/drivetrain forces, roll as a controlled state |
| 3. Fixed timestep | Planned at 1/120 s with an accumulator | **Babylon Physics V2 steps inside the render loop.** A fixed step needs `setSubTimeStep` (scene accumulator) and the `onBeforePhysicsObservable` hook. There is no public manual-step API, and `_step` clamps deltas above 0.1 s. `useDeltaForWorldStep=false` makes the world step a fixed amount **per frame**, so sim speed would depend on frame rate. Babylon does not interpolate body transforms | ADR-002: GameRuntime owns the simulation clock. P01-02A proves the Havok coupling with a frame-rate sweep before more code depends on it |
| 4. Keyboard and multitouch | Action abstraction already planned | Missing details: stuck keys on window blur, layout-independent keys, browser gestures stealing touches | Use `KeyboardEvent.code`; release all actions on `blur`/`visibilitychange`; Pointer Events with `touch-action:none`, per-`pointerId` tracking and `setPointerCapture`; suppress context menu, callout and double-tap zoom |
| 5. Mobile compatibility | WebGL2 is near-universal; WebGPU is not Baseline | Havok needs WASM SIMD (docs: unsupported below iOS 16.4). iPhone Safari has no Fullscreen API and no orientation lock. Tab memory limits are tight. A LAN-IP dev server is not a secure context, so phones fall back to WebGL | Minimum target: iOS 16.4+ and recent Android Chrome (owner to confirm). Cap device pixel ratio by default. Use a portrait prompt instead of an orientation lock. Test phones via `adb reverse` or HTTPS |
| 6. Asset loading and streaming | GLB/KTX2 planned (Phase 04) | Babylon's KTX2/Draco decoders and the Havok WASM load from Babylon's CDN unless configured, and the Havok docs say not to use the CDN in production | Self-host the Havok WASM through Vite from P01-02A and decoders from Phase 04. No CDN runtime dependencies. No GLB in Phase 01 |
| 7. Quality scaling | Five presets planned | No mechanism chosen | Primary lever is the hardware scaling level (render resolution) with frame-time hysteresis. Shadows and post-processing are secondary. The simulation is never touched. Phase 01 records the baseline only |
| 8. Application lifecycle | Failure list exists in ARCHITECTURE.md | Two unhandled traps: **React 19 StrictMode double-mounts effects** in dev, which can create two engines, and Vite HMR re-executes modules. Device/context loss differs between WebGPU and WebGL | Make the runtime a plain class with idempotent `start()`/`dispose()` mounted from one React effect. Wire `visibilitychange` to pause and reset the accumulator. Handle context/device loss and recover with a reload prompt in Phase 01 |
| 9. Future multiplayer | Deferred to Phase 10, server-authoritative | Lockstep determinism across browsers and GPUs is not realistic with float WASM physics. The server will need the simulation without a browser | Keep the dynamics core free of Babylon and DOM imports (pure TS, unit-testable in Node). Inputs are already command-based. This costs nothing now and keeps the server option open |
| 10. Long-term maintainability | Layering is sound | TypeScript 7 is newly released (tooling risk). No lint, test or format standards are written down. No git repository yet | TS `strict`, ESLint flat config, Vitest for pure modules, Playwright for browser smoke using the installed Chrome channel (no browser download). Pin exact versions. Commit the lockfile |

**Tailwind / Zustand / React:** suitable for menus and HUD. HUD values go through a Zustand store written at about 10–20 Hz from simulation snapshots, or through transient subscriptions. They are never written per physics step. **Vite** is suitable; SSR is not needed. **Supabase** stays deferred (D08).

## 2. Motorcycle simulation plan review

`PHYSICS_AND_CALIBRATION.md` is sound in its intent: SI units, calibration hierarchy, honest labeling, deterministic tests. The architecture can support every requested behavior (torque curves, acceleration, manual/automatic gears, clutch, separate brakes, traction, steering, lean, suspension, drag, collision, calibration) **if** ADR-002 and ADR-003 are adopted. Gaps and unrealistic assumptions:

1. **Load transfer is missing from the core path.** Hard acceleration in low gears is usually **wheelie-limited**, and hard front braking is **stoppie-limited** (about `g * b / h`), not grip-limited. Without pitch and load transfer, separate front and rear brakes feel identical, and 0–100 km/h will come out too fast. Move longitudinal load transfer from "optional refinement" into step 3.
2. **`F_drive = min(T_wheel / r, F_traction)` loses wheelspin.** When torque exceeds grip, the rear wheel must spin up as its own rotational state, and engine RPM must follow the wheel, not road speed. Model wheel angular velocity per wheel from step 1.
3. **Combined slip.** Braking or driving while leaned needs a friction-circle limit. Planned, but should be explicit in step 3–4 tests.
4. **Tire forces are stiff at low speed.** Slip-ratio formulas divide by speed and oscillate at 120 Hz near standstill. Use a relaxation length or low-speed blending. The sim may need internal substeps, measured in Phase 02.
5. **Steering from digital or touch input.** A rider countersteers, but keyboard or touch cannot express countersteer torque. Input should set a **target lean / target curvature**, bounded by speed and grip, and a controller produces the steering. This is a deliberate assist and must be labeled.
6. **`tan(lean) = v²/(r g)`** ignores tire width and rider offset. It is acceptable as a sanity bound; real lean is a few degrees more.
7. **Effective radius, C_dA and C_rr** must be calibration parameters with stated ranges. Rider tuck changes C_dA materially.
8. **Determinism scope.** Determinism tests should run on the pure dynamics core with fixed inputs in Node. Havok contact results are not guaranteed bit-identical across machines or browsers.
9. **Crash and fall behavior** is unspecified. Phase 01/02 should use a reset/respawn policy; full crash physics is out of MVP scope.

**Derived from simulation:** acceleration from the torque curve, gearing, drag and rolling resistance; top speed; RPM and gear speeds; engine braking; braking deceleration (grip- and stoppie-limited); wheelspin and lockup; lean angle at a given speed and radius; suspension pitch and squat; collision response.
**Deliberate assists (must be labeled):** roll stabilization and balance; steering-to-lean controller (automatic countersteer); auto clutch and anti-stall; automatic shifting; optional ABS, traction control and wheelie control; touch input smoothing; speed-sensitive steering limits; crash tolerance and respawn.

## 3. Development workflow (two agents)

The existing `AGENTS.md` / `TEAM_WORKFLOW.md` already cover one implementer per task, an independent reviewer, no concurrent edits, branches and worktrees, no pushes or deploys, no unnecessary dependencies, ADRs for architecture changes, no fake completion claims and no unrelated refactoring. `CLAUDE.md` correctly stays a thin pointer. Gaps found, with minimal edits applied to `TEAM_WORKFLOW.md`:

- **No git repository exists**, so the branch/worktree rules cannot work yet (prerequisite P01-00).
- **Shared-ledger conflicts:** both agents writing `docs/PROGRESS.md` is the most likely merge conflict. Only the task's implementer edits it; the reviewer reports in its own review note.
- **Repo-local skills are shared context:** anything under `.agents/skills` or `.claude/skills` silently steers an agent. Adding or changing a skill there needs owner approval, like a dependency.
- **User-level Three.js skills** are installed for Claude Code and Codex. `AGENTS.md` gets one line telling both agents not to apply them here.

## 4. Technical risks added by this assessment

R13 Havok stepping coupling (render-loop-driven) · R14 bundle size and WebGPU first-use hitches · R15 phone testing over LAN silently uses WebGL · R16 third-party and Three.js skills steering agents · R17 TypeScript 7 tooling maturity · R18 no git repo and no registered mobile device. Each is recorded in `RISKS_AND_DECISIONS.md`.

## 5. Phase 00 acceptance status (not approved)

Delivered: environment and skill audit, architecture verification, three proposed ADRs, physics review, workflow clarifications, Phase 01 execution plan. **Still open for the owner:** stack approval (ADR-001…003), hardware/browser targets, bike and course concept, asset/branding acknowledgment, implementer/reviewer naming, Phase 01 plan sign-off, git initialization and the fate of the bufatechno clone. The independent Codex review is pending.

## 6. Sources consulted (2026-11-17)

- Babylon WebGPU: https://doc.babylonjs.com/setup/support/webGPU, `/webGPUStatus/`, `/webGPUBreakingChanges/` (async init; `EngineFactory.CreateAsync` WebGPU→WebGL→Null; no automatic fallback otherwise; listed feature gaps).
- Babylon Havok: https://doc.babylonjs.com/features/featuresDeepDive/physics/havokPlugin/ (MIT; `await HavokPhysics()`; WASM SIMD, not iOS < 16.4; don't use the CDN in production).
- Babylon Physics V2 usage and performance tips: `/physics/usingPhysicsEngine/`, `/physics/perfTips/` (render-loop integration; shape reuse; avoid high-poly mesh shapes; `disablePreStep`).
- Babylon source (master): `Physics/v2/physicsEngine.ts` (`setTimeStep`, `setSubTimeStep`, `_step` clamp 0.1 s), `Physics/v2/Plugins/havokPlugin.ts` (`useDeltaForWorldStep`, `executeStep`), `Physics/joinedPhysicsEngineComponent.pure.ts` (sub-step accumulator, before/after-physics observables, `_advancePhysicsEngineStep` marked internal).
- MDN WebGPU API: https://developer.mozilla.org/en-US/docs/Web/API/WebGPU_API (Limited availability; secure contexts only). The per-browser compat table was not retrieved.
- npm registry metadata via `npm view` (versions in ENVIRONMENT_AUDIT §3).
