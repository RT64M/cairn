# TODO

> Status convention: see [AGENTS.md](../AGENTS.md).

## Active Steps

### Step 8: Bulk Ticket Assignment MVP (source: current goals in plan.md)

- [x] 8.1 Define the bulk-assignment entry point, permissions, and error-display scope
- [x] 8.2 Update bulk-assignment interface contracts in `INTERFACE.md`
- [x] 8.3 Verify that the `Assignment` history display scope matches the existing description in `plan.md`; no divergence to write back (this step's source is the plan, so it must not rewrite the plan directly; any divergence needs its own `fix_<desc>.md`, written back on archive)
- [!] 8.4 Send notifications immediately after bulk assignment  Revised: see 8.9; notifications move to an async queue so the page does not wait for external services
- [x] 8.5 Add frontend interaction copy keys to the error-shape notes in `INTERFACE.md`
- [ ] 8.6 Complete the acceptance matrix for partial-failure cases
  - [x] 8.6.1 Permission-denied failure display
  - [x] 8.6.2 Closed-ticket failure display
  - [ ] 8.6.3 Assignee-not-in-target-team failure display
- [ ] 8.7 Update the regression scope in `TEST.md`
- [ ] 8.8 Mark this step complete after verification commands pass
- [ ] 8.9 Move notification sending to an async queue (source: current goals in plan.md; revises 8.4)
  Blocked: moved to HUMAN.md#H-20260506-02; waiting for a human decision on whether notification failure blocks ticket assignment

### Step 9: Support-Note Audit Closure (source: fix_audit-batch.md)

- [x] 9.1 Move `fix_audit-batch.md` feedback into TODO subitems
- [x] 9.2 Define note visibility: internal, cross-team, audit
- [ ] 9.3 Add audit-event fields after note save
  - [x] 9.3.1 Record actor, ticket, note visibility, and timestamp
  - [ ] 9.3.2 Record sensitive-keyword match labels
- [ ] 9.4 Update note-create interface error rules in `INTERFACE.md`
- [ ] 9.5 Verify pre-production audit-page permissions with real SSO accounts
  Blocked: moved to HUMAN.md#H-20260506-01; waiting for human execution
- [ ] 9.6 Archive `fix_audit-batch.md` after all agent-executable items are complete

### Step 10: Project Meta-File Cleanup (source: documentation boundaries in plan.md)

- [x] 10.1 Create `.cairn/NICKNAME.md`
- [x] 10.2 Create `.cairn/ARCHITECTURE.md`
- [x] 10.3 Create `.cairn/INTERFACE.md`
- [x] 10.4 Create `.cairn/TEST.md`
- [x] 10.5 Create `.cairn/HUMAN.md`
- [x] 10.6 Create `archive/` examples
- [ ] 10.7 Check documentation links

### Step 11: Duplicate Ticket Merge (source: Key Flows / Ticket Merge in plan.md, written by archive/fix-plan_ticket-merge-20260506.md)

- [ ] 11.1 Define the merge entry point, permissions, and primary-ticket selection rules
- [ ] 11.2 Define the read-only state and primary-ticket link for merged tickets
- [ ] 11.3 Define note-migration scope: internal and cross-team notes move, audit notes do not
- [ ] 11.4 Define the 10-minute undo window boundary and expiry behavior
- [ ] 11.5 Write an audit event for the merge operation
- [ ] 11.6 Synchronize merge interface contracts and error rules in `INTERFACE.md`
- [ ] 11.7 Carry the detail-page read-only acceptance noted in the Step 7 archive (see archive/done-20260505.md)
- [ ] 11.8 Update the regression scope in `TEST.md`

## Archived

### Step 1: Project meta-file initialization — archived on 2026-05-05 -> archive/done-20260505.md
### Steps 2-4: Login, team permissions, first ticket list — archived on 2026-05-05 -> archive/done-20260505.md
### Step 5: First manual acceptance feedback — archived on 2026-05-05 -> archive/fix_initial-review-20260505.md
### Step 6: Pre-production SSO manual verification — archived on 2026-05-05 -> archive/human-20260505.md
### Step 7: Ticket detail and basic notes — archived on 2026-05-05 -> archive/done-20260505.md
