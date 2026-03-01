# Worker Entrypoint Specification

## Purpose
Bootstrap scheduler and dispatch workers.

## Responsibilities
- claim shard assignments
- run due-scan loops
- execute event consumer loops
- publish liveness and lag metrics

## Forbidden Responsibilities
- API token issuance
- business invariant mutation outside application use-cases
