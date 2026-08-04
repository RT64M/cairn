# HUMAN.md Design

`HUMAN.md` is the asynchronous human intervention ledger. It records work agents cannot complete independently and decisions agents should not make because they affect project direction, scope, architecture, or acceptance criteria.

## Why It Exists

Real account login, external permissions, physical devices, and production-like manual validation cannot be completed by an agent alone. Some questions could be guessed, but would affect product direction, feature boundaries, architecture choices, or acceptance language. Those should not be silently decided by an agent.

`HUMAN.md` lets agents package work for humans while continuing other executable TODOs that do not depend on that input. It also keeps `fix_<desc>.md` and `fix-plan_<desc>.md` from being blocked forever by manual validation or decisions.

## Should Include

- Human item ID, such as `H-YYYYMMDD-01`.
- Type: `human execution`, `human decision`, `agent confusion`, or `direction risk`.
- Source and corresponding TODO when the item blocks concrete execution.
- Current status.
- Impact scope.
- What the agent pauses.
- What the agent can continue.
- Why human input is required.
- Steps, questions, or decision options for the human.
- Expected result or how the decision takes effect.
- Feedback format.

## Should Not Include

- Fixes agents can complete.
- Ordinary development TODOs.
- Test method indexes.
- User tutorials.
- Small implementation details that do not affect direction, scope, architecture, or acceptance criteria.

## Relationship With TODO

A `HUMAN.md` item that blocks concrete work must correspond to a `TODO.md` item. The TODO remains unchecked and points to `HUMAN.md#H-...`.

Decision-only or confusion-only items may initially reference a source file or conversation context. Once the decision blocks executable work, the agent must create or confirm the corresponding TODO and point it to the human item.

## Recommended Item Format

```md
## H-YYYYMMDD-01: Title

- Type: human execution / human decision / agent confusion / direction risk
- Source: TODO N.M / plan.md / fix-plan_x.md / current conversation
- Corresponding TODO: TODO N.M, if applicable
- Status: waiting for human execution / waiting for decision / feedback received / done / canceled
- Impact scope:
- Agent pauses:
- Agent can continue:
- Why human input is required:
- Human must execute or answer:
- Expected result or decision effect:
- Feedback format:
```
