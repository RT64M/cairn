# TEST.md Design

`TEST.md` is an index of test methods and procedures. It is not a bug list.

## Why It Exists

As a project grows, test commands, data setup, environment requirements, and coverage goals become hard to maintain from memory. `TEST.md` fixes the "how to test" knowledge in one place.

## Creation Conditions

Both must be true:

- The project is complex enough that testing needs separate documentation.
- The user explicitly asks the agent to test.

Small routine checks initiated by the agent do not create or update `TEST.md`.

## Should Include

- Test categories such as unit, integration, and E2E.
- Run commands.
- Test data setup.
- Coverage goals.
- Special environment dependencies.
- Boundaries between manual validation and automated tests.

## Should Not Include

- Concrete bug feedback, which belongs in `fix_<desc>.md`.
- Manual execution steps or human acceptance-boundary decisions, which belong in `HUMAN.md`.
- Current test task progress, which belongs in `TODO.md`.
