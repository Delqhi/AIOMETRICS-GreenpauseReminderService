# Infrastructure Layer Contract

## Allowed Artifacts
- Postgres repositories
- Redis lock/index adapters
- NATS event adapters
- OIDC/JWKS clients
- notification provider clients

## Responsibilities
- implement application ports
- handle retries, timeouts, and circuit breakers
- translate transport and persistence errors to application-safe failures

## Constraints
- no domain invariant definitions
