# ADR-0002 AuthN/AuthZ Model

- Status: Accepted
- Date: 2026-03-01
- Deciders: SecurityArchitectureGroup

## Context
- multi-tenant API requires strict tenant boundary enforcement
- service must integrate with enterprise OIDC identity provider
- security controls must support auditability and least privilege

## Decision
- Authentication: OIDC JWT bearer tokens validated via JWKS
- Authorization: scope + tenant claim enforcement in application layer
- Required scopes: `Reminder:Read`, `Reminder:Write`, `Reminder:Admin`
- Service identity: mTLS certificates for east-west requests

## Consequences
- positive: centralized identity lifecycle and revocation support
- positive: explicit permission model per command surface
- negative: token validation cache and key rotation complexity

## Related
- [Security Model](../architecture/security-model.md)
- [ADR-0001 Base Architecture](0001-base-architecture.md)
