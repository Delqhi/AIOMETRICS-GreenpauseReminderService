# 11 Risks and Technical Debt

## Risks
- provider API rate-limit variance causing lag spikes
- shard skew from high-activity tenants
- clock drift across worker nodes

## Debt Register
- advanced per-channel circuit breaking deferred
- tenant quiet-hours policy engine deferred
- multi-region active-active conflict handling deferred
