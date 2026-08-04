# fix_audit-batch: Note Audit and Bulk Assignment Feedback

- Content: external agent review and operations-lead manual testing feedback
- Time: 2026-05-06 10:30
- Source: external agent review, operations-lead pre-production trial
- Scope: bulk assignment, note saving, audit-page permissions, API error display

## Feedback Items

### F-01 Missing Retryable-Item Detail on Partial Bulk-Assignment Failure

- Status: in progress
- Related TODO: TODO 8.6
- Symptom: When bulk assigning 37 tickets, 3 failed because the assignee was not in the target team, but the UI showed only a failure count and did not tell the lead which items were retryable.
- Expected: The failure list distinguishes "retry after changing assignee" from "current user has no permission."
- Plan impact: none; this refines an existing bulk-assignment acceptance criterion.

### F-02 Audit Event Missing Sensitive-Keyword Labels After Note Save

- Status: in progress
- Related TODO: TODO 9.3
- Symptom: When note content matched refund, escalation, or privacy terms, the audit log recorded only a note-created event and omitted match type.
- Expected: Audit events preserve sensitive-keyword match labels so leads can filter priority notes.
- Plan impact: may need to write back to the core-object description in `plan.md` on archive.

### F-03 Note-Create Error Shape Is Unstable

- Status: pending
- Related TODO: TODO 9.4
- Symptom: Field validation failure, permission failure, and sensitive-word policy failure return inconsistent shapes, so the frontend cannot map stable error copy.
- Expected: The note-create endpoint follows the unified error shape in `INTERFACE.md`.
- Plan impact: none; this aligns API documentation with implementation.

### F-04 Pre-Production Audit-Page Permission Needs Real SSO Verification

- Status: moved to human
- Related TODO: TODO 9.5
- Human item: HUMAN.md#H-20260506-01
- Symptom: Local mock roles can access the audit page, but enterprise SSO group mapping in pre-production needs real-account verification.
- Expected: Operations-lead accounts can open the audit page; normal support accounts cannot.
- Plan impact: none; this is external-environment acceptance.

## Closure Conditions

- TODO 8.6, 9.3, and 9.4 are completed or intentionally deprecated.
- TODO 9.5 has moved to HUMAN and no longer blocks agent-executable work.
- `INTERFACE.md`, `TEST.md`, and `ARCHITECTURE.md` have synchronized related rules.
- Archive notes record whether `plan.md` needs a write-back.
