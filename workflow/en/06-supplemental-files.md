# Workflow 6: Architecture, Interface, Test, And Terminology

> Scenario: a requirement triggers maintenance of `.cairn/ARCHITECTURE.md`, `.cairn/INTERFACE.md`, `.cairn/TEST.md`, or `.cairn/NICKNAME.md`.

## Architecture Changes

Only two triggers exist:

- Project structure, file ownership, information flow, or new code has no reasonable home under the current architecture.
- The user explicitly asks for architecture changes.

Action: update `.cairn/ARCHITECTURE.md`, and note "architecture synced" in the corresponding TODO.

## Interface Changes

When paths, methods, parameters, responses, error codes, CLI arguments, or SDK public method signatures change:

1. Sync `.cairn/INTERFACE.md`.
2. Note "INTERFACE.md synced" in the current TODO.
3. If the change is breaking, add a TODO subitem to notify callers.

## User Requests Testing

When the project is complex and the user explicitly asks for testing:

1. If `.cairn/TEST.md` does not exist, create it.
2. Record test categories, commands, data setup, and environment dependencies.
3. Run tests.
4. Problems found by testing go into `fix_<desc>.md`; do not write bug feedback into `TEST.md`.

## Terminology Growth

When project-specific terms reach 5 or more:

1. Create `.cairn/NICKNAME.md`.
2. Move terms from `AGENTS.md` or existing docs into it.
3. Future agents read `.cairn/NICKNAME.md` first.
4. Add new terms as they appear; ask the user when meaning is unclear.
