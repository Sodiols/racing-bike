# Phase 00 Environment and Skill Audit

**Task:** P00-01 / P00-05 | **Date:** 2026-11-17 | **Agent:** Claude Code (implementer) | **Reviewer:** Codex (pending)
**Scope:** Read-only inspection. Nothing was installed; no project dependency, package or skill was added. Temporary probe files lived only in the agent scratchpad outside the repository.

## 1. Confirmed facts (command evidence)

| Item | Result | How verified |
| --- | --- | --- |
| OS | Windows 11 Pro 10.0.26300 (build 26300), 64-bit | `Get-CimInstance Win32_OperatingSystem` |
| CPU / RAM | AMD Ryzen 5 7535U, 6 cores / 12 threads, 15.3 GB RAM | `Win32_Processor`, `Win32_OperatingSystem` |
| GPU / display | AMD Radeon integrated (RDNA2, Radeon 660M class), driver 32.0.21032.4002, 1920x1200 | `Win32_VideoController`; WebGPU adapter info `amd / rdna-2` |
| Free disk | D: 190.5 GB, C: 30.7 GB | `Get-PSDrive` |
| Node.js | v24.16.0 | `node --version` |
| npm / npx | 11.13.0 | `npm --version` |
| Corepack | 0.35.0 (pnpm/yarn not activated) | `corepack --version` |
| pnpm, yarn, bun, deno | Not installed | command not found |
| Git | 2.54.0.windows.1 | `git --version` |
| Project Git repository | **Not initialized** (`fatal: not a git repository`) | `git rev-parse` |
| TypeScript (global) | Not installed globally; it should be a project devDependency anyway | `tsc` not found |
| Python | 3.14.5 | `python --version` |
| GitHub CLI | Not installed | `gh` not found |
| VS Code | 1.146.0, extension `anthropic.claude-code-2.1.325` | `code --version`, extensions dir |
| Global npm packages | only `uipro-cli@2.2.3` | `npm ls -g --depth=0` |
| Chrome | 157.0.8103.51 | file version of `chrome.exe` |
| Edge | 157.0.4341.60 | file version of `msedge.exe` |
| Firefox, Brave, Opera, Safari | Not found | standard install paths |
| Claude Code | Running in the Claude desktop app (Code tab), model Opus 5.5; `claude` CLI not on PATH | session environment, `claude` not found |
| Codex | Desktop app state exists in `%USERPROFILE%\.codex` (config, plugins); `codex` CLI not on PATH | directory listing, `codex` not found |

### Browser graphics capability probe (this laptop)

A local page served from `127.0.0.1` (secure context) was opened in **headless** Chrome 157 and Edge 157 with throwaway profiles. Both returned identical results:

- `navigator.gpu` present; `requestAdapter({powerPreference:'high-performance'})` returned a **hardware** adapter (`isFallbackAdapter=false`, vendor `amd`, architecture `rdna-2`), and `requestDevice()` succeeded.
- Notable features: `texture-compression-bc`, `shader-f16`, `timestamp-query`, `float32-filterable`, `subgroups`. `maxTextureDimension2D=16384`, `maxBufferSize=2 GiB`.
- WebGL2 and WebGL1 available: `ANGLE (AMD Radeon(TM) Graphics, Direct3D11)`, max texture 16384, S3TC/BPTC yes, ASTC/ETC no (expected on desktop).
- WebAssembly present (Havok requirement). `SharedArrayBuffer` unavailable because the page was not cross-origin isolated (normal; not required by the plan).
- `maxTouchPoints = 0`: **this laptop has no touchscreen**. Real multitouch cannot be tested on it.

## 2. Not verified / assumptions

- WebGPU was confirmed for adapter and device creation only. **No Babylon.js frame was rendered**, and headed-browser behavior was not separately probed. Rendering, shader compilation time and frame time are untested.
- WebGPU on mobile browsers is not verified here. From general knowledge, not checked this session: Chrome Android ships WebGPU on recent Android versions and GPUs, Safari ships it from the 26 releases, and Firefox Android does not ship it by default. MDN currently labels WebGPU *Limited availability (not Baseline)* and secure-context-only (verified via MDN page text). **Treat WebGL2 as the guaranteed mobile path.**
- Node 24's LTS phase as of November 2026 was not checked. It satisfies every engine requirement queried (Vite 8 `^20.19 || >=22.12`, Vitest 5 `^22.12 || ^24 || >=26`, ESLint 10 `>=24`).
- No physical mobile device is registered or connected. `adb` is not installed.
- Whether Codex auto-loads `.agents/skills/*` in this repository was not tested from the Codex side.

## 3. Readiness for Babylon.js + React + TypeScript + Havok

**Ready:** Node/npm versions satisfy current Vite 8, Vitest 5, ESLint 10 and TypeScript; two Chromium browsers with working WebGPU adapters and WebGL2; WASM available for Havok; ample disk on D:.

**Current registry versions (read-only `npm view`, 2026-11-17).** Pin exact versions at bootstrap. These were not installed:
`@babylonjs/core` 9.37.0 (74 MB unpacked; tree-shaking discipline needed), `@babylonjs/havok` 1.3.14 (MIT), `vite` 8.3.8, `react`/`react-dom` 19.3.1, `typescript` 7.0.2, `tailwindcss` + `@tailwindcss/vite` 4.3.5, `zustand` 5.1.0, `vitest` 5.1.0, `@playwright/test` 1.65.1, `eslint` 10.15.0, `@supabase/supabase-js` 2.119.0 (deferred).

> TypeScript 7.x is the new native compiler line. Confirm that the editor tooling and `typescript-eslint` support it at P01-01A. If they don't, pin the newest supported 6.x/5.x and record why.

## 4. Missing prerequisites and recommended actions

| # | Gap | Impact | Recommended action | Who |
| --- | --- | --- | --- | --- |
| 1 | Project is not a Git repository | Branch/worktree workflow, review diffs and rollback are impossible | Owner authorizes `git init`, a `.gitignore` (node_modules, dist, coverage, test-results) and a baseline commit of the planning docs before P01 work | Owner approval, then implementer |
| 2 | Third-party clone with its own `.git` inside `.agents/skills/bufatechno-webgamedev` | Becomes an embedded repo/gitlink on first commit; Codex may auto-load a Three.js-leaning skill (see §5) | Decide before `git init`: move it outside the repo (recommended) or add it to `.gitignore` | Owner |
| 3 | No touchscreen and no registered phone | Phase 01 exit gate needs real multitouch on hardware | Owner names at least one Android phone (model, Android and Chrome version) for device M1 | Owner |
| 4 | Phone testing needs a secure context for WebGPU | A phone opening `http://<LAN-IP>:5173` is **not** a secure context, so `navigator.gpu` is absent and it will silently use WebGL | Use `adb reverse tcp:5173 tcp:5173` (phone sees `localhost`, which is secure) or a local HTTPS dev certificate. Both need approval (Android platform-tools install or a dev-cert plugin) | Owner decision at P01-06B |
| 5 | Firefox not installed | No non-Chromium fallback check on desktop | Optional: install Firefox for the F1 fallback row, or force WebGL in Chrome via the planned `?renderer=webgl` override | Owner (optional) |
| 6 | `gh` CLI not installed | Only matters for GitHub PRs; no remote is planned yet | None now | — |
| 7 | pnpm/yarn absent | None | Use **npm** (already present) with a committed `package-lock.json` | — |

## 5. Skill audit

"Available to Claude Code" was verified against the skill list loaded in this session. "Codex" availability was inferred from file locations and `%USERPROFILE%\.codex\config.toml`.

| Skill | Location / version | Installed? | Readable | Supports | Babylon-compatible | Scripts / permissions | Conflicts | Recommendation |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| **motorcycle-physics** (project) | `.claude/skills/` and `.agents/skills/` (byte-identical) | Yes, loaded by Claude Code | Yes | Physics task policy; defers to `PHYSICS_AND_CALIBRATION.md` | Engine-neutral | None, docs only | None | **Activate** for P01-03B (units only) and all of Phase 02 |
| **bufatechno-webgamedev** | Full git clone at `.agents/skills/bufatechno-webgamedev`, commit `9f532a8` (2026-09-06), v2.3.0, MIT | **Not available to Claude Code** (not in `.claude/skills`). Probably auto-discoverable by Codex (project `.agents/skills`), unverified | Yes (400-line SKILL.md + 19 references, ~31 KB entry) | Generic Three.js/Babylon game scaffolding, ECS, VFX, input, perf | Partly. A Babylon reference exists, but **the default decision tree picks Three.js** ("When silent: Three.js WebGPURenderer"), scaffolds plain JS with CDN import maps, and pins Babylon 8.15 (current 9.37) | Scaffold scripts write files with `fs`; `sync-plugin.js` deletes inside its own folder; tests run `npm install` and network requests when env flags are set; includes a cache-first service worker and donation images. Nothing malicious found in a grep review; **none were executed** | **Yes**: Three.js default, CDN loading, broad auto-trigger ("Prefer over generic web guidance"), very large context cost | **Do not activate.** Remove it from the repository. If wanted, keep `references/babylonjs-complete.md`, `input-controls.md` and `performance-optimization.md` as manually consulted, non-authoritative notes. Never run its scaffold scripts |
| **frontend-design** (Anthropic) | `%USERPROFILE%\.claude\skills\frontend-design` (also in `~/.agents/skills`) with LICENSE | Yes, loaded by Claude Code | Yes | Visual direction for UI | Yes: React/Tailwind menus and HUD; not 3D | None (SKILL.md + LICENSE only) | None | Optional in Phase 01 (minimal HUD); **activate in Phases 06/08** |
| **game-playtest** (OpenAI game-studio) | Only in Codex's marketplace temp clone `~/.codex/.tmp/plugins/plugins/game-studio`, commit `2f1a894` (2026-07-08) | **Not installed**: not in Codex `config.toml` plugins, not available to Claude Code | Yes | Browser game smoke/playtest checklist; prefers Playwright and screenshots | Yes, engine-agnostic. Siblings in the plugin (`three-webgl-game`, `react-three-fiber-game`) are Three.js | Not inspected beyond SKILL.md; installing the whole plugin brings Three.js siblings | Mild, via the siblings | **Codex reviewer may enable it for P01-06** with owner OK. Its checklist is already reflected in the Phase 01 test plan |
| **threejs-\*** suite (10 skills) | `%USERPROFILE%\.claude\skills` and `~/.agents/skills`, added 2026-11-17 | Yes, loaded by Claude Code at **user level** | Yes | Three.js gameplay, graphics, QA; asset generation via paid external APIs (Tripo, Gemini, ElevenLabs) | **No**, Three.js-specific | Asset skills call paid third-party APIs | **Yes**, they auto-trigger on "build/ship a browser game" | **Do not use for this project.** Asset-generation skills need separate licensing and cost approval (Phase 04) |
| Others (`ui-ux-pro-max`, `design-taste-frontend`, `apple-design`, `web-design-guidelines`, `graft`; plugins `ponytail`, `hostinger`) | User level | Yes | — | General UI or unrelated | n/a | `hostinger` can deploy | Deployment is forbidden without approval | `web-design-guidelines` is useful for HUD/accessibility review in Phase 06. Never use deploy plugins without owner approval |

**Activate before Phase 01:** `motorcycle-physics` (already active). Keep `frontend-design` available. Nothing else is required. The Codex reviewer may add `game-playtest` for P01-06.
