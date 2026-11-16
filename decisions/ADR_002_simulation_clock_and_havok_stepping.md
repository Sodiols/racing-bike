# ADR 002: Simulation Clock and Havok Stepping

**Status:** Proposed
**Date:** 2026-11-17
**Owners:** Claude Code (author), Codex (reviewer), project owner (approver)
**Affected phases:** 01–03, 07, 10

## Context

The architecture requires fixed-step simulation, independent of frame rate, with interpolated rendering (`docs/ARCHITECTURE.md`, "Simulation loop"). Facts from Babylon source (master, read 2026-11-17):

- Physics V2 is advanced by the **scene during the render loop**. `Scene._advancePhysicsEngineStep` is `@internal`. No public "disable auto-step / step manually" API was found.
- `physicsEngine.setSubTimeStep(ms)` turns on a scene accumulator that runs **N fixed substeps per frame** and fires `onBeforePhysicsObservable` / `onAfterPhysicsObservable` around each substep.
- `new HavokPlugin(useDeltaForWorldStep = true, ...)`: when `false`, every world step uses `setTimeStep` regardless of elapsed time. Simulated time then runs at frame rate × step, which is **frame-rate dependent**.
- `physicsEngine._step` clamps any delta over 0.1 s down to 0.1 s.
- Babylon does not interpolate physics body transforms for rendering.

## Options

1. **Babylon substeps:** `useDeltaForWorldStep = true`, `setSubTimeStep(1000/120)`. Apply motorcycle forces in `onBeforePhysicsObservable`. Supported public API. Needs verification of remainder carry-over, per-frame step caps and pause behavior. No render interpolation.
2. **Own clock, scene-driven step:** GameRuntime accumulator decides how many steps to run, then advances Havok one fixed step at a time through an internal or semi-public path. Full control, but relies on internal API that may change.
3. **Variable step** (default Babylon). Rejected: violates the frame-rate independence requirement.

## Decision (proposed, not approved)

- **GameRuntime owns the authoritative simulation clock**: fixed `dt` (provisionally 1/120 s), bounded steps per frame (for example, max 8; excess time is dropped and counted in diagnostics), accumulator reset on pause and on `visibilitychange`. The race clock is `stepCount × dt`, never wall time.
- **Havok coupling:** start with **Option 1** because it uses only public API. The pure-TS clock is the reference. P01-02A must show with a test that Havok's stepped body state matches the clock (same step count and dt) across 30, 60 and 144 FPS frame pacing. If the scene accumulator cannot be made to match (caps, clamping, remainder handling), switch to Option 2 behind a single adapter module and record the internal API used.
- **Rendering:** visual meshes are separate from physics bodies. They are interpolated between the previous and current simulation states using the accumulator alpha.
- **Motorcycle dynamics** (ADR-003) runs inside the per-step hook and reads and writes only its own state plus explicit body forces and velocities.

## Consequences

- Lap timing and HUD values do not depend on frame rate. Phase 02/03 frame-rate sweep tests have a defined reference.
- Phase 01 carries one small spike (P01-02A) before any gameplay code relies on the coupling.
- Option 2 would pin us to a Babylon internal. Mitigation: one adapter file, pinned Babylon version, and a test that fails on upgrade if the behavior changes.
- Rollback: the clock module is pure and engine-independent, so only the adapter changes between options.
