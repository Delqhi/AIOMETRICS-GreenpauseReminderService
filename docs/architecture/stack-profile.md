# Stack Profile

## Runtime Stack
| Layer | Selected Technology | VersionPolicy |
|---|---|---|
| LanguageRuntime | Go | `1.24.x` pinned minor |
| HTTPTransport | net/http + chi or equivalent | latest stable minor |
| ContractSpec | OpenAPI | `3.1.x` |
| AsyncContract | Protobuf | `v3` |
| PrimaryDatabase | PostgreSQL | `16.x` |
| SchedulerIndex | Redis | `7.x` |
| EventStream | NATS JetStream | `2.x` |
| Tracing | OpenTelemetry | semantic conventions stable |
| Metrics | Prometheus | `v2` query compatibility |
| Packaging | Helm | chart API v2 |
| IaC | Terraform | `>=1.8,<2.0` |

## Version Governance
- dependency updates follow monthly patch window
- major version changes require ADR
- all pinned versions recorded in lockfiles before implementation phase
