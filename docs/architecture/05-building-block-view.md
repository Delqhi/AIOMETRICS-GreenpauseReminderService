# 05 Building Block View

## Level 1 Blocks
- `ApiGatewayContainer`
- `SchedulerWorkerContainer`
- `DispatchWorkerContainer`
- `PostgresContainer`
- `RedisContainer`
- `EventBusContainer`

## Level 2 Internal Blocks
- domain: `Reminder`, `ScheduleRule`, `DispatchPolicy`
- application: `CreateReminderUseCase`, `SnoozeReminderUseCase`, `CancelReminderUseCase`
- infrastructure: `PostgresReminderRepository`, `RedisDueIndex`, `NatsEventPublisher`

## Diagram
```mermaid
flowchart TB
    subgraph ReminderService
        API[ApiGatewayContainer]
        SCH[SchedulerWorkerContainer]
        DSP[DispatchWorkerContainer]
        BUS[(EventBusContainer)]
        DB[(PostgresContainer)]
        IDX[(RedisContainer)]
    end

    API --> DB
    API --> BUS
    SCH --> IDX
    SCH --> BUS
    SCH --> DB
    DSP --> BUS
    DSP --> DB
```
