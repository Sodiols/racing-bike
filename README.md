# Motorcycle Racing Simulator | Planning Repository

**Project status:** PLANNING ONLY. No game code, engine setup, asset downloads, database, deployment, or feature implementation has begun.

**Planning baseline:** 17 November 2026, draft v1.0. This is a proposed architecture and development process, not evidence that feasibility has been validated.

## Project objective

Build a playable, high-quality 3D motorcycle racing simulator in a browser. It must support desktop keyboard and mobile touch input, automatic and manual shifting, believable motorcycle dynamics, calibrated performance, detailed tracks, adaptive graphics, and eventually a garage and multiplayer. The first release prioritizes a complete single-player time-trial experience over quantity of content.

## Read in this order

1. [`docs/PROJECT_BRIEF.md`](docs/PROJECT_BRIEF.md) for goals, scope and constraints.
2. [`docs/MASTER_PLAN.md`](docs/MASTER_PLAN.md) for the milestones and prerequisites.
3. [`docs/ARCHITECTURE.md`](docs/ARCHITECTURE.md) for software boundaries and design principles.
4. [`docs/TEAM_WORKFLOW.md`](docs/TEAM_WORKFLOW.md) for coordinating Claude Code and Codex.
5. [`AGENTS.md`](AGENTS.md) and [`CLAUDE.md`](CLAUDE.md) for agent working agreements.
6. [`docs/PROGRESS.md`](docs/PROGRESS.md) for the single source of truth on what has really been done.

## Documents

| File | Purpose |
| --- | --- |
| `AGENTS.md` | Shared durable instructions for both coding agents |
| `CLAUDE.md` | Thin Claude Code pointer; no duplicate rulebook |
| `docs/PROJECT_BRIEF.md` | Product vision, non-goals, release definition |
| `docs/PLAYER_EXPERIENCE_AND_UI.md` | Motorsport UI/UX principles and player journey |
| `docs/TASK_HANDOFF_TEMPLATE.md` | Concise implementation and reviewer handoff contract |
| `docs/MASTER_PLAN.md` | Phase-by-phase roadmap with acceptance gates |
| `docs/ARCHITECTURE.md` | Proposed client, engine, simulation, data boundaries |
| `docs/PHYSICS_AND_CALIBRATION.md` | Motorcycle engineering, modeling and verification approach |
| `docs/GRAPHICS_AND_PERFORMANCE.md` | Quality tiers, mobile constraints and measurement plan |
| `docs/ASSETS_AND_LICENSING.md` | Asset sourcing, provenance and brand licensing |
| `docs/QA_AND_RELEASE_GATES.md` | Functional, physics, performance and release checks |
| `docs/TEAM_WORKFLOW.md` | Claude/Codex ownership, branching, review and handoff |
| `docs/DATA_AND_BACKEND.md` | Planned persistence, pricing and multiplayer boundaries |
| `docs/RISKS_AND_DECISIONS.md` | Risk register, architectural decisions and open questions |
| `docs/PROGRESS.md` | Current status and evidence log; Phase 00 in review, no game code |
| `docs/ENVIRONMENT_AUDIT.md` | Phase 00 verified tool, browser, GPU and skill audit |
| `docs/PHASE_00_TECHNICAL_ASSESSMENT.md` | Phase 00 architecture, physics-plan and workflow review |
| `docs/PHASE_01_EXECUTION_PLAN.md` | Proposed Phase 01 task breakdown (not authorized) |
| `decisions/ADR_001`–`ADR_003` | Proposed stack, simulation clock and bike dynamics decisions |
| `docs/SKILLS_AND_REFERENCES.md` | Skill policy and verified documentation links |
| `milestones/PHASE_00.md` | Planning acceptance and approval worksheet |
| `milestones/PHASE_01.md` | First playable prototype scope, for later |
| `milestones/PHASE_02.md` through `PHASE_10.md` | Later individual implementation gates |
| `decisions/ADR_TEMPLATE.md` | Record significant technical decisions |
| `.agents/skills/motorcycle-physics/SKILL.md` | Project-authored Codex skill guidance |
| `.claude/skills/motorcycle-physics/SKILL.md` | Project-authored Claude skill guidance |

## How to use this package

Extract its contents into a **new empty repository**, preserving hidden folders. Do not overwrite an existing project without first comparing files. Treat this as documentation only; no `package.json` or game implementation is intentionally included. Read Phase 0 and approve the technical decisions before giving either agent an implementation task. Use the milestone-specific brief, rather than pasting the entire master plan into every message.

The final project name, commercial licenses, initial target devices, pricing region, and initial motorcycle models are still subject to approval. The default proposed pricing region is Bangladesh (BDT).
