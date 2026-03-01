# 04 Solution Strategy

## Strategy Decisions
- isolate business invariants in domain layer
- orchestrate side effects through application ports
- implement side effects in infrastructure adapters
- use outbox pattern for atomic state change + event publication
- use shard-aware workers for horizontal scale
