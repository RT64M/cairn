# Design Overview

> Cairn is a file-based protocol for humans and AI agents. Its core idea is to move development context out of chat and memory into files.
> This folder explains why each Markdown file exists, where its boundary is, and how the files constrain one another.

## File Layers

### Required Meta Files (4)

| File | Core Question | Design Purpose |
| --- | --- | --- |
| `.cairn/plan.md` | Why does this project exist, and what should it become? | Lock the stable outline and prevent design drift |
| `.cairn/ARCHITECTURE.md` | How is project structure layered, how does information flow, and how is code organized? | Lock file ownership, information flow, and code structure across sessions |
| `AGENTS.md` | How should agents execute the protocol after entering the project? | Provide executable collaboration and sync rules |
| `.cairn/TODO.md` | What is being done, where is it blocked, and why? | Central execution ledger |

### Conditional Supplements

| File | Trigger | Design Purpose |
| --- | --- | --- |
| `.cairn/fix_<desc>.md` | Review, test feedback, or plan re-check | Give defect feedback its own lifecycle and correct plan-description drift on archive |
| `.cairn/fix-plan_<desc>.md` | New feature, new module, acceptance scope, or plan-item revision | Confirm plan changes before writing `plan.md`, annotate source/date, and archive first |
| `.cairn/HUMAN.md` | Agent cannot complete work, or human decision prevents direction drift | Move human execution, confusion, and direction risks out of fix/TODO |
| `.cairn/INTERFACE.md` | Project exposes external interfaces | Keep interface contracts synced with implementation |
| `.cairn/TEST.md` | Project is complex and user explicitly asks for testing | Record test methods without mixing in bug feedback |
| `.cairn/NICKNAME.md` | Project terminology reaches 5+ terms | Ensure terminology is understood before other files |
| `.cairn/archive/` | TODO / fix / fix-plan / HUMAN reaches closure | Preserve history while keeping active files clean |

## Information Flow

```text
AGENTS.md          -> bootstrap protocol for entry, sync, and file updates
.cairn/NICKNAME.md -> explain terminology early, if present
.cairn/ARCHITECTURE.md -> project structure, file ownership, information flow, and code organization
.cairn/fix_<desc>.md -> defect feedback enters TODO first
.cairn/fix-plan_<desc>.md -> plan changes are confirmed, written, annotated, archived, then split into TODO
.cairn/HUMAN.md    -> agent-blocked or human-decision work goes to humans
.cairn/TODO.md     -> execution hub connecting all changes
.cairn/plan.md     -> written only through fix archive or fix-plan final confirmation
.cairn/archive/    -> closed history
```

## Design Principles

- **Single responsibility**: each piece of information has exactly one owner file.
- **Owned workspace**: only `AGENTS.md` lives at the project root; every other Cairn protocol file lives under `.cairn/`.
- **Architecture owns structure**: project file layout, information ownership, information flow, and code organization all live in `ARCHITECTURE.md`.
- **Record before execution**: code or documentation work must first enter `TODO.md` from `plan.md` or `fix_<desc>.md`.
- **Feedback has an entry point**: review, testing, and manual feedback enter semantically named `fix_<desc>.md`.
- **Plan changes have an entry point**: additions, extensions, and revisions enter `fix-plan_<desc>.md`; after final confirmation, they are written to `plan.md`, annotated, archived, and then split into TODO.
- **TODO does not record commands themselves**: user bootstrap commands, current conversations, and sidecar-agent notes may trigger plan / fix flow, but cannot directly become TODO sources.
- **External agents can intervene asynchronously**: other agents may create fix, fix-plan, or conditional meta files; the main agent detects those changes in read order and continues.
- **Human work has an exit**: work agents cannot do or should not decide enters `HUMAN.md`; the affected scope pauses while independent TODOs continue.
- **Interfaces change with code**: external interface signature changes must update `INTERFACE.md`.
- **Test methods and test results are separate**: `TEST.md` says how to test; `fix_<desc>.md` records what testing found.

The principles above are *how* the split works. The two below are *why it is worth doing* — they are the goals every constraint above serves:

- **Bounded and loadable in one pass**: the number of file types is capped (4 required plus at most 6 conditional), and closed batches always move out to `.cairn/archive/`. The active set therefore does not grow with project age, so any new session or new agent completes handover by reading the current state once, without digging through chat history or commit logs.
- **Every decision stays traceable**: the single source marker in `plan.md`, the four-part header on every fix / fix-plan batch (content, time, source, scope), the source field on TODO items, the stated reason on `[!]` / `[~]`, and the preserved batches in `archive/` together form one chain that is walkable from either end — from any change back to the discussion that authorized it, and from any plan item forward to when it was written and revised. Rejected approaches stay on that chain too, precisely because they produce no commit and git cannot record them.

## Detailed Design

- [AGENTS.md](agents.md)
- [plan.md](plan.md)
- [ARCHITECTURE.md](architecture.md)
- [TODO.md](todo.md)
- [fix_<desc>.md](fix.md)
- [fix-plan_<desc>.md](fix-plan.md)
- [HUMAN.md](human.md)
- [INTERFACE.md](interface.md)
- [TEST.md](test.md)
- [NICKNAME.md](nickname.md)
- [archive/](archive.md)
