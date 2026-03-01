# RFC-0002 Failure Modes and Recovery

- Status: Draft
- Date: 2026-03-01
- Owners: ReliabilityEngineeringGroup

## Objective
Define deterministic recovery behavior for scheduler, dispatch, persistence, and provider failures.

## Failure Modes
| FailureMode | DetectionSignal | RecoveryAction | SLOImpactBudget |
|---|---|---|---|
| RedisUnavailable | lock/index timeout rate spike | fallback to delayed poll + alert | <= 10 min degraded |
| PostgresPrimaryFailover | write error burst + replica promotion event | retry with jitter + connection re-resolve | <= 5 min write brownout |
| EventBusPartition | consumer lag and ack timeout | partition restart + replay from durable stream | <= 15 min lag |
| ProviderRateLimited | HTTP 429 trend | per-channel backoff + token bucket | <= 1.5x dispatch lag |
| WorkerCrashLoop | pod restart count and heartbeat gap | isolate shard lease and reschedule work | <= 2 min shard outage |

## Recovery Invariants
- no dispatch without durable schedule state
- no duplicate visible notification beyond idempotency budget
- every retry attempt emits an audit and metric event
- dead-lettered event must include deterministic replay key

## Operational Runbook Sequence
1. identify failing subsystem from alert fingerprint
2. freeze risky maintenance commands for affected shard set
3. verify idempotency lock health and outbox consistency
4. trigger controlled replay by `TenantId` shard scope
5. validate SLO recovery and close incident

## Observability Requirements
- mandatory metrics:
  - `scheduler.lock.acquire.fail_total`
  - `outbox.replay.events_total`
  - `dispatch.deadletter.total`
  - `provider.http_429.total`
- mandatory traces:
  - command-to-dispatch end-to-end trace continuity
- mandatory logs:
  - include `TenantId`, `ShardId`, `ReminderId`, `TraceId`, `IncidentId`

## Rollout Guardrails
- enable replay tooling only after dry-run validation
- require dual approval for bulk replay commands
- enforce max replay rate per shard to avoid cascading failures

## Related
- [RFC-0001 Core Logic](0001-core-logic.md)
- [NFR Profile](../architecture/nfr-profile.md)
- [Security Model](../architecture/security-model.md)
