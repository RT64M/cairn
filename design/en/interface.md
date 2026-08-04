# INTERFACE.md Design

`INTERFACE.md` records concrete definitions for external interfaces, including frontend/backend APIs, HTTP/RPC APIs, CLI commands, and SDK public methods.

## Why It Exists

`plan.md` says which interfaces are needed, but it is not enough for integration. If interface signatures change only in code, external callers and testers lose the contract.

## Should Include

- Interface paths, methods, or command names.
- Request parameters.
- Response shapes.
- Error codes.
- Authentication rules.
- Call examples.
- Compatibility and breaking-change notes.

## Should Not Include

- Private internal module calls.
- Performance optimization notes.
- Implementation details.
- Test results.

## Sync Rule

Every external interface signature change must update `INTERFACE.md` in the same change. Interface changes do not need a `fix_<desc>.md` loop, but breaking changes should add a TODO item to notify callers.
