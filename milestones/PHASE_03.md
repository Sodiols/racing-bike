# Phase 03 Execution Specification | Racing Circuit Geometry and Lap Validation

**Status:** NOT STARTED. Requires previous dependency gates and explicit user authorization.

## Player / engineering outcome

A complete circuit that supports believable riding with collision boundaries and trustworthy lap timing.

## Proposed bounded tasks

- P03-01: Define track coordinates, grade/camber assumptions and scene organization.
- P03-02: Build closed road mesh/collisions and spawn/respawn rules.
- P03-03: Add ordered checkpoints, traversal direction and invalid-lap logic.
- P03-04: Add lap timing, start and finish events and test fixtures.
- P03-05: Profile contact stability and track collision performance.

## Acceptance evidence

- [ ] Player can complete repeated valid laps on desktop.
- [ ] Shortcut or reverse traversal does not create a valid lap.
- [ ] No recurring fall-through or impassable road seam on the test route.
- [ ] Lap times use simulation time and withstand rendering frame-rate changes.

## Explicit exclusions

Full environment art, AI competitors, world-scale road network.

## Execution and review contract

One designated implementer owns each task and a separate agent reviews. Record tested device details, commands, real results, touched paths, skill use, risks and signed acceptance in `docs/PROGRESS.md`. See `docs/MASTER_PLAN.md`, `docs/QA_AND_RELEASE_GATES.md` and the relevant specialist notes.
