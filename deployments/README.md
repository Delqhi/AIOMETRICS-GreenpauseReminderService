# Deployments Blueprint

## IaC Targets
- Terraform modules for network, database, and secrets.
- Kubernetes manifests or Helm charts for `ApiGatewayContainer`, `SchedulerWorkerContainer`, and `DispatchWorkerContainer`.

## Deployment Invariants
- mTLS enabled for all internal service traffic.
- PodDisruptionBudget required for each critical worker deployment.
- HorizontalPodAutoscaler uses shard lag and CPU as scaling signals.
- Blue/green rollout with automated rollback on SLO regression.
