# 08 Crosscutting Concepts

## Security
- JWT validation at ingress
- mTLS for east-west traffic
- tenant-scoped authorization checks in application handlers

## Reliability
- idempotent dispatch writes
- bounded retries with jitter
- dead-letter routing for terminal failures

## Operability
- structured logs
- distributed traces
- SLO metrics and burn-rate alerts
