# 🧾 Upstream service contract register

## 1. Current register status

No upstream service names, URLs, owners, or public routes are confirmed or configured in this repository. Do not infer microservice boundaries from the Tripzy product name or fabricate route definitions.

| Service ID | Owner | Environment key | Public routes | Identity model | Timeout | Status |
|---|---|---|---|---|---|---|
| None registered | TBD | TBD | None | TBD | TBD | Not configured |

## 2. Why the register is mandatory

The route registry is the gateway's outbound trust boundary. A route should never forward to a host selected by the caller. A reviewed register ties each public operation to a known service owner, destination identifier, permission policy, timeout, quota, and contract test.

## 3. Route contract template

~~~yaml
routeId: example-resource-read
public:
  method: GET
  path: /api/v1/example-resources/{resourceId}
upstream:
  serviceId: example-service
  path: /internal/example-resources/{resourceId}
security:
  authentication: required
  permissions:
    - example:read
request:
  contentType: application/json
  maxBytes: 16384
response:
  contentType: application/json
  maxBytes: 262144
resilience:
  timeoutMs: 2000
  retries: 0
  idempotency: not-required
rateLimit:
  policyId: authenticated-read
ownership:
  team: TBD
  contractDocument: TBD
  escalation: TBD
~~~

This is a template, not active configuration. The service ID, paths, timeout, and policy values are illustrative.

## 4. Required owner confirmation

1. Stable service ID and technical owner.
2. Internal base URL and network boundary for each environment.
3. Allowed methods and paths.
4. Service-to-service identity requirements.
5. User permission scopes and resource-level authorization.
6. Request and response schemas.
7. Payload size, streaming, and upload behavior.
8. Latency and timeout budget.
9. Retryability and idempotency guarantees.
10. Rate and concurrency limits.
11. Error semantics and health behavior.
12. Contract test environment and escalation channel.
13. Data classification and logging restrictions.
14. Version/deprecation expectations.

## 5. Destination safety requirements

- Destinations come from trusted deployment configuration.
- Never accept target URL/hostname from request path, query, body, or headers.
- Prefer fixed service IDs over arbitrary destinations.
- Validate scheme, hostname, port, and resolved address ranges.
- Restrict redirects and revalidate every redirect target if enabled.
- Protect against DNS rebinding.
- Block metadata endpoints and unapproved private networks.
- Reinforce application allowlists with network egress policy.

## 6. Route registration lifecycle

~~~mermaid
stateDiagram-v2
  [*] --> Proposed
  Proposed --> OwnerReview
  OwnerReview --> SecurityReview
  SecurityReview --> ContractTesting
  ContractTesting --> Approved
  ContractTesting --> ChangesRequested
  ChangesRequested --> Proposed
  Approved --> Staging
  Staging --> Active
  Active --> Deprecated
  Deprecated --> Retired
  Retired --> [*]
~~~

## 7. Register maintenance

When a service changes its contract, the owner must notify gateway maintainers. Update route mapping, schema, timeout, permissions, tests, error translation, and documentation together. A removed upstream route must be retired through the public API deprecation process.

## 8. Current implementation rule

Until real contracts are supplied, keep the register empty. Empty is more accurate and safer than a fabricated list of Tripzy services.
