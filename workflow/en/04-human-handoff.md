# Workflow 4: Human Handoff

> Scenario: work must be completed by a human, or an agent finds a confusion, choice, or risk that significantly affects direction, scope, architecture, or acceptance criteria.

## Trigger Judgment

Move the following to `HUMAN.md`:

- Human-only execution, such as real account login, external permissions, physical devices, or production/staging manual validation.
- Human decisions that affect product positioning, feature boundaries, architecture choices, external interface commitments, or acceptance criteria.
- Agent confusion or direction risk where guessing would cause meaningful drift.

If the item can be completed through code, docs, tests, or a simulated environment and does not affect a direction-level decision, keep it in `TODO.md`.

## Handoff Steps

1. Decide whether the item blocks concrete execution.
2. If it blocks execution, first create or confirm a corresponding `TODO.md` item and keep it unchecked.
3. Add a `H-YYYYMMDD-01` item in `HUMAN.md`, with type: `human execution`, `human decision`, `agent confusion`, or `direction risk`.
4. State the impact scope, what the agent pauses, and what the agent can still continue.
5. If a TODO exists, write: `Blocked: moved to HUMAN.md#H-..., awaiting human execution/decision`.
6. If the source is fix or fix-plan, mark that item as moved to `HUMAN.md#H-...`.
7. Avoid the affected scope and continue independent TODO work.

## HUMAN Item Fields

- Source
- Corresponding TODO, if applicable
- Type
- Status
- Impact scope
- What the agent pauses
- What the agent can continue
- Why human input is required
- Preconditions, for human execution
- Steps to execute or questions to answer
- Expected result or decision effect
- Feedback format

## After Human Feedback

- Passed / executed: mark corresponding TODO `[x]`.
- Decision made: resume affected TODO, plan, fix, or fix-plan work according to the feedback.
- Failed / not accepted: create or update `fix_<desc>.md` for defects, or `fix-plan_<desc>.md` for scope expansion, then map back to TODO.
- No longer needed / canceled: mark TODO `[~]` with reason; unbound HUMAN items may be marked canceled.
- Completion details may move to `.cairn/archive/human-YYYYMMDD.md`.
