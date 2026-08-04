# TODO.md Design

`TODO.md` is the execution ledger. It records what is being worked on, what is complete, what is blocked, and where each item came from.

## Why It Exists

Agent execution spans sessions, people, and tools. `TODO.md` makes current state independent of chat context and gives every change a source and acceptance point.

## Status Symbols

- `[x]` done.
- `[ ]` pending.
- `[!]` modified: the original item remains partially valid but is superseded by a newer plan.
- `[~]` deprecated: the original item is canceled or replaced, but kept for traceability.
- `Blocked:` records the specific reason and next recovery step.

## Should Include

- Step numbers, titles, and sources.
- Executable subtasks.
- Verification commands or acceptance results.
- Feedback mapped from `fix_<desc>.md`.
- Execution steps derived from confirmed, written, and archived `fix-plan_<desc>.md`.
- Treatment of old items affected by plan revision, such as `[!]`, `[~]`, reopened TODOs, or archive notes.
- Human blockers pointing to `HUMAN.md`.
- Whether interface changes have synced `INTERFACE.md`.

## Should Not Include

- Stable design motivation, which belongs in `plan.md`.
- Project structure, file ownership, information flow, and code directory ownership, which belong in `ARCHITECTURE.md`.
- Test method index, which belongs in `TEST.md`.
- Raw user commands, current-conversation notes, or bootstrap actions such as "make a plan from AGENTS"; they trigger plan / fix flow but are not TODO sources.
- Full completed history, which belongs in `.cairn/archive/`.

## Source Boundary

TODO sources are limited to two categories: execution steps derived from the current `plan.md`, or defect / review / test feedback mapped from `fix_<desc>.md`. `fix-plan_<desc>.md` is written into and archived through `plan.md` before TODO is derived, so TODO does not directly cite fix-plan, user commands, or the current conversation as its source.

## Archive

Archiving is part of TODO completion, not optional cleanup. After every TODO update, agents must check archive conditions: when all items under a step have converged to `[x]` or `[~]` and there are no `[ ]`, `[!]`, or blocked items, the step must move to `.cairn/archive/done-YYYYMMDD.md` in the same session, leaving only a breadcrumb in TODO.

Steps still referenced by active items are not archived until the references close. If a later `fix-plan_<desc>.md` revision affects archived TODO, inspect the matching archive before adding notes or reopening work.
