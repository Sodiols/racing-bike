---
name: motorcycle-physics
description: Apply the project's motorcycle drivetrain, handling, transmission, calibration and deterministic test policy. Use for tasks that change motorcycle acceleration, torque, gears, clutch, tire grip, braking, steering, lean, suspension or associated physics tests.
---

# Motorcycle Physics Skill

Only use this skill on authorized engineering tasks. This planning repository contains no working physics implementation.

1. Read `docs/PHYSICS_AND_CALIBRATION.md` for the authoritative formulas, unit conventions, calibration hierarchy and test requirements.
2. Read the current milestone in `docs/MASTER_PLAN.md` and its acceptance criteria. Do not implement other phases.
3. Separate the source of real-world specifications from simulation assumptions and handling assists; never invent a verified measurement.
4. Keep SI units internally, fixed physics steps, and deterministic state transitions. Keep simulation independent of graphics frame rate.
5. Test RPM versus wheel speed, gear ratios, clutch coupling, traction, braking and edge cases. Treat numerical stability and plausible rideability as independent tests.
6. When changing physics, produce before/after traces with initial conditions, parameters, physics step duration and tolerances. Report calibration gaps honestly.
7. Do not present Havok rigid-body integration alone as a complete realistic motorcycle dynamics model.
8. Record relevant tests, residual problems and assumptions in `docs/PROGRESS.md`. Require review before phase approval.
