# Master Development Plan

**Draft version:** 1.0, 17 November 2026  
**Status:** Planning package; awaiting user approval  
**Initial implementation:** None. No phases have started.

## 1. Delivery strategy

Do not give either coding agent a single instruction to build the entire simulator. Work through individually accepted milestones. The early phases establish a running end-to-end game loop, then replace simple systems with calibrated implementations. The same project documents and completion log are shared by Claude Code and Codex.

**Primary critical path:** architecture and tool feasibility -> playable track and input -> riding dynamics -> traversable track, checkpointing and timing -> production assets -> graphics/performance -> audio/cameras/HUD -> racing AI/results -> garage/progression -> release qualification. Online persistence and multiplayer are optional later tracks.

### Status codes

`NOT STARTED`, `IN PROGRESS`, `BLOCKED`, `IN REVIEW`, `ACCEPTED`. Only change status with documented evidence in `docs/PROGRESS.md`.

### Phase gates

Each phase must satisfy four conditions: scoped deliverables built, tests run, a human-verifiable demonstration captured, and review/approval recorded. Performance gates require measurements from named devices; do not invent them.

## 2. Work breakdown and dependency map

| Phase | Short name | Depends on | Main deliverable | Target release |
| --- | --- | --- | --- | --- |
| 00 | Project foundation and decisions | None | Approved stack, engineering baseline, worktree, risk and asset strategy | R0 |
| 01 | Playable 3D vertical slice | 00 | Test track, temp bike, keyboard/touch, camera, speed HUD | R0 |
| 02 | Drivetrain and motorcycle handling | 01 | Fixed-step dynamics, torque, gears, lean, brakes and tests | R1 |
| 03 | Track topology and timed riding | 02 | Full circuit, colliders, checkpoints, lap timing | R1 |
| 04 | Production bike and environment assets | 03 | Legally usable models, animation, road material, track art | R2 |
| 05 | Rendering and adaptive performance | 04 (design can begin 01) | Graphics presets, tested fallbacks, LOD, performance evidence | R2 |
| 06 | Sound, cameras and rider-facing UI | 02,04,05 | RPM-driven audio, 3 camera modes, readable mobile HUD | R2 |
| 07 | Race modes and AI | 03,06 | Time trial, circuit race with AI, results and restart | R2 |
| 08 | Garage, settings and release quality | 05,07 | Garage, bike selection, preferences, QA and browser release candidate | R2 |
| 09 | Connected accounts and persistence | 08 | Optional Supabase accounts, saved garage, validated economy | R3 |
| 10 | Multiplayer research and implementation | 07,09 + separate approval | Optional authoritative racing rooms and netcode | R4 |

**Order rationale:** Phase 01 must already be playable on a simple test road. Phase 03 builds a credible full circuit after physics is functional. Detailed scenery never blocks controls or core driving tests. Track and asset optimization remain iterative across multiple phases.

## 3. Phase 00: Project foundation and feasibility

**Objective:** Make architectural and commercial constraints explicit before installing frameworks.

**Scope:** repository/Git branching strategy; approved requirements; supported browser matrix; package/runtime strategy; engine/Havok/WebGPU capability spike plan; third-party skill review; licensing strategy; baseline acceptance metrics; work breakdown and ADRs. Choose the lead agent for Phase 01 and the review agent.

**Deliverables:** revised `AGENTS.md`/`CLAUDE.md`, approved roadmap, ADRs for stack and simulation approach, initial target device list, asset sourcing shortlist, test strategy, implementation estimate ranges only after feasibility.

**Acceptance:** user approves scope, open decisions are assigned, no unauthorized installs or deployments performed, both agents can read consistent instructions and one implementation branch owner is designated.

**Exit demonstration:** a simple documented architectural walkthrough and readiness checklist, not gameplay.

## 4. Phase 01: First playable 3D vertical slice

**Objective:** Confirm the full chain from browser input to motorcycle motion to visual rendering, for desktop and mobile.

**Scope:** Vite/React/TypeScript/Babylon bootstrap; robust WebGPU/WebGL selection; minimal track and ground collider; provisional motorcycle body and wheels; fixed-step simulation scaffold; stable chase camera; throttle, brake, steer, reset and pause; keyboard and multitouch action mapping; minimal speed/HUD; resize and orientation handling.

**Explicitly deferred:** realistic gear/clutch, premium 3D bike, real manufacturer models, AI, multiplayer, backend, elaborate menu, full-world scenery.

**Acceptance:** player can load page and complete one circuit traversal with responsive keyboard controls; mobile controls allow steering + throttle simultaneously; reset and pause function; measured frame rates and startup errors recorded on available test hardware; no unhandled console exceptions on primary browser. Simplified physics clearly labeled.

**Exit demonstration:** short desktop and real-mobile gameplay capture or a documented blocker where physical hardware is unavailable.

## 5. Phase 02: Drivetrain, transmission and bike dynamics

**Objective:** Replace arcade motion with a documented, physically informed simulation, while preserving useful controllability.

**Scope:** unit-consistent SI model, torque curve interpolation, gearing, wheel torque, clutch, RPM dynamics, drag, rolling resistance, mass, braking forces, tire grip, lean/steering assist, suspension approximation, deterministic fixed steps, manual and automatic shifting, gear/RPM UI truthfulness.

**Acceptance:** simulation has automated unit tests for RPM/gear/speed transformations, torque, acceleration/drag, clutch and shift guards; calibration evidence on one documented reference motorcycle or explicitly original bike with disclosed assumptions; stable behavior at low and high speed; repeatable measured tests and no NaN/energy explosions; user can switch manual/automatic and feel resulting difference.

**Gate caveat:** achieving lifelike two-wheel balancing and tire behavior cannot be guaranteed by rigid-body physics integration alone. The model must clearly document riding assists, limits and simplified assumptions.

## 6. Phase 03: Full test circuit and race validation

**Objective:** Convert the simple driving space into a technically complete race circuit.

**Scope:** closed track mesh with consistent width and road collisions, elevation/camber tests, environment collision, spawn/reset safety, ordered checkpoints, direction verification, lap timing, invalid-lap safeguards, out-of-bounds handling, collision profiling.

**Acceptance:** player can complete valid laps; shortcut/reverse checkpoint abuse does not count; no reproducible drop-through, teleport or impassable seam; lap times survive varying render frame rates; collision and geometry performance tracked.

## 7. Phase 04: Art and animation integration

**Objective:** Replace prototypes with original/licensed production content without changing simulation integrity.

**Scope:** at least one high quality optimized motorcycle model with correct origin/scale, separate movable parts, riding pose/rider model when legal assets exist, wheel rotation, steering, lean, suspension animation, physically based road and landscape materials, track environment props, glTF/GLB import and versioned asset manifests.

**Acceptance:** asset register contains model, audio/texture license and provenance, scale checks and performance budget; bike appearance matches physical pivot placement; visual motion follows true physics state; at least one art-quality review across desktop/mobile presets.

**Commercial blocker:** branded assets may not be used unless rights are documented.

## 8. Phase 05: Advanced graphics and performance

**Objective:** Provide the best achievable visuals per supported hardware, not universal maximum settings.

**Scope:** WebGPU/WebGL fallback, Low/Medium/High/Ultra/Auto presets, scene load strategy, KTX2/texture compression where supported, LOD, instancing, sensible shadows/reflections, dynamic render scaling, scene profiling, memory/context loss recovery, debug/performance overlay.

**Acceptance:** no unsupported device receives a blank scene; settings have measurable effects; frame-rate distributions recorded per reference device and browser; target frame-time criteria and degradation strategy are documented; scene survives resize/context loss test; no unbounded resource leak in endurance run. Official performance numbers must come from actual devices.

## 9. Phase 06: Audio, cameras and HUD polish

**Objective:** Make the riding experience coherent, informative and comfortable.

**Scope:** engine audio based on RPM/load (not vehicle speed alone), shifting/braking/road sound, browser gesture based audio startup, volume controls, chase/cockpit/helmet cameras, mobile HUD hierarchy, manual/auto indicators, accessibility settings and user feedback.

**Acceptance:** audio starts with permitted user input, remains muteable, responds to actual RPM/gear; cameras remain stable under braking/lean; HUD values come from simulation; touch targets have real-device usability checks; settings persist locally.

## 10. Phase 07: Racing game loop and AI

**Objective:** Deliver meaningful complete racing sessions beyond free riding.

**Scope:** time trial, race start countdown, timing and penalties, circuit race, AI route following and corner speed planning, basic overtake/obstacle handling, results, restart, ghost or replay only if feasible.

**Acceptance:** at least one complete time trial and one AI race can be started and finished; consistent standings, checkpoint rules and results; AI does not routinely leave track, teleport or ignore collisions; deterministic gameplay tests for lap/rank states; failure states recover cleanly.

## 11. Phase 08: Garage, economy and single-player release candidate

**Objective:** Package a cohesive, performance-tested public single-player release.

**Scope:** motorcycle selection/garage, at least three distinct **legally usable** vehicles if practical, sourced specifications, Bangladesh market price metadata only where verified, locally persistent options, accessibility, integration tests, representative mobile testing, QA bug triage, release instructions.

**Acceptance:** core journey works in tested browsers: launch -> select -> race -> result -> restart; manual/auto gearbox, mobile and keyboard, settings, graphics tier, and error recovery validated; asset licenses reviewed; performance thresholds assessed against selected device tiers. No fake global compatibility or regulatory compliance statements.

**MVP exit:** user approves public single-player release candidate. No online multiplayer requirement.

## 12. Phase 09: Optional online accounts and progression

**Objective:** Introduce persistent profiles and virtual economy without harming offline racing.

**Scope:** Supabase Auth, RLS, immutable purchase ledger or equivalent audited transactions, bike ownership, player settings, results provenance, rate limiting, privacy policy and abuse controls. Pricing source data and virtual credits remain separate concepts.

**Acceptance:** permission/RLS tests prevent cross-user access, credit tampering and duplicate purchases; client cannot grant itself assets; migration/rollback and backup plan documented; no production migration without authorization.

## 13. Phase 10: Optional multiplayer

**Objective:** Prove networking approach and only then build authoritative multiplayer.

**Scope:** dedicated server design, input validation, simulation ownership, reconciliation, interpolation, prediction, session/lobby lifecycle, race integrity, region latency profiling, abuse mitigation. Supabase is for profiles/metadata, not high frequency real-time vehicle authority.

**Acceptance:** networking prototype meets separately approved latency, tick-rate and budget criteria; adversarial tests cover tampered clients and disconnects; players cannot manipulate authoritative positions, rankings or economy. User approves the multiplayer expansion separately.

## 14. Milestone task format (mandatory for later execution)

Every implementation ticket must contain: **phase and task ID; objective; exact in-scope files/subsystems; excluded work; dependencies; test commands; manual checks; evidence required; reviewer; rollback; and completion gate**. If any item is unknown, annotate `TBD` instead of making up facts.

## 15. Formal change control

Any change that alters the rendering engine, core physics approach, input contracts, database authority, multiplayer topology, source license terms, or target hardware needs an ADR in `decisions/` and user approval. Non-breaking visual details may be revised within a milestone, but do not silently expand scope.

## 16. Stop/go criteria

**Proceed:** milestone acceptance is supported by tests and real play evidence, review findings are resolved or accepted as bounded issues, next phase dependencies are available.

**Stop:** core simulation is unplayable, device evidence is missing for claimed support, critical licensing uncertainty remains, physics produces unexplained unstable results, or high-severity bugs cannot be reproduced and triaged.

## 17. Immediate next step

Review and approve Phase 00 decisions. This planning package itself does **not** mean Phase 00 or any implementation phase has been executed. After approval, appoint one coding agent to do Phase 00 and the other to audit its output. Only then authorize Phase 01.
