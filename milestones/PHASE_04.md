# Phase 04 Execution Specification | Motorcycle Art and Environment Asset Integration

**Status:** NOT STARTED. Requires previous dependency gates and explicit user authorization.

## Player / engineering outcome

At least one licensed high-fidelity motorcycle integrated with animation and coherent track visual assets.

## Proposed bounded tasks

- P04-01: Select and audit motorcycle, road and ambience asset provenance.
- P04-02: Convert assets to normalized GLB/glTF geometry and texture budgets.
- P04-03: Configure model pivots: wheel spin, front steering, suspension and rider where available.
- P04-04: Connect animation to actual simulation state and contact points.
- P04-05: Establish track scenery assets and material consistency.

## Acceptance evidence

- [ ] All included assets have source/license entries and approval.
- [ ] Visual wheel radius and wheelbase match physics config within approved tolerance.
- [ ] Bike wheels, lean, fork and suspension no longer appear static while riding.
- [ ] Scene asset weights and mobile fallback/LOD are recorded.

## Explicit exclusions

Unlicensed branded motorcycle emblems, ripped game assets, separate 3D engine.

## Execution and review contract

One designated implementer owns each task and a separate agent reviews. Record tested device details, commands, real results, touched paths, skill use, risks and signed acceptance in `docs/PROGRESS.md`. See `docs/MASTER_PLAN.md`, `docs/QA_AND_RELEASE_GATES.md` and the relevant specialist notes.
