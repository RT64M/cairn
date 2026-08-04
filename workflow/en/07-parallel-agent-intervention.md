# Workflow 7: Sidecar Agent Intervention

> Scenario: the main agent is continuously working on a TODO while the user asks another agent to review, supplement context, revise docs, or propose plan changes. Sidecar work should be absorbed by the main line without interrupting the main agent's current context.

## Roles

- **Main agent**: keeps progressing the current `.cairn/TODO.md` execution step and checks Cairn file changes at natural sync points.
- **Sidecar agent**: handles the temporary supporting task and writes results into the correct meta file; it does not take over the main agent's context.
- **User**: may trigger sidecar review, context supplementation, manual feedback, or plan discussion at any time, then let files carry the result back to the main line.

## Typical Use

1. The main agent is implementing a TODO step, such as `Step 12: queue bulk assignment`.
2. The user avoids interrupting the main agent and asks another agent to work in parallel, for example:
   - Review whether current implementation diverges from plan.
   - Supplement interface or test notes.
   - Draft a plan revision from a new idea.
   - Check whether documentation matches current behavior.
3. The sidecar agent reads necessary context and writes its conclusion into the appropriate Cairn file.
4. At the next sync point, the main agent follows the default read order for structure, unarchived `fix_*`, unarchived `fix-plan_*`, `.cairn/HUMAN.md`, `.cairn/TODO.md`, and relevant conditional files.
5. The main agent absorbs sidecar results according to protocol priority and continues the main line.

## Where Sidecar Results Go

| Sidecar Work Type | Write To | Main Agent Follow-Up |
| --- | --- | --- |
| Bug, implementation drift, or review issue in completed content | `.cairn/fix_<desc>.md` | Map to `TODO.md`, then handle by fix priority |
| Feature, module, or acceptance expansion outside the plan | `.cairn/fix-plan_<desc>.md` | Discuss, confirm, write `plan.md`, archive fix-plan, then split TODO |
| Real environment operation, human permission, or direction decision | `.cairn/HUMAN.md` | Pause affected scope and continue independent TODOs |
| External interface signature, error code, or call example note | `.cairn/INTERFACE.md` | If it affects current TODO, note interface sync there |
| Test method or regression matrix requested by user | `.cairn/TEST.md` | Problems found later still go to `fix_<desc>.md` |
| New project vocabulary, alias, or abbreviation | `.cairn/NICKNAME.md` | Future agents read it early after `AGENTS.md` |
| Only supplements an existing TODO step's subtask, status, or acceptance note | `.cairn/TODO.md` | Main agent merges execution ledger at sync point; the sidecar request itself must not become a new TODO source |

## Sync Points

The main agent does not need to poll every second, but must re-check at these times:

- At the start of each session.
- After finishing a TODO subitem.
- Before modifying `.cairn/plan.md`, `.cairn/TODO.md`, `.cairn/INTERFACE.md`, `.cairn/TEST.md`, or other meta files.
- After the user says another agent wrote feedback, supplements, or revisions.
- Before claiming the current task is complete.

Sync uses the default read order: root `AGENTS.md` -> `.cairn/NICKNAME.md` if present -> `.cairn/ARCHITECTURE.md` -> unarchived `.cairn/fix_*` -> unarchived `.cairn/fix-plan_*` -> `.cairn/HUMAN.md` -> `.cairn/TODO.md` -> `.cairn/plan.md`. Read `.cairn/INTERFACE.md` and `.cairn/TEST.md` on trigger.

## Example

The main agent is working on:

```text
.cairn/TODO.md
- [ ] 12.3 Implement retry for the bulk assignment worker
```

Meanwhile, the user asks a sidecar agent to check the interface contract. The sidecar agent finds that `POST /tickets/bulk-assign` already returns `409_CONFLICTING_STATUS`, but the error code is missing from the docs.

The sidecar agent does not interrupt the main agent or edit the same worker file. It updates:

```text
.cairn/INTERFACE.md
- Add 409_CONFLICTING_STATUS to POST /tickets/bulk-assign.

.cairn/TODO.md
- [ ] 12.4 Sync bulk assignment error-code docs (source: current goals in plan.md; sidecar synced INTERFACE.md)
```

After finishing `12.3`, the main agent syncs TODO, sees `12.4`, confirms `INTERFACE.md` is updated, and either adds tests or marks the subitem complete.

If the sidecar agent found an implementation bug instead, it should create `fix_bulk-assign-contract.md`, not only edit TODO. The main agent handles it by fix priority at the next sync.

## Anti-Patterns

- Sidecar agent edits `plan.md` directly, bypassing `fix-plan_<desc>.md` and final user confirmation.
- Sidecar agent edits the same code region the main agent is actively editing.
- Sidecar agent leaves feedback only in chat, so the main agent cannot detect it.
- Sidecar agent records the sidecar request, current conversation, or its one-line conclusion as a new TODO source instead of returning to `plan.md` / `fix_*` / `fix-plan_*`.
- Defect feedback and plan expansion are mixed into one `fix_<desc>.md`.
- Main agent claims completion without checking unarchived sidecar `fix_*`, `fix-plan_*`, or `TODO.md` changes.
