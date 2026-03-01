# Greenpause Reminder Service Blueprint

## Status
- Phase: Greenpause
- ImplementationGate: CLOSED
- SourceCodePolicy: No Go, TypeScript, or Rust production code before architecture sign-off.

## High-Level Overview
- ServiceName: `ReminderService`.
- ProductCapability: Create, schedule, snooze, and cancel reminders.
- PrimaryActors: `EndUser`, `Operator`.
- ExternalSystems: `OIDCProvider`, `EmailProvider`, `SmsProvider`, `PushProvider`.
- ArchitectureStyle: Hexagonal Architecture + Domain-Driven Design + Contract-First API.

## Portfolio Integration
- CanonicalDirectory: `09-GreenpauseReminderService`.
- StableKey: `09-greenpause-reminder-service`.
- RootPolicy: Conforms to `NN-ProjectName` naming standard.

## Architecture Guardrails
- DomainPurity: `internal/domain` contains only `Entities`, `ValueObjects`, `DomainServices`, `DomainEvents`.
- PortIsolation: `internal/application` depends only on domain abstractions and port interfaces.
- AdapterContainment: `internal/infrastructure` owns all I/O and framework dependencies.
- ContractFirst: `api/` artifacts are the external interface source of truth.
- DeterministicScheduling: reminder dispatch time derives only from persisted schedule state.

## Repository Layout
```text
09-GreenpauseReminderService/
├── api/
│   ├── openapi/
│   └── proto/
├── cmd/
│   ├── server/
│   ├── worker/
│   └── cli/
├── internal/
│   ├── domain/
│   ├── application/
│   └── infrastructure/
├── pkg/
├── docs/
│   ├── architecture/
│   ├── adr/
│   └── rfc/
├── deployments/
└── configs/
```

## Target Stack Profile
- Runtime: `Go 1.24.x`.
- API: `OpenAPI 3.1` for REST + `Protobuf` for async/stream contracts.
- Persistence: `PostgreSQL 16.x`.
- Scheduler Index + Locking: `Redis 7.x`.
- Event Backbone: `NATS JetStream`.
- AuthN: OIDC + JWT (RS256/ES256).
- AuthZ: Tenant-scoped RBAC.
- Observability: OpenTelemetry + Prometheus + Loki/ELK.
- Delivery Platform: Kubernetes + Terraform + Helm.

## Dev Environment Setup
1. Install tooling:
   - `git >= 2.45`
   - `go >= 1.24`
   - `node >= 22`
   - `docker >= 26`
2. Enter repository:
   - `cd /Users/jeremyschulze/dev/AIOMETRICS/09-GreenpauseReminderService`
3. Validate blueprint assets:
   - `npx markdownlint-cli2 "**/*.md"`
   - `npx @redocly/cli lint api/openapi/reminder-v1.yaml`
   - `buf lint api/proto`

## Build Pipeline Commands
| Stage | Command | Gate |
|---|---|---|
| DocsLint | `npx markdownlint-cli2 "**/*.md"` | Markdown quality and structure |
| ContractLintOpenAPI | `npx @redocly/cli lint api/openapi/reminder-v1.yaml` | OpenAPI semantic validity |
| ContractLintProto | `buf lint api/proto` | Protobuf conventions and compatibility |
| ArchitectureLinkCheck | `npx markdown-link-check docs/**/*.md README.md` | Internal reference integrity |
| SecuritySpecCheck | `rg -n "mTLS|JWT|Sharding|SLO|RPO|RTO" docs` | Mandatory NFR/security vocabulary present |

## NFR Baseline
| NFR | Target | EnforcementPoint |
|---|---|---|
| ApiP95Latency | <= 120 ms | SLO dashboard + load-test gate |
| ApiP99Latency | <= 250 ms | canary burn-rate alert |
| DueToDispatchLagP99 | <= 1,000 ms | scheduler lag metrics |
| Throughput | >= 15,000 reminders/minute/region | sustained load profile |
| DuplicateDispatchRate | < 0.01% | idempotency audit job |
| Availability | 99.95% monthly | multi-zone SLO/error budget |
| DataLossRPO | <= 5 s | WAL + outbox replication |
| RecoveryRTO | <= 15 min | failover drill |

## Security Baseline
- TransportSecurity: TLS 1.3 external; mTLS internal service-to-service.
- IdentityModel: OIDC access token with JWT validation (`iss`, `aud`, `exp`, `nbf`, `scope`).
- AuthorizationModel: tenant-bound RBAC with `Reminder:Write` and `Reminder:Read` scopes.
- DataProtection: AES-256 at rest; KMS-backed key rotation every 90 days.
- SecretManagement: runtime secret injection only; no plaintext credentials in repository.
- Auditability: immutable `AuditEvent` stream for create/update/snooze/cancel/dispatch operations.

## Scaling Baseline
- PartitionKey: `TenantId` for primary shard routing.
- SecondarySpread: `UserId` hash for hot partition mitigation.
- QueueTopology: region-local queues with cross-region replay on failover.
- ReadScaling: read replicas for query traffic; writes pinned to shard primary.
- Rebalancing: virtual shards for live redistribution under skew.

## Documentation Index
- [Architecture Index](docs/architecture/README.md)
- [System Overview](docs/architecture/system-overview.md)
- [Stack Profile](docs/architecture/stack-profile.md)
- [NFR Profile](docs/architecture/nfr-profile.md)
- [Security Model](docs/architecture/security-model.md)
- [ADR Index](docs/adr/README.md)
- [ADR-0001 Base Architecture](docs/adr/0001-base-architecture.md)
- [ADR-0002 AuthN/AuthZ Model](docs/adr/0002-authn-authz-model.md)
- [ADR-0003 Partitioning Strategy](docs/adr/0003-partitioning-strategy.md)
- [RFC-0001 Core Logic](docs/rfc/0001-core-logic.md)
- [RFC-0002 Failure Modes and Recovery](docs/rfc/0002-failure-modes-recovery.md)
- [API Contract OpenAPI](api/openapi/reminder-v1.yaml)
- [API Contract Proto](api/proto/reminder_v1.proto)
