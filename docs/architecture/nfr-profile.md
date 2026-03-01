# NFR Profile

## Performance Budgets
| Metric | Target | Window |
|---|---|---|
| CommandWriteLatencyP95 | <= 120 ms | 5m |
| CommandWriteLatencyP99 | <= 250 ms | 5m |
| DueToDispatchLagP99 | <= 1,000 ms | 5m |
| SchedulerScanCycle | <= 250 ms | 1m |

## Reliability Budgets
| Metric | Target |
|---|---|
| DuplicateDispatchRate | < 0.01% |
| AvailabilityMonthly | >= 99.95% |
| RPO | <= 5 s |
| RTO | <= 15 min |

## Scalability Budgets
| Metric | Target |
|---|---|
| ReminderWrites | >= 5,000 req/s/region |
| Dispatches | >= 15,000/min/region |
| ShardRebalanceTime | <= 10 min |
