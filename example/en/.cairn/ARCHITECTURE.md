# Architecture

## Project Structure

Northstar Ops is a separated frontend/backend ticket collaboration example. `ARCHITECTURE.md` records project structure, Cairn file ownership, information flow, code organization, dependency direction, new-code ownership, and startup order; business goals, acceptance scope, interface parameters, and agent execution procedure belong elsewhere.

### Cairn File Ownership

```text
AGENTS.md          agent bootstrap, read, and sync protocol
.cairn/plan.md     project positioning, feature scope, data model, acceptance criteria
.cairn/ARCHITECTURE.md project structure, information flow, code organization, dependency direction
.cairn/TODO.md     active execution steps, subtasks, blockers, verification results
.cairn/fix_*       review, test feedback, and implementation-drift closure
.cairn/fix-plan_*  plan-scope addition or revision closure
.cairn/HUMAN.md    human execution items and directional human decisions
.cairn/INTERFACE.md external interface contracts
.cairn/TEST.md     test method index
.cairn/NICKNAME.md project terminology and aliases
.cairn/archive/    closed history
```

### Information Flow

```text
.cairn/plan.md -> .cairn/TODO.md -> implementation and verification -> .cairn/archive/
.cairn/fix_*.md -> .cairn/TODO.md -> fix closure -> .cairn/plan.md corrections as needed -> .cairn/archive/
.cairn/fix-plan_*.md -> .cairn/plan.md -> .cairn/TODO.md -> .cairn/archive/
.cairn/TODO.md -> .cairn/HUMAN.md -> human feedback -> .cairn/TODO.md
.cairn/INTERFACE.md / .cairn/TEST.md / .cairn/NICKNAME.md sync on trigger
```

## Code Organization

```text
apps/
  web/              Browser workbench, lists, details, bulk actions, audit page
  api/              HTTP API, authentication entry, authorization, ticket writes
  worker/           Notifications, SLA scans, exports, retries, async tasks
packages/
  domain/           Ticket, note, assignment, audit-event domain models
  permissions/      Roles, capabilities, team scopes, authorization checks
  contracts/        API request/response types, error shapes, event payloads
  config/           Environment parsing and shared configuration
db/
  migrations/       Database migrations
  seeds/            Local and test seed data
```

## New-Code Ownership

| New Item | Location |
| --- | --- |
| Pages, forms, list state, browser interactions | `apps/web` |
| HTTP routes, request validation, response assembly | `apps/api` |
| Async tasks, queue consumers, retry logic | `apps/worker` |
| Ticket state, notes, assignments, audit domain rules | `packages/domain` |
| Capabilities, role scope, authorization checks | `packages/permissions` |
| Shared frontend/backend types, error codes, event payloads | `packages/contracts` |
| Environment config, startup config, shared constants | `packages/config` |
| Schema changes | `db/migrations` |

If new code has no clear owner, update this file before implementing it.

## Dependency Direction

```text
apps/web    -> packages/contracts
apps/api    -> packages/domain -> packages/permissions
apps/api    -> packages/contracts
apps/worker -> packages/domain -> packages/contracts
apps/*      -> packages/config
```

- `apps/web` does not depend directly on `packages/domain` or `packages/permissions`.
- `packages/domain` does not depend on `apps/*`.
- `packages/contracts` does not depend on application code.
- `apps/worker` does not own requests that must return synchronously to users.

## Startup Order

Local development starts in this order:

1. Load environment config from `packages/config`.
2. Start the database and run `db/migrations`.
3. Start `apps/api` and expose HTTP endpoints.
4. Start `apps/worker` and connect queues/background jobs.
5. Start `apps/web` and access services through the API.

## Extension Points

- New external interface: define the contract in `packages/contracts`, implement the route in `apps/api`, and update `.cairn/INTERFACE.md`.
- New async flow: define the event payload in `packages/contracts`, then add the consumer in `apps/worker`.
- New permission capability: add it to `packages/permissions` first, call it from `apps/api`; the frontend only consumes returned capabilities.
- New table: write a migration in `db/migrations` and put related domain objects in `packages/domain`.
