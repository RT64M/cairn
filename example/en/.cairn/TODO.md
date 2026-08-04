# TODO

> Status convention: see [AGENTS.md](../AGENTS.md).

## Active Steps

### Step 8: Bulk Ticket Assignment MVP (source: current goals in plan.md)

- [x] 8.1 Define the bulk-assignment entry point, permissions, and error-display scope
- [x] 8.2 Update bulk-assignment interface contracts in `INTERFACE.md`
- [x] 8.3 Update `Assignment` history notes in `plan.md`
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

## Archived

### Step 1: Project meta-file initialization — archived on 2026-05-05 -> archive/done-20260505.md
### Steps 2-4: Login, team permissions, first ticket list — archived on 2026-05-05 -> archive/done-20260505.md
### Step 5: First manual acceptance feedback — archived on 2026-05-05 -> archive/fix_initial-review-20260505.md
### Step 6: Pre-production SSO manual verification — archived on 2026-05-05 -> archive/human-20260505.md
### Step 7: Ticket detail and basic notes — archived on 2026-05-05 -> archive/done-20260505.md
