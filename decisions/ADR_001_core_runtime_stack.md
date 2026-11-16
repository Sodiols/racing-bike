# ADR 001: Core Runtime Stack and Renderer Selection

**Status:** Proposed
**Date:** 2026-11-17
**Owners:** Claude Code (author), Codex (reviewer), project owner (approver)
**Affected phases:** 01–10

## Context

D01, D02 and D04 in `docs/RISKS_AND_DECISIONS.md` propose Vite + TypeScript + Babylon.js + Havok V2 + React/Tailwind/Zustand, with WebGPU and a WebGL fallback. Phase 00 checked this against official docs and the local machine (`docs/PHASE_00_TECHNICAL_ASSESSMENT.md`, `docs/ENVIRONMENT_AUDIT.md`). Babylon does **not** fall back to WebGL automatically when you construct `WebGPUEngine`. The app must choose (`EngineFactory.CreateAsync` tries WebGPU, then WebGL, then Null).

## Options

1. **Keep the proposed stack** (Babylon single engine). Built-in Havok plugin, WebGPU and WebGL maintained side by side, inspector, GUI not needed.
2. Three.js + Rapier. Smaller core, but physics, loaders and tooling must be assembled by hand. Switching engines has no evidence-based benefit here and would contradict AGENTS.md.
3. Native engine export (Unity/Godot web). Large downloads, weaker mobile-web story, outside the plan.

## Decision (proposed, not approved)

Option 1, with these specifics:

- **Package manager:** npm (installed) with a committed lockfile. **Exact version pins** chosen at P01-01A from the then-current releases (2026-11-17 registry: Babylon 9.37.0, Havok 1.3.14, Vite 8.3.8, React 19.3.1, TypeScript 7.0.2, Tailwind 4.3.5, Zustand 5.1.0).
- **Imports:** deep ES-module imports from `@babylonjs/core/...` only, so tree-shaking works. Barrel imports are not allowed.
- **Renderer selection:** try WebGPU if `navigator.gpu` exists and `WebGPUEngine.IsSupportedAsync` resolves true, with a bounded init timeout. On failure use the WebGL2 `Engine`. If both fail, show a user-facing error overlay. A `?renderer=webgpu|webgl` URL override is available for testing. The chosen backend is shown in diagnostics and recorded in test evidence.
- **Not used in Phase 01:** `@babylonjs/gui` (React owns the HUD) and `@babylonjs/loaders` (no GLB until Phase 04).
- **React boundary:** the GameRuntime is a framework-free class with idempotent `start()`/`dispose()`, safe under React StrictMode double-mounting and Vite HMR.

## Consequences

- One engine and one physics plugin keep maintenance down. Bundle size must be measured from the first build.
- WebGPU-only regressions are possible. Both backends must pass smoke tests every phase.
- TypeScript 7 may be ahead of lint/editor tooling. Fall back to the newest supported TS version, recorded in PROGRESS.
- Rollback: renderer selection is isolated in one module, so forcing WebGL-only is a one-line change.
