# Shared Agent Instructions: Browser Motorcycle Simulator

This file is authoritative for **Claude Code and Codex**. Keep it concise. Details live in linked documents and are read only when relevant to the current task.

## Current state and execution boundary

- Planning repository only. No game code has been written; no phases are implemented.
- Do not implement any game feature, install third-party skills/packages, initialize Supabase, push commits, or deploy unless the user explicitly authorizes the corresponding task.
- Do not claim a feature, test, device benchmark or asset license has been verified without evidence.
- Current status: `docs/PROGRESS.md`. Current roadmap: `docs/MASTER_PLAN.md`.

## Proposed stack (provisional until Phase 0 approved)

TypeScript, Babylon.js, Havok Physics V2, WebGPU with WebGL fallback, React, Tailwind CSS, Zustand, Vite; glTF/GLB content. Supabase/PostgreSQL is deferred until persistent accounts and progression are needed. Avoid adding Three.js or another rendering engine to the runtime.

## Essential design rules

1. First deliver a playable, measurable vertical slice. Do not substitute marketing pages, static scenes or false HUD data for gameplay.
2. Use fixed-step physics decoupled from rendering; all HUD values derive from simulation state.
3. Implement inputs through a shared action abstraction for keyboard and mobile touch; the same gameplay state machine handles both.
4. Separate rendering, physics, drivetrain, input, races, audio, assets, UI and persistence. React manages menus/HUD, not per-frame simulation.
5. Use measured constraints instead of assuming Ultra graphics can run at 60 FPS on every device.
6. Do not invent verified real-world motorcycle specifications or regional prices. Track source URL, year, region, currency and verification status.
7. Respect licensing of motorcycle models, emblems, music, sound, textures and third-party scripts.
8. Prefer original or clearly licensed placeholders for early milestones. Label approximations and assists honestly.
9. Add no irreversible database, security, environment or deployment changes without authorization.

## How to work

- For a task, read only the relevant milestone and architecture sections; do not reread the entire planning corpus by default.
- Determine the requested phase and its acceptance criteria **before modifying files**. If the user's request is planning-only, make documentation changes only.
- Check relevant local skills if installed. Do not claim external skills are installed merely because URLs appear in `docs/SKILLS_AND_REFERENCES.md`.
- Do not apply Three.js-specific skills (for example the user-level `threejs-*` suite) or skills that default to another engine; see `docs/ENVIRONMENT_AUDIT.md` §5.
- Use one primary agent to implement a bounded task; have the other agent review the resulting diff. Follow `docs/TEAM_WORKFLOW.md`. Never run two agents concurrently writing the same files or branch.
- For implementation tasks: provide a short plan, make a focused change, run relevant tests, verify actual browser behavior when feasible, and record real results in `docs/PROGRESS.md`.
- Never fabricate test outcomes, benchmark values, review approvals or completed milestones.
- Prefer the smallest correct change and targeted file reads. Avoid repeated exploratory loops and unnecessary dependencies.
- Store key design choices in `decisions/` when they materially affect future phases.
- Do not automatically push to remote, merge into protected branches, or deploy.

## Definition of complete

A milestone is complete only when all acceptance criteria in `docs/MASTER_PLAN.md` and `docs/QA_AND_RELEASE_GATES.md` are met, evidence is recorded, and the user approves the phase gate. A successful `npm run build` is not gameplay validation.

## Reporting format

Report: implemented scope; paths changed; skills actually used; commands and test results; physical/device evidence; remaining risks; proposed next task. Keep updates concise and factual.
