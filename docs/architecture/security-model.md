# Security Model

## Identity and Access
- external identity source: OIDC provider
- accepted token type: JWT access token
- required claims: `iss`, `aud`, `sub`, `exp`, `nbf`, `scope`, `tenant_id`

## Transport Security
- north-south traffic: TLS 1.3
- east-west traffic: mTLS with SPIFFE-like service identity
- certificate rotation: 24h automatic rotation

## Data Security
- at rest: AES-256 via managed KMS keys
- in transit: TLS/mTLS mandatory
- secret retrieval: runtime from secret manager only

## Audit and Compliance
- immutable audit events for all mutating commands
- minimum retention: 400 days
- privileged operator actions require reason code

## Threat Controls
- replay protection via idempotency keys and nonce windows
- rate limiting at edge and per-tenant quota controls
- WAF and input schema validation at API boundary
