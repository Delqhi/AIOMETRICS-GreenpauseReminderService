# 12 Glossary

- Reminder: domain entity representing a user reminder intent.
- ScheduleRule: value object defining reminder timing semantics.
- DispatchRecord: immutable record of one channel send attempt.
- OutboxEvent: event persisted in same transaction as state mutation.
- Shard: partition unit used for tenant-local workload scaling.
