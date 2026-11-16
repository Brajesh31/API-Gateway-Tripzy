# ✨ Capability catalogue and implementation status

## How to read this document

The capability descriptions below define intended gateway scope. They are not a feature claim. The current repository contains no gateway runtime source or route handlers.

## Capability matrix

| Capability | Intended behavior | Current implementation |
|---|---|---|
| Central request entry | Provide one public HTTP boundary | Not implemented |
| Route registry | Resolve explicit method/path to service ID | Not implemented |
| Identity verification | Verify signed credentials and claims | Not implemented |
| Route authorization | Enforce permissions before forwarding | Not implemented |
| Request validation | Bound method, headers, content type and body | Not implemented |
| Header policy | Filter inbound and outbound headers | Not implemented |
| Rate limiting | Apply route/client/identity quotas | Not implemented |
| Timeout budgets | Enforce overall and per-service deadlines | Not implemented |
| Retry policy | Retry only safe operations | Not implemented |
| Error normalization | Return stable public errors | Not implemented |
| Correlation IDs | Propagate request-level diagnostic context | Not implemented |
| Health endpoints | Report liveness and readiness | Not implemented |
| Metrics and tracing | Observe latency, failures and dependencies | Not implemented |
| Service contract governance | Require owner and security approval | Documentation only |

## 1. Route aggregation

A future route registry should map a public method/path to a stable route ID, service ID, internal path, authentication requirement, permission set, payload limits, timeout, retry rule, quota policy, and owner. The registry should be validated at startup. Duplicate or ambiguous route definitions should prevent startup.

## 2. Identity verification

The gateway should verify the credential signature and all required claims using an explicitly trusted issuer and key source. Token decoding alone is not verification. Identity-provider outages need a documented fail-closed behavior for protected routes.

## 3. Authorization

Each protected route must declare the permission required. The gateway can enforce coarse route access, but the upstream service must still check ownership and access to individual resources.

## 4. Rate limiting

Quota keys may be based on authenticated subject, API client, tenant, route group, or a carefully trusted network identity. The key and failure policy must be chosen per route risk. Distributed quotas require shared state and an ADR.

## 5. Resilience and deadlines

A request should have one overall deadline and smaller per-upstream timeouts. Concurrency limits should isolate services. Retries must be bounded and idempotency-aware.

## 6. Error normalization

The public error contract should remain stable even when internal services use different error formats. Do not leak upstream stack traces, private hosts, database messages, or raw payloads.

## 7. Correlation and observability

A validated correlation ID links a request across logs and traces. Metrics should use bounded labels such as route ID, service ID, method and status class. Raw tokens and personal data must not appear in telemetry.

## 8. Health and readiness

Liveness indicates that the process is alive and can make progress. Readiness indicates that the instance can accept requests with its current configuration. Dependency health should be reported separately so one downstream outage does not restart every gateway instance.

## 9. Acceptance criteria for a capability

A capability is not complete because its document exists. It is complete only when:
- Source code implements the behavior.
- Configuration is validated and documented.
- Positive and negative tests cover expected behavior.
- Security review addresses relevant abuse cases.
- Operational telemetry and failure behavior are defined.
- Public docs accurately describe the implementation.
