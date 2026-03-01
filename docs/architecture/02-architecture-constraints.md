# 02 Architecture Constraints

## Organizational Constraints
- Greenpause gate blocks production code before blueprint approval.
- architecture decisions require ADR records.

## Technical Constraints
- OpenAPI-first external contract
- Hexagonal layering mandatory
- OIDC/JWT required for external auth
- mTLS required for internal transport
- tenant sharding required before multi-region rollout
