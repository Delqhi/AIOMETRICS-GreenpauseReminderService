# CLI Entrypoint Specification

## Purpose
Operate administrative and maintenance workflows.

## Allowed Commands
- replay outbox events
- reindex due schedules
- run consistency audits

## Constraints
- all mutating operations require explicit tenant scope flags
- all commands must emit immutable audit events
