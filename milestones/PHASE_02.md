# Phase 02 Execution Specification | Drivetrain, Transmission and Riding Dynamics

**Status:** NOT STARTED. Requires previous dependency gates and explicit user authorization.

## Player / engineering outcome

A testable motorcycle simulation whose acceleration, RPM, gears, braking and lean respond to physically meaningful parameters.

## Proposed bounded tasks

- P02-01: Establish fixed-step unit conventions and reproducible physics fixtures.
- P02-02: Implement torque-curve engine, inertia, drag and rolling resistance.
- P02-03: Implement clutch, neutral, gear ratios, shift delays and automatic shift logic.
- P02-04: Add front/rear braking, traction and bounded steering/lean assistance.
- P02-05: Calibrate at least one bike against a documented data hierarchy and test traces.

## Acceptance evidence

- [ ] RPM/gear/speed equations covered by unit tests.
- [ ] Manual and automatic modes visibly affect the same bike simulation.
- [ ] Frame rendering FPS changes do not change physics outcomes beyond a documented tolerance.
- [ ] Clutch, brake, stall/limiter and zero-speed scenarios show no NaN or uncontrolled forces.
- [ ] Calibrated parameters and remaining approximations explicitly documented.

## Explicit exclusions

Final 3D bike artwork, high-res scenery, AI racing, accounts, multiplayer.

## Execution and review contract

One designated implementer owns each task and a separate agent reviews. Record tested device details, commands, real results, touched paths, skill use, risks and signed acceptance in `docs/PROGRESS.md`. See `docs/MASTER_PLAN.md`, `docs/QA_AND_RELEASE_GATES.md` and the relevant specialist notes.
