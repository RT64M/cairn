# fix-plan_ticket-merge: Duplicate Ticket Merge (archived on 2026-05-06)

- Content: duplicate-ticket merge capability proposed by an operations lead; a new feature outside the current plan
- Time: 2026-05-06 15:10
- Source: operations lead, after piloting in pre-production
- Expected impact on plan: Current Version Goals (one addition), Core Objects (add Merge Record), Key Flows (add "Ticket Merge", revise "Note Auditing"), Acceptance Criteria (two additions)

## Discussion and Confirmation

### Layer 1 — User explicit requirements

- P-01 A lead can manually merge tickets the same customer submitted more than once within 24 hours into one primary ticket.
- P-02 After merging, merged tickets become read-only; their detail pages keep an entry point linking to the primary ticket.
- P-03 The merge operation writes an audit event.

### Layer 2 — Agent suggestions (approved per item)

- P-04 Offer a 10-minute undo window after merging. **User: approved.**
- P-05 Move notes from merged tickets onto the primary ticket, preserving their original source markers. **User: approved, but only internal and cross-team notes; audit notes are not moved, so audit trails never spill across tickets.**
- P-06 Auto-detect suspected duplicates and prompt the lead. **User: not approved. This phase ships manual merge only; auto-detection is deferred.**
- P-07 Add merge to the bulk-action entry point. **User: not approved. Merging requires confirming a primary ticket case by case and does not fit bulk.**

### Final confirmation

- Time: 2026-05-06 15:40
- User's confirmation: merge design finalized; write P-01 through P-05 into the plan.

## What Was Written Into plan.md

| Plan location | Action | Items |
| --- | --- | --- |
| Current Version Goals | Add "Duplicate ticket merge" | P-01 |
| Core Objects | Add `Merge Record` | P-01, P-02, P-04 |
| Key Flows | Add the "Ticket Merge" section | P-01 through P-04 |
| Key Flows / Note Auditing | Revise: add note-migration visibility boundaries for merges | P-05 |
| Acceptance Criteria | Add two items (read-only plus primary-ticket link, audit trails do not cross tickets) | P-02, P-03, P-05 |

Unapproved items P-06 and P-07 stay in this file only and do not enter `plan.md`.

## Impact On Completed TODO

- Step 7 "Ticket detail and basic notes" was archived in `archive/done-20260505.md`. Merged tickets now need a read-only state and a primary-ticket link on the detail page, which extends already-completed scope.
- Decision: do not reopen Step 7. Append a note under its archive entry instead, and let Step 11, derived from the revised `plan.md`, carry the implementation. The note has been added to `archive/done-20260505.md`.

## Archive Note

The `plan.md` write and its source annotations are complete, so this file is archived ahead of the implementation TODOs, per protocol. See `TODO.md` Step 11 for the implementation.
