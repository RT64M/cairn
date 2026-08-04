# Workflow 1: New Project Bootstrap

> Scenario: an empty directory or newly initialized repository needs to adopt the Cairn protocol.

## Goal

Agents establish file constraints first, then discuss the plan, and only then write code. New projects do not need to copy every template file. They only need to copy the root `AGENTS.md` template, create `.cairn/`, and create project-specific meta files in the strict order below.

## Creation Order: No Skipping Or Parallel Creation

### Step 1: Create The Project Directory And Initialize Git

### Step 2: Copy `AGENTS.md`, The First File The Project Produces

```bash
cp template/AGENTS.en.md my-project/AGENTS.md
```

Chinese-first projects may copy `template/AGENTS.zh.md`.

> No other meta file may be created before `AGENTS.md` exists.

### Step 3: Create `.cairn/`

Create `.cairn/` immediately after `AGENTS.md`. All Cairn-managed files except root `AGENTS.md` live there.

### Step 4: Discuss And Finalize `.cairn/plan.md`

Principle: **open exploration during discussion, strict containment when committing.**

#### 3.1 Discussion Phase

- Discuss positioning, feature list, data model, external interfaces, and acceptance criteria.
- Proactively identify gaps, uncovered edge cases, and ambiguous acceptance language.
- The agent may suggest additional features, risks, and alternatives for user consideration.
- Every uncertainty must be asked explicitly and confirmed by the user; do not silently fill guesses or defaults.

#### 3.2 Commit Phase

When writing `plan.md`, split content into two layers:

- **Layer 1: explicit user requirements**. Capture all requirements the user clearly stated, without omission or unilateral trimming.
- **Layer 2: agent suggestions**. Add agent-proposed extensions or edge-case handling only after explicit per-item user approval. Unapproved suggestions remain in the discussion record and do not enter the plan.

#### 3.3 Final Confirmation

After the plan draft is complete, the user must give final confirmation, such as "plan finalized", before the agent writes `.cairn/plan.md` and proceeds.

### Step 5: Create The Remaining Required Meta Files In Order

After `.cairn/plan.md` is finalized and confirmed:

1. `.cairn/ARCHITECTURE.md`
2. `.cairn/TODO.md`

`fix_<desc>.md` and `fix-plan_<desc>.md` are always on demand under `.cairn/`. Do not pre-create empty placeholders.

### Step 6: Check Conditional Supplement Triggers

Create these only after their triggers appear:

- External interfaces exist: create `.cairn/INTERFACE.md`.
- The project is complex and the user asks for testing: create `.cairn/TEST.md`.
- Project-specific terminology reaches 5 or more terms: create `.cairn/NICKNAME.md`.
- Human execution or direction-level human decisions appear: create `.cairn/HUMAN.md`.

### Step 7: Split The Initial Plan Into `.cairn/TODO.md` Steps

Ensure every future code change has a corresponding ledger item.

Split TODO only from the finalized `plan.md`. User bootstrap commands such as "read AGENTS and make a plan" trigger the bootstrap flow, but are not TODO item sources.

## Anti-Patterns

- Creating other meta files before `AGENTS.md`.
- Creating root Cairn state files instead of using `.cairn/`.
- Skipping discussion and drafting `plan.md` immediately.
- Writing agent suggestions into `plan.md` without explicit user approval.
- Creating `ARCHITECTURE.md` or writing code before final plan confirmation.
- Writing code without `plan.md`.
- Recording a bootstrap command such as "make a plan from AGENTS" as the TODO source.
- Putting user tutorials into `AGENTS.md`.
- Putting interface details into `plan.md`.
- Pre-creating empty `fix_<desc>.md` or `fix-plan_<desc>.md`.
- Naming new feedback batches with pure numbers such as `fix_01.md`.

## Next

- Daily iteration: [02-iteration-cycle.md](02-iteration-cycle.md)
- First feedback batch: [03-fix-lifecycle.md](03-fix-lifecycle.md)
