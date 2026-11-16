# 🧾 Upstream service contract register

## Purpose

The gateway must route only to known Tripzy services. This register is the review point for service ownership, public routes, upstream destinations, authentication, failure budgets, and contract tests.

## Current state

No upstream services or URLs are configured in this repository. Do not infer service names or endpoints from the product name. Service details remain undecided until confirmed by their owners.

## Service register

| Service ID | Owner | Base URL key | Public routes | Auth model | Timeout | Status |
|---|---|---|---|---|---|---|
| None registered | TBD | TBD | None | TBD | TBD | Not configured |

## Route registration template

Copy this template for each approved route:

```yaml
routeId: example-read
public:
  method: GET
  path: /api/v1/example
upstream:
  serviceId: example-service
  path: /internal/example
security:
  authentication: required
  permissions:
    - example:read
request:
  contentType: application/json
  maxBytes: 16384
resilience:
  timeoutMs: 2000
  retries: 0
  idempotencyRequired: false
rateLimit:
  policyId: authenticated-read
ownership:
  team: TBD
  contractDocument: TBD
```

This is a template, not active configuration.

## Required owner confirmation

Before a service is registered, collect:
1. Service name and technical owner.
2. Internal base URL and network exposure rules.
3. Allowed methods and paths.
4. Token validation and service-to-service identity requirements.
5. Permission/scopes and resource-level authorization requirements.
6. Expected request and response schema.
7. Maximum payload and upload behavior.
8. Expected latency and timeout budget.
9. Retryability and idempotency guarantees.
10. Rate and concurrency limits.
11. Error semantics and dependency health endpoint.
12. Contract test environment and support escalation.

## Destination safety

- Destination URLs must come from trusted deployment configuration.
- Reject destinations supplied through path, query, body, or headers.
- Prefer a fixed service ID to a client-selected URL.
- Validate scheme, hostname, port, and allowed network ranges at configuration time.
- Protect against DNS rebinding where destination resolution is dynamic.
- Never permit the gateway to reach cloud metadata endpoints or arbitrary private addresses unless explicitly required and isolated.

## Contract lifecycle

```mermaid
stateDiagram-v2
    [*] --> Proposed
    Proposed --> OwnerReview
    OwnerReview --> SecurityReview
    SecurityReview --> ContractTest
    ContractTest --> Approved
    ContractTest --> ChangesRequested
    ChangesRequested --> Proposed
    Approved --> Active
    Active --> Deprecated
    Deprecated --> Retired
    Retired --> [*]
```

Keep historical route decisions and deprecation dates in version control or an approved change record.
