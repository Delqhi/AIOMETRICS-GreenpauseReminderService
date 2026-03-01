# Application Layer Contract

## Allowed Artifacts
- UseCases: command and query handlers
- Ports: repository, event bus, clock, idempotency lock
- DTO mappings between transport contract and domain model

## Responsibilities
- orchestrate domain objects
- enforce transaction boundaries
- coordinate outbox publication

## Forbidden Dependencies
- concrete adapter imports from `internal/infrastructure`
