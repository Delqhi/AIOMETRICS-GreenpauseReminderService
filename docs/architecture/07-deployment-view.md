# 07 Deployment View

## Deployment Units
- API deployment
- Scheduler worker deployment
- Dispatch worker deployment
- Postgres stateful set / managed service
- Redis cache / managed service
- NATS cluster / managed service

## Platform Baseline
- Kubernetes multi-zone cluster
- Terraform-managed cloud resources
- Helm release per environment

## Deployment Diagram
```mermaid
flowchart LR
    subgraph RegionA[Region-A]
        APIA[API Pods]
        SCHA[Scheduler Pods]
        DSPA[Dispatch Pods]
        PGA[(Postgres Primary)]
        RDA[(Redis)]
        NAA[(NATS Cluster)]
    end

    subgraph RegionB[Region-B Standby]
        APIB[API Pods Standby]
        SCHB[Scheduler Pods Standby]
        DSPB[Dispatch Pods Standby]
        PGB[(Postgres Replica)]
        RDB[(Redis Replica)]
        NAB[(NATS Mirror)]
    end

    APIA --> PGA
    SCHA --> RDA
    SCHA --> NAA
    DSPA --> NAA
    PGA --> PGB
    NAA --> NAB
    RDA --> RDB
```
