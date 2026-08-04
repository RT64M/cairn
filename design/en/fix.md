# fix_<desc>.md Design

`fix_<desc>.md` is the closure file for one batch of test, review, or manual feedback. `desc` must be semantic, such as `fix_audit-batch.md` or `fix_mobile-layout.md`.

## Why It Exists

Feedback mixed directly into TODO becomes hard to distinguish from the original plan. Feedback left only in chat loses its source and closure state. `fix_<desc>.md` gives each defect-feedback batch its own lifecycle. It does not handle plan scope additions or revisions; those go through `fix-plan_<desc>.md`.

`fix_<desc>.md` is also one entry point for asynchronous external-agent intervention. A human may ask another agent to review direction, implementation, or documentation consistency while the main agent keeps working. That agent can create `fix_<desc>.md`, `fix-plan_<desc>.md`, or another conditional meta file. The main agent does not need to stop; it detects unarchived fix, fix-plan, and TODO changes on each sync.

## Creation Conditions

Create only when:

- The user asks for code review, security audit, or plan re-check.
- The user provides concrete test feedback, E2E failure details, manual QA bugs, or an external-agent report.

Agents must not log their own incidental self-review findings into fix unless the user explicitly asked for self-review.

## Should Include

- Batch content, time, source, and scope.
- Each feedback item's observed behavior, expected behavior, and status.
- Corresponding TODO entries.
- Whether the item needs `HUMAN.md`.
- Impact on `plan.md` before archive.

## Lifecycle

1. Create a semantically named `fix_<desc>.md`.
2. Map actionable items into `TODO.md`.
3. Let agents complete the fixes they can do.
4. Move human-only work into `HUMAN.md`.
5. Once all items converge, write any plan impact back to `plan.md`.
6. Archive as `.cairn/archive/fix_desc-YYYYMMDD.md`.

## Relationship With Continuous Main-Agent Work

- External agents may add or update `fix_<desc>.md` while the main agent works.
- If the feedback changes plan scope, it belongs in `fix-plan_<desc>.md`, not fix.
- The main agent reads unarchived fix and fix-plan files before TODO at sync points, so it can absorb new feedback without losing context.
- After discovering a new fix, the main agent maps actionable items into TODO before continuing ordinary TODO work.
