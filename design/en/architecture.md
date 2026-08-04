# ARCHITECTURE.md Design

`ARCHITECTURE.md` records project structure. It answers: how should Cairn files divide ownership, how does information flow, where should code go, and how should modules depend on each other?

## Why It Exists

When agents work on a project over many sessions, both project structure and code structure can drift toward whatever location is convenient in the moment. `ARCHITECTURE.md` gives Cairn file ownership, information flow, new code, refactors, and module dependencies a stable reference point.

## Should Include

- Cairn workspace layout, including the structural relationship between root `AGENTS.md` and files under `.cairn/`.
- Which information each meta file owns: plan, fix, fix-plan, HUMAN, TODO, INTERFACE, TEST, NICKNAME, and archive.
- High-level information flow between planning, feedback, human handoff, execution ledger, and archive.
- Project source top-level directories and key subdirectory responsibilities.
- Rules for placing new code.
- Module dependency direction.
- Startup entrypoints and initialization order.
- Core call chains.
- Extension points and naming conventions.

## Should Not Include

- Concrete algorithm implementation.
- Interface parameters and response shapes, which belong in `INTERFACE.md`.
- Project goals and acceptance criteria, which belong in `plan.md`.
- Agent execution procedure, status symbols, read order, and sync constraints, which belong in `AGENTS.md`.
- Current refactor tasks, which belong in `TODO.md`.

## Change Path

Normal feature work, refactors, and bug fixes do not automatically change architecture. Update `ARCHITECTURE.md` only when project structure, file ownership, information flow, or code ownership has no reasonable home under the current architecture, or when the user explicitly asks for an architecture change. Note the architecture sync in the corresponding TODO item.
