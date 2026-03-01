# ADR-0003 Partitioning Strategy

- Status: Accepted
- Date: 2026-03-01
- Deciders: PlatformArchitectureGroup

## Context
- throughput target requires horizontal scaling beyond single database node
- tenant isolation must be preserved under scale

## Decision
- primary partition key: `TenantId`
- shard function: `ShardId = Hash(TenantId) mod N`
- secondary spread: `UserId` hash for hot-tenant mitigation
- worker ownership: shard leasing with heartbeat and failover timeout

## Consequences
- positive: linear scale-out with additional shard capacity
- positive: deterministic routing and reduced cross-shard joins
- negative: rebalancing operations add operational complexity

## Related
- [NFR Profile](../architecture/nfr-profile.md)
- [ADR-0001 Base Architecture](0001-base-architecture.md)
