# AGENTS.md Design

`AGENTS.md` is the agent collaboration protocol. It is not for end users; it tells agents what to read, what to create if missing, and when each file may be changed.

## Why It Exists

Without `AGENTS.md`, every agent has to rediscover the project rules: whether to read architecture or TODO first, where test failures go, how human-only work is handed off, and whether interface changes require documentation updates. `AGENTS.md` turns those decisions into explicit rules.

## Should Include

- Bootstrap and read protocol when entering a project.
- `TODO.md` status symbols.
- Lifecycles for `fix_<desc>.md` and `fix-plan_<desc>.md`, including plan source annotations and quick fix-plan archive.
- `HUMAN.md` handoff rules.
- Trigger and sync rules for `ARCHITECTURE.md`, `INTERFACE.md`, `TEST.md`, and `NICKNAME.md`.
- Cross-project execution constraints such as status sync and comments.

## Should Not Include

- User installation guides.
- Feature marketing or FAQ.
- Concrete business plans.
- Project structure, file ownership, information flow, or code organization.
- Current task progress.
- A specific review batch's issue list.

Those belong in project user docs, `plan.md`, `ARCHITECTURE.md`, `TODO.md`, `fix_<desc>.md`, or `fix-plan_<desc>.md`.

## Templates

- English template: [../../template/AGENTS.en.md](../../template/AGENTS.en.md)
- Chinese template: [../../template/AGENTS.zh.md](../../template/AGENTS.zh.md)

Downstream projects normally copy one template as root `AGENTS.md`, then append project-specific rules at the end.
