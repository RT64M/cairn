# Cairn Agent Collaboration Protocol

## Change Frequency Conventions

To avoid subjective wording, every "change frequency" column in this template uses the following four tiers:

| Tier | Meaning |
| --- | --- |
| **Initialization only** | Settled once during project setup or design; rarely revisited afterwards. |
| **Low frequency** | Updated only when collaboration rules or protocol structure changes; usually untouched within a single session. |
| **Synced with code** | Updated together with code changes, in the same PR or commit. |
| **Per-session sync** | Must be written back before and after every code change the agent makes. |

## Cairn Workspace Layout

- The project root keeps only one Cairn entry file: `AGENTS.md`.
- All other Cairn-managed files live under `.cairn/`, which is the Cairn workspace folder.
- When this document uses a short file name such as `plan.md`, `TODO.md`, `fix_<desc>.md`, or `archive/`, it means the file or folder inside `.cairn/` unless `AGENTS.md` is named explicitly.
- Do not move, rename, or manage project source files through `.cairn/`; the folder is only for Cairn protocol state.

## Required Meta Files (4, mandatory in every project)

Every Cairn project must contain the following 4 protocol files. Their responsibilities must not overlap:

| File | Role | Change Frequency | Who May Change It |
| --- | --- | --- | --- |
| `.cairn/plan.md` | **Plan**: positioning / feature list / data model / interfaces / acceptance criteria | Initialization only | **Only written back when `.cairn/fix_<desc>.md` archives (description corrections as needed) or after `.cairn/fix-plan_<desc>.md` final confirmation (new / revised plan content)**; obvious typos are the exception |
| `.cairn/ARCHITECTURE.md` | **Architecture**: project structure / file ownership / information flow / code ownership / startup order | Initialization only | When project structure, information flow, or code ownership has no clear home, or when the user explicitly asks for architecture changes |
| `AGENTS.md` | **Agent collaboration protocol**: bootstrap/read protocol, status symbols, sync requirements, execution details | Low frequency | When collaboration rules change; must stay in the project root |
| `.cairn/TODO.md` | **Execution ledger**: all steps, subtasks, blockers, sources; both pending and completed work | Per-session sync | Agents must update it around execution; sources are only `.cairn/plan.md` or `.cairn/fix_<desc>.md` |

> Summary: **plan owns project intent, ARCHITECTURE owns project structure, TODO owns execution, AGENTS owns the agent collaboration protocol.** Testing and review closure are handled by the conditional supplementary file `fix_<desc>.md`; plan revision is handled by `fix-plan_<desc>.md` (see their dedicated sections below).

## Conditional Supplementary Files (created on trigger)

The following files are not part of the required meta files. Create them only when their trigger conditions are met. Each file has a single responsibility and must not duplicate the required files.

| File | Trigger | Change Frequency | Default Read Order |
| --- | --- | --- | --- |
| `.cairn/fix_<desc>.md` | The user requests code review / security audit / plan re-check, or provides concrete test feedback | Synced with code (**special status, see dedicated section below**) | Yes (all unarchived batches, sorted by filename) |
| `.cairn/fix-plan_<desc>.md` | The user proposes new features / new modules / acceptance scope extensions outside the current plan, or asks to revise existing plan items | Synced with code (**entry point for writing into `plan.md`, see dedicated section below**) | Yes (all unarchived batches, sorted by filename) |
| `.cairn/HUMAN.md` | `fix_<desc>.md` / `fix-plan_<desc>.md` / `TODO.md` contains work the agent cannot complete, or a human decision that affects project direction, scope, architecture, or acceptance criteria | Low frequency | Yes, if present |
| `.cairn/INTERFACE.md` | The project has external interfaces such as frontend/backend APIs, HTTP/RPC APIs, CLI commands, or SDK methods | Synced with code | No, read on demand |
| `.cairn/TEST.md` | The project is complex **and** the user explicitly asks the agent to test; agent-initiated small checks do not create it | Low frequency (updated only when the user asks for testing) | No, read on demand |
| `.cairn/NICKNAME.md` | The project has at least 5 project-specific terms; fewer terms may live in `AGENTS.md` first | Low frequency | Yes (glossary, read early) |

### HUMAN.md

- `HUMAN.md` is optional and does not need to exist by default.
- Create or update it when `fix_<desc>.md` / `fix-plan_<desc>.md` / `TODO.md` contains work the agent cannot complete, such as real browser validation, account login checks, physical device testing, or external permission operations.
- Also create or update it when the agent finds a `human decision`, `agent confusion`, or `direction risk` that affects product positioning, feature boundaries, architecture choices, external interface commitments, or acceptance criteria. The agent must not silently guess in those cases.
- `HUMAN.md` does not own plans, developer TODOs, user tutorials, or review closure. It records items that need human execution, confirmation, or decision, including impact scope, status, expected result, and feedback format.
- The agent should package work it cannot complete or should not decide unilaterally into `HUMAN.md`; humans may edit item status, feedback, or completion results at any time. The agent pauses the affected scope while continuing other executable TODO work that does not depend on that human input.
- A `HUMAN.md` item that blocks concrete execution must correspond to an existing `TODO.md` item so human work never leaves the execution ledger. Decision-only or confusion-only items may initially reference a source file or conversation context; once they block executable work, the agent must create or confirm the corresponding TODO item.

### INTERFACE.md

- **Purpose**: records concrete definitions for all external interfaces so external callers, frontend integrators, and third-party testers can use them.
- **Required when**: the project exposes frontend/backend interfaces, HTTP/RPC APIs, CLI commands, SDK public methods, or any equivalent external interface.
- **Should include**: paths / methods / request parameters / response shapes / error codes / authentication / call examples.
- **Should not include**: private internal module calls, implementation details, or performance optimization notes.
- **Boundary with `plan.md`**: `plan.md` says which interfaces are needed (What); `INTERFACE.md` defines exact parameters and responses (How).
- **Change frequency**: synced with code. Every interface signature change must update this file. Interface changes do not need a fix loop.
- **Read rule**: not in the default read order; read only for external calls, frontend integration, or interface changes.

### TEST.md

- **Purpose**: an index for project test methods and procedures.
- **Creation condition**: both conditions must hold: the project has become complex, and the user explicitly asks the agent to test. Small agent-initiated checks do not create it.
- **Should include**: test categories, commands, test data setup, coverage goals, and special environment dependencies.
- **Should not include**: concrete bug feedback, which belongs in `fix_<desc>.md`; human validation steps, which belong in `HUMAN.md`.
- **Boundary with `fix_<desc>.md`**: `TEST.md` describes how to test; `fix_<desc>.md` records what a test or review found.
- **Update trigger**: update only when the user actively asks for testing. Routine agent test commands do not trigger updates.
- **Read rule**: not in the default read order; read when the user asks for testing.

### NICKNAME.md

- **Purpose**: a glossary of project-specific terms and naming preferences so new agents and developers can understand local vocabulary, abbreviations, aliases, and naming habits.
- **Creation condition**: create the file when there are at least 5 project-specific terms. With fewer than 5, manage them in a section of `AGENTS.md`; move them to `NICKNAME.md` after crossing the threshold.
- **Include terms when**:
  - A common word has a project-specific meaning.
  - The user coined a project-specific term.
  - A future developer would not quickly understand the abbreviation or codename.
- **Should include**: term -> short definition -> one usage example or scenario; when useful, include old names, aliases, and nearby terms that should not be confused.
- **Should not include**: general programming terms, industry-standard vocabulary, or temporary codenames already clear from surrounding docs.
- **Change frequency**: low frequency. Add new terminology when the agent first identifies it in a session.
- **Read rule**: in the default read order. Agents read root `AGENTS.md` first, then `NICKNAME.md` before architecture and state files when it exists.

### AGENTS.md Content Boundary

- `AGENTS.md` is for agents and automated collaborators. It records collaboration rules, execution flow, status synchronization, constraints, and operational details.
- `AGENTS.md` must not contain user-facing installation guides, feature descriptions, usage guides, marketing copy, or FAQ.
- Put each Cairn-owned piece of information in exactly one owning file: project intent in `plan.md`, structure and information ownership in `ARCHITECTURE.md`, and execution state in `TODO.md`.

### ARCHITECTURE.md Content Rules

`ARCHITECTURE.md` should include at least:

- **Cairn project structure**: the relationship between the `.cairn/` workspace, required files, conditional files, and archive directory.
- **Information ownership and flow**: which information belongs to plan, fix, fix-plan, HUMAN, TODO, INTERFACE, TEST, NICKNAME, and archive, and how it moves between them.
- **Code directory structure and ownership**: top-level and key subdirectories, their responsibilities, and where new code should go.
- **Module dependency relationships**: dependency direction between modules / packages, and constraints against cycles.
- **Startup and call order**: entrypoints, initialization order, and the core call chain.
- **Extension conventions**: where and how to add features, models, and interfaces.

`ARCHITECTURE.md` must not include specific implementation details, algorithm logic, interface parameters, acceptance criteria, or agent execution procedure; those belong in code comments, `INTERFACE.md`, `plan.md`, or `AGENTS.md`.

### Template Change Boundary

- `AGENTS.md` is the project agent collaboration protocol. In normal projects, collaboration rule changes default to the current project `AGENTS.md`.
- Do not modify the upstream template unless the user explicitly asks to change the template or sync rules back to it.
- Template maintenance does not go into a downstream project `TODO.md`; `TODO.md` records only current-project execution, code, and meta-file work.

## Key Rules

### New Project Bootstrap Flow

Every new project adopting this protocol must produce files in the following strict order. No skipping or parallel creation is allowed:

1. **First file: `AGENTS.md`** (copied from the template). Place `AGENTS.md` at the project root before any other meta file is created. It serves as the baseline for every subsequent action; no other meta file may be created before it.

2. **Second item: `.cairn/`, created before other Cairn state files.** Create the folder immediately after `AGENTS.md`. It belongs to Cairn and will contain all protocol state except root `AGENTS.md`.

3. **Third file: `.cairn/plan.md`, finalized only after a thorough discussion.** The overarching principle is: **open exploration during discussion, strict containment when committing.**
   - **Discussion phase (open exploration)**: the agent must actively engage the user in a deep discussion covering at least the project's positioning, feature list, data model, external interfaces, and acceptance criteria. The agent must **proactively identify** potential gaps in the plan, uncovered edge cases, and ambiguous acceptance language. The agent may extend the discussion to suggest additional features, latent risks, or alternative approaches.
   - **Question discipline**: every uncertainty must be raised as an explicit question, awaiting user confirmation. Guesses or default values must not be silently filled in.
   - **Commit phase (strict containment)**: when content is written into `plan.md`, it must be split into two layers:
     - **Layer 1 — User explicit requirements**: every requirement the user has explicitly stated must be captured in full. Nothing may be omitted or trimmed unilaterally.
     - **Layer 2 — Agent suggestions**: any extension, additional feature, or edge-case handling proposed by the agent must obtain the user's explicit per-item approval before it is written into `plan.md`. Suggestions without approval remain only in the discussion record and do not enter the plan.
   - **Final confirmation**: once the plan draft is complete, the user must give an explicit final confirmation ("plan finalized" or equivalent) before the agent writes `.cairn/plan.md` and proceeds to subsequent steps.

4. **After `.cairn/plan.md` is finalized and confirmed**, create the remaining required files in order: `.cairn/ARCHITECTURE.md` -> `.cairn/TODO.md`. `fix_<desc>.md` and `fix-plan_<desc>.md` remain on demand under `.cairn/` and are not pre-created.

   After `TODO.md` exists, its execution steps may only be split from the finalized `plan.md`. User bootstrap commands such as "make a plan from AGENTS" or "initialize the project" trigger the flow, but must not be recorded as TODO sources.

5. **Conditional supplementary files** are created under `.cairn/` only after their trigger conditions are met; they are not pre-created during project bootstrap.

### When Entering an Existing Project

1. Check that all required files exist. Required layout: root `AGENTS.md`, plus `.cairn/plan.md` / `.cairn/ARCHITECTURE.md` / `.cairn/TODO.md`. `fix_<desc>.md` and `fix-plan_<desc>.md` are on demand under `.cairn/`.
2. If legacy Cairn files exist at the project root, migrate every Cairn-managed file except `AGENTS.md` into `.cairn/`, preserving filenames and moving `archive/` to `.cairn/archive/`. Do not move source files or unrelated documentation.
3. Check optional file triggers. If the project has external interfaces but lacks `.cairn/INTERFACE.md`, or has at least 5 project-specific terms but lacks `.cairn/NICKNAME.md`, create the missing file.
4. Default read order: root `AGENTS.md` (this file) -> `.cairn/NICKNAME.md` if present (glossary, early) -> `.cairn/ARCHITECTURE.md` -> all unarchived `.cairn/fix_<desc>.md` files sorted by name -> all unarchived `.cairn/fix-plan_<desc>.md` files sorted by name -> `.cairn/HUMAN.md` if present -> `.cairn/TODO.md` -> `.cairn/plan.md`.
5. `.cairn/INTERFACE.md` / `.cairn/TEST.md` are not in the default read order. Read them on demand for external calls, frontend integration, interface changes, or user-requested testing.

## fix_<desc>.md: Conditional, but with Special Status

Although `fix_<desc>.md` is a conditional supplementary file, it is the **only entry that may rewrite `plan.md` for description corrections**. Its status differs from the other conditional files, so it has its own dedicated section.

### Naming rule

- After `fix_`, use a short semantic description in lowercase kebab style, such as `fix_gesture-scoring.md`, `fix_cold-start-bug.md`, or `fix_ui-layout-review.md`.
- Pure numbers such as `fix_01.md` are forbidden because the filename should reveal the batch's subject.

### Creation timing

Create only in the following two cases. The agent must not create one unilaterally:

- The user explicitly asks for code review / security audit / plan re-check.
- The user provides concrete test feedback (manual QA bugs, E2E failures, reports from external agents, etc.).

### Parallel files

Multiple `fix_<desc>.md` files may exist in parallel, each representing a distinct review batch or related feedback group. Filenames must distinguish their subjects.

### File header

Every `fix_<desc>.md` must begin with the batch's content, time, source, and scope, e.g. "Content: Edge manual QA feedback", "Time: 2026-05-03 21:30", "Source: user manual testing".

### Default source assumption

Items in `fix_<desc>.md` are assumed to come from another agent or tester, not from this session agent's self-analysis. Unless the user explicitly asks for self-review, the agent must not log its own minor observations into `fix_<desc>.md`.

### Priority

All unarchived `fix_<desc>.md` files outrank `TODO.md`. If any fix file has unresolved items, first map them into `TODO.md` items, then continue regular TODO work. Each session begins with the order: fix -> TODO.

### Lifecycle

Created and kept open until all items are resolved (every related TODO entry is `[x]` or `[~]`) -> write any "impact on plan" back to `plan.md` -> archive to `.cairn/archive/fix_desc-YYYYMMDD.md`, preserving the description from the original filename.

### Multiple fix archives

Archive each batch separately into `.cairn/archive/`. Do not merge unrelated batches. Legacy `fix.md` / `fix_01.md` files may be read for compatibility, but new batches must use semantic names.

## fix-plan_<desc>.md: Conditional, the Entry Point for Extending or Revising the Plan

`fix-plan_<desc>.md` resembles `fix_<desc>.md` in form but differs entirely in semantics:

- `fix_<desc>.md` handles fixes to **already-implemented content** (defects where the implementation diverges from the plan, issues raised in review).
- `fix-plan_<desc>.md` handles **extensions or revisions to the plan itself** (new features, new modules, revisions to existing plan items, scope extensions to the acceptance criteria).

### Naming rule

- The prefix is fixed as `fix-plan_`, followed by a short semantic description in lowercase kebab style. Examples: `fix-plan_audit-log.md`, `fix-plan_offline-mode.md`.
- Pure numbers are forbidden, the same as for `fix_*`.

### Strictly separate from fix

Even if the user raises both "fix a bug" and "add a new feature" in the same conversation, the agent **must create two separate files**; merging is not allowed. The filename prefix is the discriminator: `fix_*` is processed as defect feedback, `fix-plan_*` is processed as plan revision.

### Creation timing

Create only when:

- The user explicitly proposes a new feature or new module outside the current plan.
- The user explicitly asks to modify existing `plan.md` content or adjust existing plan items.
- The user requests scope-level adjustments to acceptance criteria or feature boundaries (not defect correction).

The agent must not create a `fix-plan_<desc>.md` unilaterally.

### Commit constraints (identical to new-project plan creation)

Before any content is written into `plan.md`, the contents of `fix-plan_<desc>.md` must follow the same "open exploration during discussion, strict containment when committing" rules as new-project plan creation:

- **Discussion phase (open exploration)**: the agent must actively discuss with the user the positioning of the new content, its impact on existing functionality, edge cases, and acceptance criteria, and proactively identify gaps and risks.
- **Question discipline**: every uncertainty must be raised as an explicit question, awaiting user confirmation. Guesses or default values must not be silently filled in.
- **Commit phase (strict containment)**: writing into `fix-plan_<desc>.md` and subsequently into `plan.md` follows the two-layer rule:
  - **Layer 1 — User explicit requirements**: every new item the user has explicitly stated must be captured in full. Nothing may be omitted or trimmed unilaterally.
  - **Layer 2 — Agent suggestions**: extensions or refinements proposed by the agent must obtain the user's explicit per-item approval before being written.
- **Final confirmation**: once the `fix-plan_<desc>.md` draft is complete, the user must give an explicit final confirmation. Only then may the agent write the contents into `plan.md`; after source annotation and archiving the `fix-plan`, proceed to TODO derivation and implementation.

### File header

Every `fix-plan_<desc>.md` must begin with: content, time, source, and the expected scope of impact on `plan.md` (which sections will be added or revised).

### Priority

- `fix_*` outranks `fix-plan_*`: defect feedback must converge first; scope revisions come after.
- `fix-plan_*` outranks `TODO.md`: an unarchived fix-plan means the plan rewrite loop is not closed; complete the `plan.md` write, source annotation, and `fix-plan` archive before continuing regular TODO work.

### Lifecycle

1. **Create** when the user proposes new content beyond the current plan.
2. **Discuss and obtain final confirmation** following the commit constraints above.
3. **Write into `plan.md`** immediately after final confirmation, following `fix-plan_<desc>.md`. This is the **only entry point for adding, extending, or revising content in `plan.md`**.
4. **Annotate the source**: every item added or revised in `plan.md` by this `fix-plan` must include the source `fix-plan_<desc>.md` (or the archived `.cairn/archive/fix-plan_desc-YYYYMMDD.md`) and the modification date (at least `YYYY-MM-DD`; include `YYYY-MM-DD HH:mm TZ` when known).
5. **Check impact on completed TODO**: if the plan change overwrites, revises, or removes completed TODO work, first search `TODO.md`; if the step has been archived, read the relevant `.cairn/archive/done-YYYYMMDD.md` and decide whether the old archive needs re-review, a supplemental note, or a reopened TODO.
6. **Archive `fix-plan` quickly**: after `plan.md` is updated and source annotations are complete, move `fix-plan_<desc>.md` to `.cairn/archive/fix-plan_desc-YYYYMMDD.md` first. Ideally, `fix-plan` does not wait for implementation TODOs; it only owns the plan rewrite loop.
7. **Break down into TODO** items derived from the updated `plan.md`; if completed TODOs are affected, use the decision from step 5 to add TODOs, mark old TODOs as `[!]` / `[~]`, or add archive notes.
8. **Implement and validate** following the regular iteration flow.

### Plan-rewrite authority compared with fix

| Entry | When `plan.md` is written | Nature of the change |
| --- | --- | --- |
| `fix_<desc>.md` | At archive time, only when current state diverges from the plan's description | Correcting description divergence |
| `fix-plan_<desc>.md` | Immediately after final confirmation, with source annotation | Adding / extending / revising plan content |

### HUMAN.md Intervention Rules

- Defect or review items the agent can solve still enter `TODO.md`, where the agent completes code or doc fixes.
- After the agent finishes the items it can handle, if the only remaining work is human validation or external human operations, those must move to `HUMAN.md` instead of leaving the fix file blocked.
- When the agent finds confusion, a tradeoff, or a risk that affects project direction, scope, architecture, or acceptance criteria, it must move that item to `HUMAN.md` and mark the affected scope. The agent must not keep changing that scope, but should continue TODO work that does not depend on the decision.
- When moving an item, mark it as handled or routed inside `fix_<desc>.md` / `fix-plan_<desc>.md` and note the target `HUMAN.md#H-...`. Once no agent-executable work remains, the source file may be archived per its lifecycle.
- A `HUMAN.md` item that blocks concrete execution must reference a `TODO.md` item, e.g. `TODO 21.8`. The corresponding TODO stays unchecked with a blocker pointing to `HUMAN.md#H-...`.
- Decision-only or agent-confusion items that do not yet block concrete execution may reference a source file or conversation context first; once they block executable work, the agent must create or confirm the corresponding TODO item.
- After the human completes or decides the item, the agent updates the corresponding TODO item to `[x]` / `[~]`, or resumes the affected TODO, plan, fix, or fix-plan flow, then archives or trims the HUMAN entry.
- Use `H-YYYYMMDD-01` IDs. Each item must include source, type, status, impact scope, paused scope, work the agent can continue, why human input is needed, what the human should do or answer, expected result or decision effect, and feedback format. Human-execution items should also include preconditions and steps.
- Completed HUMAN details may move to `.cairn/archive/human-YYYYMMDD.md`; keep only a short index in the active file.

### plan.md Revision Path

Direct edits to `plan.md` during normal sessions are forbidden, except for obvious typos or broken links. Any plan modification must go through one of the following entries:

- **`fix_<desc>.md`**: corrects divergence between the current state and the plan's description (e.g. an implementation has shipped but the plan was not updated). The plan is rewritten on archive, on demand.
- **`fix-plan_<desc>.md`**: extends or revises the plan's scope (new features, new modules, new acceptance criteria). When handling a plan modification request, first create `fix-plan_<desc>.md`, then modify `plan.md` from that file, annotate the changed plan items with the `fix-plan` source and modification date, archive the `fix-plan` first, and only then decide how to update or add `TODO.md` entries from the revised plan.

See the dedicated sections for `fix_<desc>.md` and `fix-plan_<desc>.md`.

### ARCHITECTURE.md Revision Path

- **Do not modify during normal iteration**: new features, refactors, and bug fixes do not automatically trigger architecture changes.
- **Allowed cases**:
  1. Project structure, file ownership, information flow, or new code has no suitable home under the existing architecture, and design adjustment cannot resolve that.
  2. The user explicitly asks for architecture changes.
- When modified, edit `ARCHITECTURE.md` directly and note in the corresponding `TODO.md` item that architecture has been synchronized.

### INTERFACE.md Sync Rules

- Interface signature changes (path / method / parameters / return value / error code) must update `INTERFACE.md` in the same PR / commit as the code change.
- Comments-only or internal implementation changes do not trigger updates.
- Interface changes do not need a `fix_<desc>.md` loop. If a breaking change affects external callers, add a separate `TODO.md` step to notify callers.

### NICKNAME.md Maintenance Rules

- When the agent first identifies project-specific terminology in a session, append it to `NICKNAME.md` (or to the terminology section of `AGENTS.md` if still below the threshold).
- If the user uses a term whose meaning cannot be inferred from general knowledge, ask for its definition before adding it to the glossary.
- Remove terms only when the user explicitly says they are deprecated or renamed. Keeping the old term as an alias to the new term is safer.

## TODO.md Status Symbols

- `[x]` completed
- `[ ]` pending
- `[!]` **modified**: the original item is still partly valid but replaced by a new plan; the same line must explain `modified: <reason / target item>`
- `[~]` **removed**: the original item was canceled or replaced; the same line must explain `removed: <reason / target item>`. Keep removed items for traceability.
- `Blocked:` indicates a blocker and records the reason.

## TODO ↔ Code Synchronization

- **TODO source constraint**: every TODO step and subtask source is only the current `plan.md` or `fix_<desc>.md`; `fix-plan_<desc>.md` must first be written into and archived through `plan.md`, then TODO is derived from the updated plan. Raw user commands, the current conversation, sidecar-agent notes, test feedback, or AGENTS bootstrap commands may trigger plan / fix flow, but must not directly become TODO sources.
- **Before every code change**: confirm that `TODO.md` has a corresponding item. If none exists, return to the source layer first: derive existing-plan work from `plan.md`, create `fix_<desc>.md` before mapping defect feedback to TODO, or run `fix-plan_<desc>.md` and write back to `plan.md` before splitting TODO for new or revised plan scope.
- **After every code change**: immediately mark TODO as `[x]`, write a blocker, or add subtasks, then report to the user. Do not change code first and update TODO later.
- **Whenever a new plan or feedback appears**: if it is a plan modification, first create and confirm `fix-plan_<desc>.md`, update `plan.md` directly with source and modification date annotations, archive the `fix-plan`, and only then derive or adjust `TODO.md` from the revised `plan.md`; if it is defect, test, or review feedback, first create or update `fix_<desc>.md`, then map it to TODO; if completed TODOs are affected, inspect the current TODO section or relevant `.cairn/archive/done-YYYYMMDD.md` before deciding whether old items become `[!]` / `[~]`, a re-review TODO is needed, or an archive note should be added.
- **Whenever a new human intervention item appears**: if work requires human execution, or a human decision affects project direction, scope, architecture, or acceptance criteria, write it into `HUMAN.md`; if it blocks concrete execution, first create or confirm the corresponding `TODO.md` item, then point the TODO blocker to `HUMAN.md#H-...`. The agent pauses the affected scope while continuing other TODO work that does not depend on that human input.
- **Whenever an external interface changes**: if an interface signature changed, add "synced `INTERFACE.md`" under the current TODO item; otherwise the item must not be marked `[x]`.
- Do not mark a step completed before validation commands pass. Exceptions must record the reason next to the item.
- Blocked steps remain unchecked and must include `Blocked: <specific reason + next recovery action>`.

## TODO Step Archiving

Archiving is part of a TODO step's completion loop, not optional cleanup. After every `TODO.md` update, check whether any step satisfies the archive trigger; once it does, archive it in the same session and leave a traceable breadcrumb.

### Trigger

A TODO step must be archived when all of its subtasks have converged to `[x]` or `[~]`, and it has no `[ ]`, `[!]`, or blocker items.
If an active step still references it through `[!] modified: ... see step N`, do not archive yet; once the reference closes, archive it immediately.

### Archive Action

1. Move the entire `### Step N: <title>` section and its subtasks to `.cairn/archive/done-YYYYMMDD.md`.
2. Multiple steps archived on the same day share the same file; later batches use `-2.md`, `-3.md`, etc.
3. The archive file first line is `# Archived: steps N, M, ... (archived on YYYY-MM-DD)`.

### Breadcrumb Left In TODO

Leave one traceable line in the original location:

```md
### Step N: <title> - Archived on YYYY-MM-DD -> archive/done-YYYYMMDD.md
```

### Must Not Archive When

- The step contains `[ ]`, `[!]`, or blockers.
- The step is still referenced by active steps.

## Archive Rules

- Every project uses a single `.cairn/archive/` folder for Cairn archived files.
- Fix archive naming: `.cairn/archive/fix_desc-YYYYMMDD.md`, preserving the description from the original fix filename. Legacy `fix.md` archives use `.cairn/archive/fix-YYYYMMDD.md`.
- Fix-plan archive naming: `.cairn/archive/fix-plan_desc-YYYYMMDD.md`, preserving the description from the original fix-plan filename. By default, `fix-plan` archives immediately after `plan.md` has been modified and source annotations are complete; it does not wait for subsequent TODO implementation.
- Human archive naming: `.cairn/archive/human-YYYYMMDD.md`.
- Archiving does not delete content; move the whole file and append "(archived on YYYY-MM-DD)" to its top heading.

## Project Structure And Code Organization

- When a single file exceeds 800 lines, evaluate whether it should be split.
- Project structure, file ownership, information flow, and new code ownership follow `ARCHITECTURE.md`; if no suitable location exists, update architecture before continuing.

## Comments

- Comments explain why something is done, not what the code already says.
