# Workflow 3: `fix_<desc>.md` Lifecycle

> Scenario: the user gives review feedback, test failure details, manual QA bugs, or asks for review / plan re-check.
> Scope: only for fixes to already completed content. If the user proposes a new feature or module outside the plan, use [05-plan-revision.md](05-plan-revision.md).

## Boundary With fix-plan

| Trigger | Use `fix_<desc>.md` | Use `fix-plan_<desc>.md` |
| --- | --- | --- |
| Implementation bug or regression | Yes | No |
| Review or security audit issue in current work | Yes | No |
| Actual deviation found by testing | Yes | No |
| User proposes a feature or module outside the plan | No | Yes |
| User expands acceptance criteria or feature scope | No | Yes |

If both types appear in the same conversation, create two separate files.

## Create

Use semantic names, for example:

- `fix_mobile-layout.md`
- `fix_api-contract.md`
- `fix_security-review.md`

Do not use `fix_01.md` for new files.

The file header should record:

- Content
- Time
- Source
- Scope

## Map To TODO

Every feedback item must map to `TODO.md`:

- Agent can fix it: create a concrete TODO subitem.
- No longer valid: mark TODO `[~]` with the reason.
- Superseded by a new approach: mark TODO `[!]` and point to the new item.
- Needs human execution or decision: move to `HUMAN.md`; if it blocks concrete execution, create TODO first and point the blocker to `HUMAN.md`.

## Progress

`fix_*` has the highest priority, above `fix-plan_*` and ordinary TODO. If any unarchived fix has unresolved agent work, handle it before regular TODO.

## Archive

When every item in the batch is done, deprecated, or moved to human handoff, and no agent-executable work remains:

1. Summarize impact on `plan.md`.
2. Write back to `plan.md` only if current state diverges from the plan description. Pure fixes usually need no plan update.
3. Move to `.cairn/archive/fix_desc-YYYYMMDD.md`, preserving the semantic desc.
4. Mark corresponding TODO items complete or keep human blockers.

## Relationship With Plan Revision

`fix_<desc>.md` is not an entry point for adding plan content. It only corrects description drift on archive when needed. New or expanded plan scope must use `fix-plan_<desc>.md`.

## Anti-Patterns

- Putting a new feature into `fix_<desc>.md`.
- Combining defect feedback and new features in one file because they came from one conversation.
- Agents putting incidental self-review notes into fix without being asked.
- Merging unrelated feedback batches.
- Checking items in fix without syncing TODO.
- Leaving manual validation or direction decisions inside fix forever.
