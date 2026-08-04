# Workflow 2: Daily Iteration

> Scenario: the project already has meta files, and the user asks to add a feature, fix a bug, update docs, or continue work.

## Agent Entry

Follow the default read order from `AGENTS.md`:

1. root `AGENTS.md`
2. `.cairn/NICKNAME.md`, if present
3. `.cairn/ARCHITECTURE.md`
4. All unarchived `.cairn/fix_<desc>.md`
5. All unarchived `.cairn/fix-plan_<desc>.md`
6. `.cairn/HUMAN.md`, if present
7. `.cairn/TODO.md`
8. `.cairn/plan.md`

## Execution Loop

1. Handle agent-executable items in unarchived `fix_*` first; then handle unarchived `fix-plan_*` plan rewrite closure.
2. Confirm the current work has a corresponding `TODO.md` item.
3. If not, return to the source layer first: derive existing-plan work from `plan.md`; create or update `fix_<desc>.md` before mapping defect, test, or review feedback to TODO; or run `fix-plan_<desc>.md`, write it into `plan.md`, archive it, and only then split TODO for new or revised plan scope. The raw user command itself must not be a TODO source.
4. Decide whether interface, architecture, test, or terminology files need to be read or updated.
5. Implement the change.
6. After verification, update TODO status.
7. If a TODO step meets archive conditions, move it into `.cairn/archive/` and leave a breadcrumb. Archiving is part of closure, not optional cleanup.

## Common Branches

- External interface signature changed: sync `INTERFACE.md` and note "INTERFACE.md synced" in TODO.
- Project structure, file ownership, information flow, or new code has no reasonable home: update `ARCHITECTURE.md` before continuing.
- User asks for testing: read or create `.cairn/TEST.md`, then follow it.
- Agent cannot complete an item, or a human decision affects direction, scope, architecture, or acceptance: move it to `HUMAN.md`. If it blocks execution, first confirm that the executable item already entered TODO from `plan.md` or `fix_<desc>.md`, then point the blocker to `HUMAN.md`.
- User gives test feedback or review: create a semantically named `fix_<desc>.md`, then map it to TODO.
- User proposes a feature, module, or plan revision outside the current plan: create `fix-plan_<desc>.md`, follow [05-plan-revision.md](05-plan-revision.md), write and annotate `plan.md`, archive fix-plan first, then split TODO.

## Done Criteria

Do not only say "done". The corresponding TODO must converge to `[x]` or `[~]`, verification or blocker reasons must be clear, and any archive-eligible step must be archived with a breadcrumb.
