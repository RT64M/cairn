# NICKNAME.md Design

`NICKNAME.md` is the quick glossary for project-specific terminology, shorthand, and naming preferences.

## Why It Exists

The same word may have a project-specific meaning, and the same object may have old names, short names, or team aliases. If every agent guesses again, requirements, TODOs, and feedback can be misread. A glossary removes ambiguity before architecture and state files are read.

## Creation Condition

Create it when the project has at least 5 project-specific terms. With fewer terms, a small section in `AGENTS.md` is enough; move them to `NICKNAME.md` after crossing the threshold.

## Should Include

- Term.
- Short definition.
- Usage scenario or one example sentence.
- Old-name to new-name aliases.
- Nearby terms or abbreviations that should not be confused.

## Should Not Include

- General programming terminology.
- Industry-standard vocabulary.
- One-off temporary codenames whose context is already clear.

## Read Order

Agents read the root `AGENTS.md` first for bootstrap. If `.cairn/NICKNAME.md` exists, they read it next, before `.cairn/ARCHITECTURE.md`, fix, HUMAN, TODO, and other state files.
