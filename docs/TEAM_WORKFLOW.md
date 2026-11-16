# Claude Code and Codex Collaboration Protocol

**Purpose:** Two AI coding agents, one coherent codebase and a reliable audit trail.

## The coordination model

The user is the product owner and final phase approver. Claude Code and Codex are engineering agents. For each task designate **exactly one implementer** and **one independent reviewer**. Either agent can take either role; choose deliberately rather than assigning permanent exclusive ownership.

Suggested starting model:

- **Claude Code:** primary implementer for the initial runtime, scene and input slice.
- **Codex:** independent reviewer for architecture, performance, correctness, physics and automated tests.
- **Alternate ownership** for later tasks after stabilizing work boundaries. Keep one writer per shared subsystem at a time.

These are proposals, not rules tied to agent capabilities. The user may swap roles.

## Branch discipline

`main` represents approved milestone state. Each bounded task uses a branch, for example `feature/p01-game-bootstrap`, `feature/p02-transmission`, or `review/p02-transmission` if review requires isolated edits.

Never have Claude and Codex simultaneously modify the same working tree. Use independent git worktrees or separate branches and serialized merges. Never allow both agents to force-push the same branch or overwrite uncommitted changes.

## Handoff protocol

1. Owner selects one approved milestone and one bounded task.
2. Implementer reads `AGENTS.md`, current `docs/PROGRESS.md`, exact milestone and relevant technical notes.
3. Implementer reports intent, scope and tests; makes changes; runs targeted tests; records evidence and creates a reviewable diff.
4. Reviewer receives branch/diff, acceptance criteria, logs and references. Reviewer does **not** independently rewrite unrelated systems.
5. Reviewer finds correctness/performance/security/license issues and marks each blocking or non-blocking.
6. Implementer resolves blocking findings; reviewer re-verifies as necessary.
7. Owner approves merge/phase gate. Record accepted status, linked commit and known exceptions in `docs/PROGRESS.md`.
8. After the phase closes, begin the next approved milestone.

## Shared context rules

- No reliance on agent chat memory for vital decisions. Write them to version-controlled documents.
- `AGENTS.md` is canonical; `CLAUDE.md` references it without duplicating conflicting instructions.
- `docs/PROGRESS.md` is the truth about completed work. If no evidence exists, status remains unverified.
- Keep each task prompt short. Cite relevant path(s) and task ID rather than pasting every planning file into the context window.
- When an agent changes architectural assumptions, add an ADR and request user review before implementation.

## Protect production and costs

Do not deploy, push, install unreviewed scripts, purchase assets, incur paid cloud spend, create public Supabase resources or alter DNS without user consent. Prefer local and reversible experiments first.

## Recommended communication format

```text
Task: P01-02, Keyboard+touch action abstraction
Agent role: Implementer (Claude Code)
Branch: feature/p01-input
Scope: input module, controls, unit tests only
Out of scope: physics refactor, menu redesign, backend
Acceptance: simultaneous throttle+steer in desktop and real touch check
Proof: test commands, short gameplay capture, known limitations
Review requested from: Codex
```

## Phase 00 clarifications (proposed 2026-11-17)

- **Prerequisite:** the project must be a Git repository (P01-00) before any branch or worktree rule can apply.
- **Single-writer ledger:** only the current task's implementer edits `docs/PROGRESS.md`. The reviewer reports findings as a review note to the owner (or a `review/` branch file). The implementer records the outcome.
- **Repo-local skills are shared context:** adding or changing anything under `.claude/skills` or `.agents/skills` needs owner approval, like a dependency.

## When parallel work becomes safe

Parallel development is acceptable only after ownership boundaries are stable and tasks touch non-overlapping files or use isolated worktrees. Examples: asset audits vs isolated test generation; HUD design exploration vs physics specification review. Integration still goes through one controlled merge queue.
