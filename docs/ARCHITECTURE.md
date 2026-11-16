# Proposed Technical Architecture

**Status:** Provisional, not implemented. Confirm via Phase 00 feasibility checks.

## Target layers

```text
Browser shell / Vite + React + TypeScript
  |-- Main menu, settings, garage, results, responsive HUD
  |-- Input UI adapters: keyboard, touch, future gamepad
  |-- Zustand: UI/application state and view-models only
  |
  +-- GameRuntime (framework independent)
       |-- InputActions -> InputCommandBuffer
       |-- FixedStepSimulation
       |     |-- Havok V2: rigid body contacts/collisions
       |     |-- MotorcycleDynamics: torque, tires, lean and assists
       |     |-- WorldCollision / RoadFriction
       |-- RaceRules: checkpoints, laps, timing, starts, finish
       |-- Presentation: Babylon scene, animation, camera, HUD snapshot
       |-- Audio: engine RPM/load, road, events
       |-- AssetManager: GLB, KTX2, lifecycle, cache
       +-- Diagnostics: frame time, counters, errors

Deferred services (Phase 09+)
  +-- Supabase Auth / Postgres / RLS / API
  +-- Dedicated authoritative multiplayer server (Phase 10)
```

## Engine choices

- **Babylon.js** is the sole production 3D engine.
- **Havok Physics V2** handles contacts, gravity and rigid-body interactions. Motorcycle tire and drivetrain behavior must be implemented in an explicit module; Havok is not assumed to have turnkey realistic two-wheeler physics.
- **WebGPU** preferred on supported compatible secure contexts; **WebGL** fallback. Detect capability at runtime and provide clear error UX.
- **React** owns UI composition; do not update React state at physics tick frequency. Publish coarse or interpolated read-only snapshots at appropriate UI refresh rates.
- **Vite** is proposed as the minimal single-page game shell; SSR and SEO concerns are secondary to the gameplay client. Defer Next.js unless there is a real product requirement.
- **Supabase** deferred until player accounts/progression. No network request in the core simulation loop.

## Simulation loop

Accumulate measured wall-clock delta; advance a bounded number of fixed simulation steps (provisional 1/120 s for drivetrain/handling experiments, to be measured and confirmed in Phase 02), then interpolate snapshots for rendering. Guard against the spiral of death after background tab suspension. Pause/resume explicitly. Use seconds, meters, kilograms, radians, newtons, newton-meters and watts in core formulas. Convert to km/h and RPM at boundary layers.

Phase 00 finding: Babylon Physics V2 advances Havok from the scene render loop and has no public manual-step API. Fixed steps come from `setSubTimeStep` plus `onBeforePhysicsObservable`. The coupling strategy is proposed in `decisions/ADR_002_simulation_clock_and_havok_stepping.md` and must be proven by spike P01-02A.

Frame rate affects visuals, never physics laws, gear state logic or lap timing. Use a simulation clock for races rather than assuming one rendering frame equals one time slice. A deterministic input log + seeded randomness should permit repeatable test scenarios.

## Input boundary

| Abstract action | Desktop | Mobile | Ownership |
| --- | --- | --- | --- |
| throttle [0..1] | W/Up | hold virtual throttle | Input layer |
| frontBrake [0..1] | S/Down | touch brake | Input layer |
| rearBrake [0..1] | Space | optional second brake | Input layer |
| steer [-1..1] | A/D | 2 buttons, later tilt | Input layer |
| clutch [0..1] | Shift | manual mode control | Input layer |
| shiftUp / shiftDown | E / Q | dedicated buttons | Transmission |
| cameraCycle | C | HUD control | Camera |
| pause / reset | Esc / R | HUD controls | Game flow |

Use simultaneous touch input without gesture cancellation or browser scroll. Mobile landscape is the game orientation target; show a helpful portrait-mode prompt rather than leaving broken controls.

## Planned source tree after Phase 01 approval

```text
src/
  app/                    # React shell and screens
  game/
    runtime/              # initialization, clocks, lifecycle
    input/                # devices -> abstract actions
    physics/              # world, colliders, fixed stepping
    bikes/                # bike configurations, dynamics, transmission
    tracks/               # layout and checkpoints
    racing/               # race states, AI, result rules
    rendering/            # Babylon scene, cameras, shader presets
    audio/                # engine/ambient audio
    assets/               # catalog, manifests, loading
    telemetry/            # locally visible performance counters
  shared/                 # pure types, units, schemas
public/assets/           # only vetted binary assets
scripts/                 # deterministic validation, later
```

This structure is illustrative. Do not create empty code directories solely to satisfy the diagram.

## Performance and memory

Asset byte budgets and device tier thresholds must be validated on representative devices before being treated as acceptance numbers. Initial profiling covers startup, steady track lap, sustained play, mobile memory pressure and background/foreground behavior. Optimize LOD, textures, GPU instancing and draw calls from measurements rather than guesswork.

## Worker architecture

Start simulation on the main thread for integration simplicity. Consider moving selected pure computations into a Worker after profiling shows CPU bottlenecks; transfer/serialization cost and Havok integration may outweigh gains. Never assume physics in a Worker is automatically faster.

## API and persistence boundary

No account is required for R0 through R2. Local options may use browser storage and be exportable/resettable. Future backend must validate data server-side, and offline results must not be treated as trusted competitive leaderboard scores.

## Failure handling

Handle failed engine init, missing assets, unsupported WebGL/WebGPU, lost rendering context, tab visibility changes, browser audio lock, and device orientation changes. Surface recoverable errors to users and diagnostics to developers. No blank screens with silent exceptions.
