# Helm Blueprint

## Planned Charts
- reminder-api
- reminder-scheduler-worker
- reminder-dispatch-worker

## Invariants
- liveness/readiness probes mandatory
- HPA required for each workload
- PodDisruptionBudget required for critical workers
