# HUMAN

> This file records human-only execution items and directional decisions agents cannot safely make alone. Agents should pause the affected scope while continuing other TODO items that do not depend on the human input.

## H-20260506-01: Pre-Production Audit-Page SSO Permission Verification

- Type: human execution
- Source: fix_audit-batch.md F-04
- Related TODO: TODO 9.5
- Status: waiting for human execution
- Impact scope: pre-production audit-page permission acceptance
- Agent-paused portion: real SSO permission conclusion and acceptance pass marker
- Agent-can-continue portion: TODO 8.6, 9.3, 9.4, 10.7
- Why human input is needed: this requires logging into pre-production with real enterprise SSO accounts. Agents do not have accounts, verification codes, organization permissions, or browser sessions.
- Preconditions:
  - Pre-production has deployed the version that includes the note audit page.
  - At least one operations-lead account and one normal support account are available.
  - Both accounts belong to the correct enterprise SSO groups.
- Steps:
  - Log into pre-production with the operations-lead account.
  - Open the audit-page entry.
  - Confirm note-audit list, filters, and export entry are visible.
  - Log out and log back in with the normal support account.
  - Confirm the support account cannot enter the audit page and cannot see the export entry.
  - Record any abnormal copy, blank page, redirect loop, or permission leakage.
- Expected result:
  - Operations-lead account can access the audit page.
  - Normal support account cannot access the audit page.
  - Neither account enters a login loop.
  - Audit-page permission result matches the permission boundary in `ARCHITECTURE.md`.
- Feedback format:
  - Execution time:
  - Pre-production version:
  - Lead-account result:
  - Support-account result:
  - Exception description: text only
  - Passed:

## H-20260506-02: Should Notification Failure Block Bulk Assignment?

- Type: human decision
- Source: TODO 8.9
- Related TODO: TODO 8.9
- Status: waiting for human decision
- Impact scope: bulk-assignment notification queue, acceptance criteria, user-visible failure copy
- Agent-paused portion: whether notification failure rolls back the ticket owner or marks the whole bulk assignment failed
- Agent-can-continue portion: TODO 8.6 failure matrix, TODO 9.3 audit-event fields, TODO 9.4 note-interface error rules, TODO 10.7 documentation link check
- Why human input is needed: this decision changes what operations leads understand as "bulk assignment succeeded" and affects downstream interfaces and acceptance criteria. Agents can implement any option, but should not choose product direction alone.
- Human decision needed:
  - Option A: assignment succeeds once ticket owner updates; notification failure only enters retry queue and shows a warning.
  - Option B: notification failure marks that item as partial failure, but does not roll back the updated owner.
  - Option C: notification failure rolls back that owner update to keep strong consistency.
- Expected result or decision effect: after a human chooses an option, the agent updates TODO 8.9 and synchronizes `INTERFACE.md`, `TEST.md`, and acceptance notes as needed.
- Feedback format:
  - Decision option: A / B / C
  - Additional notes:
  - Update plan needed:
