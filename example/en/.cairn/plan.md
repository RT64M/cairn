# Northstar Ops Plan

## Project Positioning

Northstar Ops is an internal operations ticket collaboration platform. It helps support, fulfillment, finance, and operations leads view customer requests, assign owners, record handling notes, track SLA risk, and preserve audit trails in one workspace.

The project uses a separated frontend/backend architecture:

- The web frontend owns the workbench, list filters, detail pages, bulk actions, and multilingual UI copy.
- The API backend owns authentication, authorization, ticket state transitions, audit logs, notifications, and reporting aggregation.
- Background workers own SLA alerts, notification retries, daily reports, and expired-data cleanup.

## Current Version Goals

The current phase delivers an MVP that operations teams can pilot:

- SSO login and team permissions.
- Ticket list, ticket detail, assignment, notes, and status transitions.
- Bulk assignment and bulk closing.
- Support-note auditing.
- Basic SLA alerts.
- Chinese and English interface copy.
- Audit-log export.
- Duplicate ticket merge. (source: fix-plan_ticket-merge, 2026-05-06)

## Non-Goals

- No public registration.
- No customer self-service portal.
- No native mobile app.
- No complex BI reporting.
- No real-time chat integration.
- No cross-tenant ticket sharing.

## Core Users

| User | Main Need |
| --- | --- |
| Support specialist | Claim, note, transfer, and close tickets quickly |
| Operations lead | Bulk assign tickets, inspect SLA risk, and sample handling quality |
| Finance collaborator | View refund or compensation tickets and add handling status |
| System administrator | Manage teams, permissions, audit exports, and notification settings |

## Core Objects

| Object | Description |
| --- | --- |
| Ticket | A customer request or internal operations task |
| Assignment | Current owner plus historical assignment changes |
| Note | Support note with internal, cross-team, or audit visibility |
| SLA Policy | Response and resolution time policy |
| Audit Event | Sensitive-operation record |
| Notification | Reminder task for owners and leads |
| Merge Record | Merged tickets, primary ticket, actor, and undo window (source: fix-plan_ticket-merge, 2026-05-06) |

## Key Flows

### Login and Permissions

Users log in through enterprise SSO. The backend derives access scope from organization, team, and role. The frontend only displays tickets, actions, and exports the user is allowed to access.

### Ticket Handling

After a ticket enters the queue, support can claim it or a lead can bulk assign it. During handling, users can add notes, change status, transfer teams, or close the ticket. Every sensitive operation writes an audit event.

### Bulk Assignment

A lead filters a group of tickets and bulk assigns them by team, owner, priority, and SLA risk. The system must prevent duplicate submission, permission leakage, and invisible partial failures.

### Ticket Merge (source: fix-plan_ticket-merge, 2026-05-06)

When the same customer submits duplicate tickets within 24 hours, a lead can pick a primary ticket and merge the rest into it manually. Merged tickets become read-only and their detail pages keep a link to the primary ticket. A 10-minute undo window applies; after it expires the merge cannot be undone. The merge operation writes an audit event. This phase ships manual merge only — no duplicate auto-detection, and merge is not part of the bulk-action entry point.

### Note Auditing (source: plan initialization, 2026-05-05; last revised: fix-plan_ticket-merge, 2026-05-06)

Notes pass through baseline validation before saving. Notes involving refunds, privacy, or escalated complaints are marked as audit focus items and searchable from the audit page. On a ticket merge, internal and cross-team notes move to the primary ticket and keep their original source markers; audit notes do not move, so audit trails never spill across tickets.

## Acceptance Criteria

- An operations lead can bulk assign within a filtered result set of up to 200 tickets.
- Partial bulk-operation failure shows success count, failure count, and retryable items clearly.
- Saved support notes remain consistent across detail pages, audit logs, and export records.
- Unauthorized users cannot read or operate on other teams' tickets.
- All user-visible copy renders through i18n keys.
- Core interfaces have contract or integration test coverage.
- Real SSO, real accounts, external notification verification, or notification-policy decisions that affect acceptance move to `HUMAN.md`.
- Merged tickets render read-only on the detail page and link to their primary ticket. (source: fix-plan_ticket-merge, 2026-05-06)
- Audit trails do not spill across tickets after a merge; audit notes on merged tickets stay with the original ticket. (source: fix-plan_ticket-merge, 2026-05-06)

## Documentation Boundaries

- Current execution state goes in `TODO.md`.
- Test feedback and review findings go in semantically named `fix_<desc>.md` files, such as `fix_audit-batch.md`.
- Human execution items and directional human decisions go in `HUMAN.md`.
- API and frontend/backend interface rules go in `INTERFACE.md`.
- Test strategy goes in `TEST.md`.
- Architecture notes go in `ARCHITECTURE.md`.
- Stable terminology goes in `NICKNAME.md`.
