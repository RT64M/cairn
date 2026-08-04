# .cairn/archive/ Design

`.cairn/archive/` stores closed TODO steps, fix batches, fix-plan batches, and human handoff history.

## Why It Exists

Active files should not grow forever, but history should not be deleted. Archiving keeps the current state readable while preserving deprecated `[~]` items, modified `[!]` routes, and feedback-closure records.

## Archive Types

| File | Source |
| --- | --- |
| `.cairn/archive/done-YYYYMMDD.md` | TODO steps whose items have completed or been deprecated |
| `.cairn/archive/fix_desc-YYYYMMDD.md` | Closed fix batches whose plan impact has been handled |
| `.cairn/archive/fix-plan_desc-YYYYMMDD.md` | fix-plan batches already written into `plan.md` with source and date |
| `.cairn/archive/human-YYYYMMDD.md` | Completed or closed human execution, decision, or direction-risk details |

## Rules

- Archiving moves content; it does not delete it.
- Archive titles preserve the original title and add the archive date.
- When a TODO step meets archive conditions, it must be archived in the same session. Archiving is part of completion, not optional cleanup.
- `fix-plan` archives immediately after `plan.md` has been updated with source and date annotations; it does not wait for implementation TODOs.
- Later TODO entries track implementation derived from the new plan and do not block the already-closed fix-plan archive.
- Multiple TODO archive batches on the same day may use `-2.md`, `-3.md`, etc.
- TODO keeps a breadcrumb in the original location.
- Steps containing `[ ]`, `[!]`, or blockers must not be archived.
