# fix-plan_<desc>.md Design

`fix-plan_<desc>.md` is the closure file for adding, extending, or revising the plan. It handles changes to the plan itself, not defects in already implemented work.

## Why It Exists

If new features, scope changes, or plan revisions go straight into `TODO.md`, the project outline bypasses confirmation. If they go straight into `plan.md`, the stable design drifts during ordinary sessions. `fix-plan_<desc>.md` makes plan changes confirmable, traceable, and quick to archive.

## Creation Conditions

Create it only when the user explicitly asks for:

- A new feature or module outside the current plan.
- A change, replacement, or deprecation of an existing `plan.md` item.
- An expansion of acceptance criteria, feature scope, or project boundaries.

Bug fixes, test feedback, and review issues go to `fix_<desc>.md`, not `fix-plan_<desc>.md`.

## Should Include

- Batch content, time, and source.
- Expected impact on `plan.md`.
- Plan changes explicitly requested by the user.
- Agent suggestions that the user approved item by item.
- Final confirmation status.

## Should Not Include

- Bugs or review items in already implemented work, which belong in `fix_<desc>.md`.
- Ordinary execution steps, which belong in `TODO.md`.
- Interface parameter details, which belong in `INTERFACE.md`.
- Post-implementation history, which belongs in `.cairn/archive/`.

## Lifecycle

1. Create a semantically named `fix-plan_<desc>.md`.
2. Discuss with the user and converge the draft under two layers: explicit user requirements first, agent suggestions only with item-by-item approval.
3. After final user confirmation, immediately write the accepted content into `plan.md`.
4. Annotate each new or revised plan item with source and modification date, as a single consolidated marker — replace, never append; keep at most the original source plus the latest revision.
5. If completed TODOs are affected, inspect `TODO.md`; if already archived, inspect the matching `.cairn/archive/done-YYYYMMDD.md`, then decide whether to reopen, annotate, or add a review TODO.
6. After `plan.md` updates and source annotations are complete, archive as `.cairn/archive/fix-plan_desc-YYYYMMDD.md`.
7. Derive or adjust `TODO.md` from the updated plan, then proceed to implementation and verification.

## Relationships

- With `fix_<desc>.md`: fix corrects implementation or plan-description divergence; fix-plan changes the plan itself.
- With `plan.md`: fix-plan is the entry point for adding, extending, or revising plan content. Ordinary sessions must not bypass it.
- With `TODO.md`: TODO derives from confirmed plan content; it does not replace fix-plan confirmation and archiving.
- With `.cairn/archive/`: fix-plan archive does not wait for downstream implementation; it only closes the plan rewrite loop.
