# Phase 08 Execution Specification | Garage, Settings and Single-Player Release Candidate

**Status:** NOT STARTED. Requires previous dependency gates and explicit user authorization.

## Player / engineering outcome

A coherent full single-player release that is accessible, legally distributable and tested on target hardware.

## Proposed bounded tasks

- P08-01: Build garage browse/select and a clear motorcycle specification panel.
- P08-02: Model sourced bike price metadata with market, year, date and evidence.
- P08-03: Implement functional control, graphics, camera and audio settings.
- P08-04: Improve onboarding, recoverable errors and critical player journey.
- P08-05: Run full device/browser/asset-license/security QA and triage issues.

## Acceptance evidence

- [ ] Launch -> select -> ride -> finish -> result -> restart works.
- [ ] At least one polished playable bike and target three variants if legally feasible.
- [ ] All publicly displayed price/spec claims are sourced or explicitly marked estimates.
- [ ] No unresolved P0/P1 issues in defined core journey; performance and accessibility reviewed.
- [ ] Owner accepts release candidate before deployment.

## Explicit exclusions

Online account dependency, real-money shop, authoritative multiplayer.

## Execution and review contract

One designated implementer owns each task and a separate agent reviews. Record tested device details, commands, real results, touched paths, skill use, risks and signed acceptance in `docs/PROGRESS.md`. See `docs/MASTER_PLAN.md`, `docs/QA_AND_RELEASE_GATES.md` and the relevant specialist notes.
