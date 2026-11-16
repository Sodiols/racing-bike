# Phase 06 Execution Specification | Audio, Camera Modes and Player-Facing Racing UI

**Status:** NOT STARTED. Requires previous dependency gates and explicit user authorization.

## Player / engineering outcome

Responsive riding sound and robust presentation of real game state on desktop/mobile.

## Proposed bounded tasks

- P06-01: Build documented RPM/load-driven engine audio graph with compliant sound activation.
- P06-02: Add gear, brake, tire and environmental sound effects.
- P06-03: Implement chase, cockpit and helmet cameras and comfort controls.
- P06-04: Build polished responsive speedometer, tachometer, gear and race HUD.
- P06-05: Test volume controls, muted mode, touch placement and camera transitions.

## Acceptance evidence

- [ ] Sound starts only when browser permits and follows actual engine state.
- [ ] All major camera modes usable in riding maneuvers, without clipping where expected.
- [ ] Speed, RPM and gear HUD originate from simulation state; never hardcoded animators.
- [ ] Manual and mobile controls remain visible and usable in landscape.

## Explicit exclusions

Standalone marketing homepage, 3D engine replacement.

## Execution and review contract

One designated implementer owns each task and a separate agent reviews. Record tested device details, commands, real results, touched paths, skill use, risks and signed acceptance in `docs/PROGRESS.md`. See `docs/MASTER_PLAN.md`, `docs/QA_AND_RELEASE_GATES.md` and the relevant specialist notes.
