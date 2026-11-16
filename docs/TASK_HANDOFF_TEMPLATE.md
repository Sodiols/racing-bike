# Bounded Task Handoff Template

Use this **only after the owner authorizes a phase**. Fill concrete details rather than pasting the entire master roadmap into both agents.

```text
Task ID: PXX-YY
Milestone: PHASE_XX.md
Owner approval and scope:
Implementer: Claude Code or Codex
Reviewer: The other agent
Branch/worktree:
Read only these docs:
Relevant skill files actually installed:
Goal and player-observable behavior:
In scope:
Out of scope:
Dependencies:
Implementation acceptance criteria:
Test commands and real-browser checks:
Needed capture/log/device information:
Known risks and fallback:
No push/deploy/paid services without approval.
```

## Reviewer checklist

- Did the implementer stay in the approved scope?
- Is this a real feature rather than UI-only imitation?
- Are simulation units and frame independence preserved?
- Are inputs and mobile behavior correct for the approved milestone?
- Were tests actually run, and are results reproducible?
- Are licenses, data sources and privacy boundaries respected?
- Are new dependencies justified and reviewed?
- Is there a clear rollback and correct progress-log update?

## Owner phase acceptance

Record approved commit, demonstrated scenario, named devices, quantitative evidence if claimed, unresolved issues and explicit approval in `docs/PROGRESS.md`. If not approved, keep phase `IN REVIEW` or `BLOCKED`.
