# Risk Register and Decisions Requiring Approval

**State:** Pre-development. All decisions in this document are proposals unless marked approved.

## Provisional technical decisions

| ID | Decision | Proposal | Approval |
| --- | --- | --- | --- |
| D01 | Runtime | Vite + TypeScript + Babylon.js with React overlay | Pending |
| D02 | Rendering | WebGPU with automatic WebGL fallback | Pending |
| D03 | Collision/physics | Havok V2 + custom motorcycle dynamics and controlled assists | Pending |
| D04 | State | Simulation kept independent of React; Zustand only for app/UI | Pending |
| D05 | Release scope | Single-player time trial before AI, garage and multiplayer | Pending |
| D06 | Market price region | Bangladesh/BDT as initial price display region | Pending |
| D07 | Content IP | Original or explicitly licensed 3D bikes and audio | Pending |
| D08 | Backend | No Supabase until optional account phase | Pending |
| D09 | Agents | Claude Code and Codex use one-writer/one-reviewer handoff | Pending |
| D10 | Simulation clock | Runtime-owned fixed-step clock; Havok coupling proven by spike | Pending (ADR-002) |
| D11 | Bike dynamics | Single chassis body + custom pure-TS tire/drivetrain core, target-lean assist | Pending (ADR-003) |

D01, D02 and D04 are detailed in `decisions/ADR_001_core_runtime_stack.md`. Evidence: `docs/PHASE_00_TECHNICAL_ASSESSMENT.md`.

## Risk register

| ID | Risk / failure mode | Likelihood | Impact | Mitigation / evidence needed | Phase |
| --- | --- | --- | --- | --- | --- |
| R01 | True 2-wheel handling is substantially more difficult than arcade steering | High | High | Calibrated simplified model, assists, test maneuvers | 02 |
| R02 | Performance and thermal throttling on budget mobile | High | High | Mobile vertical slice, quality tiers, frame-time logs | 01/05 |
| R03 | Ultra graphics claim incompatible with some browsers | High | High | WebGL fallback, adaptive quality, explicit compatibility | 01/05 |
| R04 | Real brand model/logos create licensing exposure | Medium | High | Asset ledger, original alternatives, legal review | 04/08 |
| R05 | Engine sounds need licensed samples and RPM mapping | Medium | Medium | Recorded provenance and real audio tests | 06 |
| R06 | Two agents overwrite each other's changes | High | High | Single-writer ownership, worktrees, independent review | All |
| R07 | Scope drifts toward open world/multiplayer before MVP | High | High | Phase gates and explicit non-goals | All |
| R08 | Physics tests pass but gameplay feels poor | Medium | High | Capture handling test routes and feedback | 02/03 |
| R09 | Regional motorcycle prices become stale/misleading | Medium | Medium | Source, market, model year, verified date fields | 08/09 |
| R10 | Unverified third-party skill executes unsafe code | Medium | High | Read-only inspection before install or execute | 00 |
| R11 | Browser audio blocked until interaction | High | Low | Gesture-init audio and exposed sound settings | 06 |
| R12 | Online economy or standings are client-controlled | Medium | High | Server authority and RLS in connected phase | 09/10 |
| R13 | Babylon Physics V2 steps inside the render loop (no public manual step); fixed-step coupling may drift with frame rate | Medium | High | ADR-002 spike P01-02A with 30/60/144 FPS sweep | 01 |
| R14 | Large Babylon bundle and WebGPU first-use pipeline hitches | Medium | Medium | Deep imports, bundle size log from P01-01A, material warm-up | 01/05 |
| R15 | Phone testing over LAN HTTP is not a secure context, so WebGPU silently falls back to WebGL | High | Medium | `adb reverse` or HTTPS dev cert; record backend in every test | 01 |
| R16 | Third-party or Three.js skills (repo-local clone, user-level suite) steer agents off-stack | Medium | Medium | Remove repo clone; AGENTS.md rule; skill changes need approval | 00+ |
| R17 | TypeScript 7 native compiler newer than lint/editor tooling | Medium | Low | Verify at P01-01A; pin the newest supported version otherwise | 01 |
| R18 | No Git repository and no registered mobile device yet | Certain | High | P01-00 owner-gated init; owner names device M1 | 00/01 |

## Questions to resolve during Phase 00

1. Product name and target audience: casual-realistic or simulation-heavy?
2. Minimum playable Android hardware and target browser list?
3. Starting location theme: fictional closed race circuit vs real location recreation?
4. Are real branded bikes essential, and is there a licensing/asset budget?
5. What are the primary target markets for display prices and currency?
6. Expected initial download size and device connection conditions?
7. Will the first release be free, paid, or ad supported? No monetization assumed.
8. Source asset creation pipeline and who can approve licensing?
9. Preferred hosting provider and owner for production deployment?

Answers are not prerequisites to **reading** the planning pack; they are approval decisions before committing implementation architecture or commercial assets.
