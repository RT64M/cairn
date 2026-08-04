# API Contract

## Principles

- APIs use versioned paths.
- All business errors use a unified error shape.
- List endpoints consistently support pagination, sorting, and filter summaries.
- Write operations require idempotency keys or backend idempotency protection.
- The backend returns permissions/capabilities; the frontend uses them to show or hide actions.

## Authentication

| Item | Contract |
| --- | --- |
| Login source | Enterprise SSO |
| Session type | Secure cookie |
| Permission source | Backend maps SSO groups to roles and teams |
| Frontend responsibility | Display login state and available actions, never final authorization |

## Resources

| Resource | Description |
| --- | --- |
| Tickets | Ticket list, detail, state transition |
| Assignments | Ticket owner and assignment history |
| Notes | Support notes and visibility |
| Audit Events | Sensitive-operation records |
| Notifications | Notification-task status |
| Exports | Export jobs and file status |

## Key Interfaces

| Interface | Description | Main Errors |
| --- | --- | --- |
| Query ticket list | Filter tickets by queue, status, team, SLA risk | no permission, invalid filter |
| Get ticket detail | Return ticket, notes, assignment history, available actions | not found, no permission |
| Bulk assign | Assign a group of tickets to a team or owner | partial failure, idempotency conflict, assignee not in team |
| Create note | Append a ticket note and write audit event | invalid field, sensitive-word policy failure, no permission |
| Query audit events | Query by ticket, actor, event type, and time range | no permission, range too large |
| Create export job | Generate export file from filters | no permission, export range too large |

## Error Shape

| Field | Description |
| --- | --- |
| code | Stable error code for frontend copy mapping |
| message_key | User-visible copy key |
| request_id | Troubleshooting identifier |
| field_errors | Field-level error list |
| retryable | Whether the user can change input and retry |
| details | Limited debug context without sensitive data |

## Bulk-Assignment Result

The bulk-assignment endpoint must distinguish whole-operation result from per-item result:

| Result | Description |
| --- | --- |
| success_count | Number of successfully assigned tickets |
| failed_count | Number of failed tickets |
| retryable_items | Items that can be retried after changing owner, team, or filters |
| blocked_items | Items the current user cannot operate on or whose state forbids operation |
| operation_id | Tracking ID for this bulk operation |

## API Documentation Rules

- When adding an interface, update the contract-test scope in `TEST.md`.
- When adding an error code, update this file's error-shape notes.
- When changing permission semantics, update the permission boundary in `ARCHITECTURE.md`.
- When implementation and interface docs diverge, create a semantically named `fix_<desc>.md`, then move the work into `TODO.md`.
