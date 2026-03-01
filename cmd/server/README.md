# Server Entrypoint Specification

## Purpose
Bootstrap HTTP API runtime for command and query endpoints.

## Responsibilities
- load runtime configuration and secrets
- initialize dependency graph via application ports
- expose health/readiness probes
- start graceful shutdown orchestration

## Forbidden Responsibilities
- domain rule execution
- direct SQL statements in transport handlers
