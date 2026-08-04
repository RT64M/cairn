# README.md Design

`README.md` is the user-facing entry point. It explains what the project is, how to use it, how to configure it, and what its limits are.

## Why It Exists

Users should not need to read agent collaboration rules, internal TODOs, or review records to use a project. README separates the user view from the development view.

## Should Include

- Project purpose.
- Installation, opening, or running flow.
- Typical usage.
- Configuration.
- Privacy, limitations, and FAQ.
- User-visible capability changes.

## Should Not Include

- Agent read order and status conventions, which belong in `AGENTS.md`.
- Current task progress, which belongs in `TODO.md`.
- Design outline, which belongs in `plan.md`.
- Test command indexes, which belong in `TEST.md`.
- Review or bug feedback, which belongs in `fix_<desc>.md`.

## Difference From This Repository's README

The root `README.md` in this repository explains the Cairn protocol and the top-level folders. A downstream project's `README.md` should explain that project itself.
