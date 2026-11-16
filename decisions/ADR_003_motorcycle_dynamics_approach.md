# ADR 003: Motorcycle Dynamics Approach

**Status:** Proposed
**Date:** 2026-11-17
**Owners:** Claude Code (author), Codex (reviewer), project owner (approver)
**Affected phases:** 01 (provisional model only), 02, 03, 07, 10

## Context

Babylon/Havok has no vehicle or two-wheeler helper (official docs reviewed 2026-11-17). A free rigid body resting on two contact points cannot balance, and keyboard/touch input cannot express countersteer torque. `docs/PHYSICS_AND_CALIBRATION.md` requires SI units, a calibrated drivetrain, separate brakes, traction, lean, suspension and deterministic tests.

## Options

1. **Full multibody in Havok** (chassis + fork + wheels + joints, tire friction from Havok contacts). Closest to physical, but Havok's contact friction is not a tire model. Balance and countersteer would need a rider controller on an unstable system. Tuning is hard and nondeterministic across machines.
2. **Single chassis body + custom wheel/tire model** (ray- or shape-cast suspension contacts, custom longitudinal/lateral tire forces, drivetrain and wheel-spin states, roll as a controlled state driven by a target-lean controller). Havok handles world collision and integration.
3. **Fully kinematic arcade model** (no Havok dynamics). Simple, but it fails the "believable simulation" goal and gives poor collision response.

## Decision (proposed, not approved)

- **Option 2** for Phases 02+. Phase 01 uses a deliberately simple, labeled provisional model with the same input and output interfaces, so Phase 02 replaces the internals, not the contracts.
- The **dynamics core is pure TypeScript** with no Babylon or DOM imports. It is unit-testable in Node and reusable on a future authoritative server. Babylon/Havok code is an adapter around it.
- Rider input sets **target lean / curvature**, bounded by speed and available grip. A labeled assist controller produces the steering and roll.
- Phase 02 adds to the physics plan: per-wheel angular velocity (wheelspin and lockup), longitudinal load transfer (wheelie/stoppie limits), friction-circle combined slip, and low-speed tire-force blending.

## Consequences

- Handling is believable and controllable rather than a literal multibody reproduction. Assists are disclosed in UI and docs.
- Deterministic tests run on the pure core. Havok contact behavior is tested for stability, not bit-exactness.
- Crash/fall physics is not modeled for the MVP; it is replaced by a reset/respawn policy.
- Rollback: if Option 2 handling proves inadequate in Phase 02, a multibody experiment can be built behind the same core interface without touching input, HUD or race code.
