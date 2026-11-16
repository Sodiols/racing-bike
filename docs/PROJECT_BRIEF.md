# Project Brief and Product Boundaries

**Version:** Draft 1.0 | **Status:** Awaiting approval | **Project state:** Not started

## Vision

A realistic yet accessible browser-based 3D motorcycle racing experience featuring responsive controls, coherent motorcycle behavior, detailed environments, credible sounds, real-world specification references, and adaptive rendering. Players should be able to start and complete a meaningful race on desktop or mobile without installing a native application.

## Product principles

- **Playable first:** a control-responsive motorcycle on a track precedes lavish menus and backend features.
- **Believable, not falsely certified:** physics approximates real motorcycles and is measured against reference data, but is not a safety-grade engineering simulator.
- **Same simulation, adaptive visuals:** different devices can use different rendering quality while preserving gameplay rules.
- **Mobile is a first-class platform:** touch controls and performance are engineered from the first playable milestone.
- **No fake functionality:** menus, HUD, settings and gameplay actions must be connected to actual systems.
- **Commercial discipline:** record asset provenance and the legal basis for any branded content.

## Player journeys

1. Open the HTTPS site, check compatibility, choose control and graphics settings, start a time trial, ride, complete a valid lap and see a result.
2. Switch between keyboard or touch input depending on device, select automatic/manual gears and choose a comfortable camera.
3. Later: compare motorcycles, choose owned bikes, compete against AI, save progress, and race online once networking is validated.

## Release definitions

**R0 - First playable technical slice (Phase 1):** launchable page, basic circuit, temporary motorcycle, throttle, brakes, steering, chase camera, speed indication, pause/reset, keyboard/touch. Not yet a realistic simulator.

**R1 - Credible riding prototype (Phases 2-3):** physically informed drivetrain, shifting, lean/braking handling, calibrated estimates, laps/checkpoints, complete circuit topology.

**R2 - Public single-player MVP (Phases 4-8):** polished licensed assets, responsive sound, adaptive graphics, time trial and circuit race, testing, accessible mobile UI and a useful garage. Account login and real-money purchasing are not required.

**R3 - Optional connected release (Phase 9):** profiles and server-validated persistence. Not a prerequisite for R2.

**R4 - Optional multiplayer release (Phase 10):** authoritative racing networking and anti-cheat, only after separate feasibility and budget approval.

## Non-goals for initial MVP

Open world, city-scale map, realistic pedestrian traffic, damage simulation, VR, live branded dealership integration, global user-generated tracks, native Android/iOS apps, in-app real-money payments, and online multiplayer. These are separate scope decisions.

## Assumptions to validate

- Primary development platform: Windows desktop with Git, Node LTS, browser developer tools.
- Primary mobile evaluation: at least one real Android device; additional hardware classes required before broad release.
- Default price display market: Bangladesh, in BDT. This is a tentative product decision, not yet a data contract.
- Game renderer: Babylon.js with WebGPU when supported and WebGL fallback. Physics V2 Havok for collision and dynamics assistance; custom two-wheeler dynamics still required.
- No commercial asset budget has been approved. The prototype must work with legally usable temporary assets.

## Explicit acceptance principles

A milestone is only accepted when its gameplay behavior is demonstrated and recorded. A README claim, green TypeScript build or screenshot does not by itself prove success. All real-world spec and speed claims need an attributed data source or a clear simulation estimate label.
