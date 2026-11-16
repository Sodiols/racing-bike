# Phase 01 Execution Specification | First Rideable Prototype

**Status:** NOT STARTED. Do not execute until Phase 00 receives user approval.

## Player outcome

A player loads the game in a real browser, starts a session, accelerates, turns, brakes, sees an honest speed display, pauses and resets. Desktop keyboard and simultaneous mobile touch controls both work on a simple course.

## Scoped tasks

- P01-01: runtime entry, safe engine selection, basic error overlay and resize handling.
- P01-02: short closed loop test course and collision ground, with clear visual track boundaries.
- P01-03: provisional bike proxy with physically meaningful units and fixed-step scaffold.
- P01-04: input action abstraction with keyboard and multitouch adapters.
- P01-05: stable chase camera, minimal HUD, pause/reset/scene lifecycle.
- P01-06: smoke tests, mobile browser playtest and baseline performance log.

## Expected evidence

Build and typecheck results, gameplay browser record, test commands/results, keyboard/multitouch checklist, device/browser identification, WebGPU or WebGL mode recorded, no uncaught error, limitations documented.

## Explicit non-goals

True motorcycle gearbox, production visuals, manufacturer licenses, open world, AI, garage, login, virtual currency, online racing.

## Exit gate

One complete track traversal controlled by a human on desktop and available real mobile hardware; no blocked core action; known shortcomings tracked. The user must approve acceptance.
