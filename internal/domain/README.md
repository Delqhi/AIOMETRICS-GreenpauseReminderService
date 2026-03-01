# Domain Layer Contract

## Allowed Artifacts
- Entities: `Reminder`
- ValueObjects: `ScheduleRule`, `DeliveryChannel`, `TenantScope`
- DomainServices: `ScheduleCalculator`
- DomainEvents: `ReminderScheduled`, `ReminderCanceled`

## Invariants
- no cross-tenant entity references
- state transitions are explicit and finite
- recurring schedule emits strictly monotonic execution instants

## Forbidden Dependencies
- network clients
- SQL drivers
- queue SDKs
- framework annotations
