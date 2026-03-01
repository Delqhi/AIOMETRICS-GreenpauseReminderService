# ADR-0001 Base Architecture

- Status: Accepted
- Date: 2026-03-01
- Deciders: LeadSoftwareArchitect, SeniorSecurityEngineer
- TechnicalStory: Establish an implementation-agnostic foundation for ReminderService with strict domain isolation and secure, scalable operations.

## Context
- Requirement: Design-first delivery with Greenpause gate.
- Requirement: Clear separation of business invariants from I/O and framework concerns.
- Requirement: Multi-channel notification dispatch with deterministic scheduling.
- Constraint: Auditable security controls (mTLS + JWT) from day zero.
- Constraint: Horizontal scaling by tenant-based sharding.

## Decision
- ArchitecturePattern: Hexagonal Architecture with Domain-Driven Design boundaries.
- Layering:
  - `internal/domain`: Entities, ValueObjects, DomainServices, DomainEvents.
  - `internal/application`: UseCases, Commands, Queries, Port interfaces.
  - `internal/infrastructure`: Adapter implementations and transport/persistence details.
- ContractStrategy: Contract-first API in `api/` using OpenAPI 3.1.
- RuntimeStack:
  - ServiceRuntime: Go 1.24 (planned)
  - PrimaryStore: PostgreSQL 16
  - SchedulerIndex: Redis 7
  - EventTransport: NATS JetStream (or equivalent durable stream)
  - Observability: OpenTelemetry + Prometheus + structured logs
- SecurityModel:
  - NorthSouth: TLS 1.3 + JWT (OIDC)
  - EastWest: mTLS enforced for service identity
  - Authorization: Tenant-bound RBAC scopes

## Alternatives Considered
1. Layered MVC with direct ORM usage in handlers
- Rejected: leaks persistence concerns into business logic and weakens invariants.
2. Event-sourcing as initial architecture
- Rejected: increases operational complexity before clear need for full event replay semantics.
3. Monolithic queue-only scheduler without relational source of truth
- Rejected: weak auditability and difficult reconciliation.

## Consequences
- Positive:
  - Strong isolation of domain invariants.
  - Replaceable infrastructure adapters.
  - Testability via ports and deterministic use-case orchestration.
- Negative:
  - Higher initial design/documentation load.
  - Additional adapter boilerplate.
- Neutral:
  - Requires disciplined governance on cross-layer dependencies.

## Compliance and Quality Impact
- Security: mTLS and JWT are mandatory non-optional controls.
- Performance: latency and scheduling SLO budgets are first-class architecture inputs.
- Scalability: shard-aware routing and partition ownership required in worker design.
- Operability: all critical state transitions must emit audit and observability signals.

## Decision Validation Criteria
- No forbidden dependencies from `domain` to `infrastructure`.
- API contracts lint clean and versioned.
- RFC-defined scheduling invariants testable as acceptance criteria.
- NFR dashboards include latency, lag, duplicate rate, and failure budget.

## Related Documents
- [System Overview](../architecture/system-overview.md)
- [Stack Profile](../architecture/stack-profile.md)
- [Security Model](../architecture/security-model.md)
- [RFC-0001 Core Logic](../rfc/0001-core-logic.md)
- [README](../../README.md)
