# plan.md Design

`plan.md` is the stable project outline. It answers: what is this project doing, why, and what counts as done?

## Why It Exists

TODO changes frequently, user documentation is user-facing, and chat history disappears. `plan.md` preserves low-frequency design context so agents can still understand project direction across many sessions.

## Should Include

- Project positioning.
- Users and core scenarios.
- Feature list.
- Data model and core objects.
- Required interfaces or entrypoints.
- Acceptance criteria.
- Explicit non-goals.

## Should Not Include

- Current execution steps, which belong in `TODO.md`.
- Interface parameters and response details, which belong in `INTERFACE.md`.
- Cairn file structure, information ownership, information flow, code directories, and dependency direction, which belong in `ARCHITECTURE.md`.
- Test feedback and review items, which belong in `fix_<desc>.md`.

## Change Path

`plan.md` must not be casually edited during ordinary sessions. It has only two write paths:

- `fix_<desc>.md`: corrects divergence between current state and plan description, as needed during fix archive.
- `fix-plan_<desc>.md`: adds, extends, or revises plan content. After final user confirmation, the accepted content is written to `plan.md`, annotated with source and date, the fix-plan is archived first, and TODO is derived or adjusted from the updated plan.

The source annotation is a **single consolidated marker per item — replace, never append**. When revising an item that already carries a marker, rewrite that marker instead of appending a new citation, and keep at most two references (the original source and the latest revision). The full revision chain already lives in `archive/` and git history, so plan items must not accumulate a growing `source / revised-source` list.
