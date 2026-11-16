# Phase 09 Execution Specification | Optional Connected Profiles and Progression

**Status:** NOT STARTED. Requires previous dependency gates and explicit user authorization.

## Player / engineering outcome

Safe server-validated ownership and persistence without forcing accounts for basic riding.

## Proposed bounded tasks

- P09-01: Approve data model, privacy policy and backend operating costs.
- P09-02: Build Supabase auth and least-privilege RLS.
- P09-03: Add user settings, owned motorcycles, and result provenance.
- P09-04: Add transactional game economy logic and idempotency as needed.
- P09-05: Test cross-user access, replayed transactions, migrations and rollback.

## Acceptance evidence

- [ ] Access policies reject unauthorized reads and writes.
- [ ] Client cannot mint currency, assets or verified race results.
- [ ] Offline-first race functionality remains usable.
- [ ] Database rollout and backups explicitly approved.

## Explicit exclusions

Using client-only economy balances as authority, online racing physics.

## Execution and review contract

One designated implementer owns each task and a separate agent reviews. Record tested device details, commands, real results, touched paths, skill use, risks and signed acceptance in `docs/PROGRESS.md`. See `docs/MASTER_PLAN.md`, `docs/QA_AND_RELEASE_GATES.md` and the relevant specialist notes.
