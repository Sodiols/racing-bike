# Project Progress and Evidence Ledger

**As of:** 17 November 2026  
**Overall status:** PHASE 00 ASSESSMENT DELIVERED; AWAITING CODEX REVIEW AND OWNER APPROVAL. No game development has started.  
**Current phase:** 00 (in review). Phase 01 not authorized.  
**Repository:** No game source code, no dependencies, no engine bootstrap, no game tests, no assets acquired, no database, no deployment. Not yet a Git repository.

## Phase tracker

| Phase | Name | Status | Evidence / approval |
| --- | --- | --- | --- |
| 00 | Project foundation and decisions | IN REVIEW | Assessment by Claude Code 2026-11-17 (see below); Codex review and owner gate pending |
| 01 | Playable 3D vertical slice | NOT STARTED | None |
| 02 | Drivetrain and handling | NOT STARTED | None |
| 03 | Track and lap system | NOT STARTED | None |
| 04 | Motorcycle and track art | NOT STARTED | None |
| 05 | Graphics and performance | NOT STARTED | None |
| 06 | Audio, cameras, HUD | NOT STARTED | None |
| 07 | Race loop and AI | NOT STARTED | None |
| 08 | Garage and single-player release | NOT STARTED | None |
| 09 | Optional connected accounts | NOT STARTED | None |
| 10 | Optional multiplayer | NOT STARTED | None |

## Planning artifacts delivered

This planning set contains proposed scope, architecture, risks, agent protocols, asset strategy, QA gates and a custom documentation-only physics skill for both tools. Drafting planning files does not constitute engineering validation or a completed Phase 00.

## Phase 00 record (2026-11-17, implementer: Claude Code, reviewer: Codex, pending)

**Completed and verified (command evidence in `docs/ENVIRONMENT_AUDIT.md`):**
- Planning baseline integrity: `sha256sum -c FILE_MANIFEST.sha256` gave 31/31 OK before any edit. The manifest describes the v1.0 baseline and is intentionally not regenerated.
- Environment: Windows 11 Pro 26300, Node 24.16.0, npm 11.13.0, Git 2.54.0, Chrome 157, Edge 157; Ryzen 5 7535U with Radeon iGPU (RDNA2), 15.3 GB RAM. No touchscreen.
- WebGPU: hardware adapter and device created in **headless** Chrome 157 and Edge 157 from a `127.0.0.1` page; WebGL2 available (ANGLE D3D11). No Babylon frame rendered yet.
- Skills: `motorcycle-physics` and `frontend-design` loaded in Claude Code; bufatechno clone present but not loaded by Claude Code; OpenAI `game-playtest` not installed (only in Codex marketplace cache).
- Official Babylon docs and source read: Havok MIT and WASM SIMD; physics stepping is render-loop driven; no vehicle helper; no automatic WebGPU→WebGL fallback.

**Documentation created:** `docs/ENVIRONMENT_AUDIT.md`, `docs/PHASE_00_TECHNICAL_ASSESSMENT.md`, `docs/PHASE_01_EXECUTION_PLAN.md`, `decisions/ADR_001_core_runtime_stack.md`, `ADR_002_simulation_clock_and_havok_stepping.md`, `ADR_003_motorcycle_dynamics_approach.md`.
**Documentation updated:** this file; `RISKS_AND_DECISIONS.md` (R13–R18, ADR links); `TEAM_WORKFLOW.md` (Phase 00 clarifications); `AGENTS.md` (one line on user-level Three.js skills); `SKILLS_AND_REFERENCES.md` (audit status); `ARCHITECTURE.md` (stepping note); `milestones/PHASE_00.md` (status); `README.md` (document index).

**Recommendations (not executed):** stack preserved; ADR-002 Havok stepping spike first in Phase 01; physics plan additions (load transfer, wheel-spin state, combined slip, low-speed tire blending, target-lean steering assist); remove the bufatechno clone from the repo.

**Unresolved decisions / awaiting owner approval:**
1. Approve, amend or reject ADR-001, ADR-002, ADR-003 (all *Proposed*).
2. Authorize `git init` + `.gitignore` + baseline commit (P01-00), and decide the fate of `.agents/skills/bufatechno-webgamedev` first.
3. Minimum hardware/browser targets; name at least one real Android phone (model, OS, Chrome version).
4. Bike and course concept (fictional is allowed); asset/branding policy acknowledgment.
5. Confirm roles for Phase 01 (proposed: Claude Code implementer, Codex reviewer).
6. Sign off `docs/PHASE_01_EXECUTION_PLAN.md`, including the dependency list in P01-01A.
7. Mobile test access method (`adb reverse` with Android platform-tools, or local HTTPS dev certificate).

**Phase 00 gate checklist (`milestones/PHASE_00.md`):** all six items remain unchecked pending owner approval.

## Change log

| Date | Change | Author / evidence |
| --- | --- | --- |
| 2026-11-17 | Initial planning package authored, not implemented | Planning draft |
| 2026-11-17 | Phase 00 technical assessment: environment and skill audit, architecture verification, 3 proposed ADRs, Phase 01 execution plan | Claude Code; read-only commands only, no installs |

## Next action

Codex performs an independent review of the Phase 00 deliverables listed above. Then the owner decides the open items and approves or rejects the Phase 00 gate. Phase 01 starts only after that approval, beginning with P01-00.
