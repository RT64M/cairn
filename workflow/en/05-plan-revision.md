# Workflow 5: `fix-plan_<desc>.md` Lifecycle

> Scenario: the user proposes a feature or module outside the plan, expands acceptance criteria, changes feature scope, or revises an existing plan item.
> Scope: only plan expansion or revision. For defects in completed content, use [03-fix-lifecycle.md](03-fix-lifecycle.md).

## Principle

Ordinary sessions must not edit `plan.md` directly except for obvious typos or broken links. Any addition, expansion, or revision to the plan must go through `fix-plan_<desc>.md`, user discussion, and final confirmation before entering `plan.md`. The same bootstrap principle applies: **open exploration during discussion, strict containment when committing.**

## Boundary With fix

`fix-plan_<desc>.md` and `fix_<desc>.md` look similar but mean different things:

- `fix_<desc>.md`: fixes defects in completed content; may correct plan-description drift on archive.
- `fix-plan_<desc>.md`: changes the plan itself; after final user confirmation, writes into `plan.md` immediately with source and date.

If both appear in one conversation, create separate files. The filename prefix identifies the batch type.

## Create

Use semantic names, for example:

- `fix-plan_audit-log.md`
- `fix-plan_offline-mode.md`
- `fix-plan_billing-extension.md`

Do not use pure numeric names.

The file header should record:

- Content
- Time
- Source
- Expected impact on `plan.md`, including sections to add or revise

## Steps

### 1. Discussion Phase

Discuss:

- Purpose, target users, and core behavior.
- Impact on existing features, data model, and external interfaces.
- Edge cases and risks.
- Acceptance criteria.

The agent may propose related ideas, risks, and options. Every uncertainty must be asked and confirmed; do not silently fill guesses.

### 2. Commit Phase

When writing `fix-plan_<desc>.md`, split content into:

- **User explicit requirements**: include everything the user clearly requested.
- **Agent suggestions**: include only items the user explicitly approved. Unapproved suggestions stay in the discussion record and do not enter the fix-plan file.

### 3. Final Confirmation

After the draft is complete, the user must give final confirmation, such as "fix-plan finalized".

### 4. Write `plan.md`

After final confirmation, immediately write the accepted content into the relevant `plan.md` sections. This is the only entry point for adding, expanding, or revising plan content.

### 5. Annotate Source

Every plan item added or revised by this fix-plan must include:

- Source: `fix-plan_<desc>.md`, or archived `.cairn/archive/fix-plan_desc-YYYYMMDD.md`
- Modification date: at least `YYYY-MM-DD`; include time and timezone when known

### 6. Check Impact On Completed TODO

If this plan change covers, changes, or deprecates completed TODO:

1. Check the corresponding current TODO section.
2. If archived, read the matching `.cairn/archive/done-YYYYMMDD.md`.
3. Decide whether old archives need review, notes, or reopened TODO.

### 7. Archive fix-plan First

After `plan.md` updates and source annotations are complete, move `fix-plan_<desc>.md` to `.cairn/archive/fix-plan_desc-YYYYMMDD.md`.

fix-plan closes the plan rewrite loop only; it does not wait for downstream implementation TODOs.

### 8. Split TODO

Derive TODO steps from the updated `plan.md`. If completed TODOs are affected, add TODOs, mark old items `[!]` or `[~]`, or add archive notes according to step 6.

### 9. Implement And Verify

Proceed using ordinary iteration: [02-iteration-cycle.md](02-iteration-cycle.md).

## Plan Write Permission Comparison

| Entry | When it writes plan | Nature of plan change |
| --- | --- | --- |
| `fix_<desc>.md` | On archive, as needed | Corrects description drift |
| `fix-plan_<desc>.md` | Immediately after final user confirmation | Adds, expands, or revises plan content |

## What May Directly Change plan

- Obvious typos.
- Broken links.
- User explicitly says to directly fix a typo in plan.

Everything else, even a single acceptance-standard sentence or field rename, should go through `fix-plan_<desc>.md`.

## Anti-Patterns

- Putting new features into `fix_<desc>.md`.
- Combining defect feedback and new features in one file.
- Skipping discussion and drafting fix-plan from one sentence.
- Writing fix-plan content into `plan.md` without final user confirmation.
- Writing agent suggestions into fix-plan or plan without user approval.
- Waiting for implementation TODOs before archiving fix-plan.
- Making major plan changes during fix-plan archive; plan should already have been synced after final confirmation.
