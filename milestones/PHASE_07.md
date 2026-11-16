# Phase 07 Execution Specification | Time Trial, Circuit Race and AI Opponents

**Status:** NOT STARTED. Requires previous dependency gates and explicit user authorization.

## Player / engineering outcome

A player can complete a time trial and a race against AI and see valid results.

## Proposed bounded tasks

- P07-01: Implement race lifecycle, countdown, timing and state transitions.
- P07-02: Implement course waypoint and corner speed planning for AI bikes.
- P07-03: Add collision awareness, overtaking rules and reset strategy.
- P07-04: Implement race results, positions, finishing, retry and time-trial bests.
- P07-05: Run race flow tests and repeated race sessions with diagnostics.

## Acceptance evidence

- [ ] Full time-trial path works end-to-end.
- [ ] AI circuit race has correct start, position, finish and results.
- [ ] AI behavior respects physical motion constraints and does not routinely teleport.
- [ ] Resets, replays and invalid laps cannot corrupt standings.

## Explicit exclusions

Global leaderboards, public matchmaking, realtime multiplayer.

## Execution and review contract

One designated implementer owns each task and a separate agent reviews. Record tested device details, commands, real results, touched paths, skill use, risks and signed acceptance in `docs/PROGRESS.md`. See `docs/MASTER_PLAN.md`, `docs/QA_AND_RELEASE_GATES.md` and the relevant specialist notes.
