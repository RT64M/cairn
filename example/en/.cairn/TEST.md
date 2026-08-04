# Test Strategy

## Test Layers

| Layer | Goal | Example Scope |
| --- | --- | --- |
| Unit tests | Verify pure logic such as state machines, permission checks, error mapping | Ticket state transitions, error-code mapping |
| Contract tests | Verify stable frontend/backend API contracts | Bulk-assignment result, note-error shape |
| Integration tests | Verify database, API, and queue cooperation | Audit-event writes, notification enqueue |
| End-to-end tests | Verify critical user flows | View tickets after login, bulk assign, add note |
| Manual verification | Cover real external environments agents cannot access | Enterprise SSO, real notifications, production permission spot checks |

## Current Regression Matrix

| Scenario | Coverage | Status |
| --- | --- | --- |
| Operations lead bulk assignment fully succeeds | Contract test, end-to-end test | covered |
| Bulk assignment partially fails | Contract test, end-to-end test | pending 8.6.3 |
| Normal support cannot enter audit page | Manual verification | HUMAN.md#H-20260506-01 |
| Note matches sensitive words and writes audit | Integration test | pending 9.3.2 |
| Export job creation and expiration | Integration test | covered |
| Missing i18n key detection | Static check | covered |

## Acceptance Rules

- API shape changes must update contract tests.
- Permission changes must include no-permission scenarios.
- User-visible copy changes must check i18n keys.
- External accounts, real notifications, or pre-production environments move to `HUMAN.md`.
- A TODO item must not be marked `[x]` before verification passes.

## Manual Testing Boundary

Manual testing only handles real-environment items agents cannot complete. Anything possible through documentation, static checks, unit tests, contract tests, or simulated environments does not go into `HUMAN.md`.
