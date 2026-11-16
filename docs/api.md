# 🔌 Public API design and contract reference

> **Implementation status:** No public endpoint is currently implemented. This document defines the contract discipline required before a route is exposed. It must not be used as a list of live Tripzy endpoints.

## 1. Public API principles

The gateway should present stable, versioned HTTP contracts that are independent of internal service hostnames and deployment topology. The public contract must be owned jointly by gateway maintainers and the service team responsible for the domain operation.

Every route needs an explicit method/path match, stable route ID, service owner, destination ID, authentication policy, permission requirements, request/response schema, error mapping, timeout, retry/idempotency policy, quota, and tests.

## 2. Route registry model

A route record is a reviewed configuration object. A conceptual example:

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
resilience:
  timeoutMs: 2000
  retries: 0
  idempotency: not-required
rateLimit:
  policyId: authenticated-read
ownership:
  team: TBD
  contract: TBD
~~~

This is a schema illustration only. It is not active route configuration and contains no real Tripzy service.

## 3. HTTP method semantics

| Method | Intended semantics | Retry default |
|---|---|---|
| GET | Retrieve a representation | May be retried within budget if safe |
| HEAD | Retrieve headers without response body | May be retried within budget |
| POST | Create or execute an operation | No automatic retry by default |
| PUT | Replace resource state | Only if service contract guarantees idempotency |
| PATCH | Partially modify resource | Explicit contract decision required |
| DELETE | Remove or deactivate a resource | Explicit contract decision required |
| OPTIONS | Discover supported behavior/CORS preflight | Handle at a clearly defined boundary |

Do not infer idempotency from a method name alone. A service owner must confirm the operation's behavior.

## 4. Request contract

Document:
- Path parameters and their format.
- Query parameters, cardinality, defaults, and allowed values.
- Required headers and content types.
- Authentication requirements.
- Request body schema and maximum size.
- Validation rules and safe field-level errors.
- Idempotency key behavior where applicable.
- Pagination, sorting, filtering, and cursor semantics where applicable.

## 5. Response contract

Document:
- Success status codes.
- Response schema and required/optional fields.
- Content type and encoding.
- Pagination metadata.
- Cache semantics, if any.
- Response size limit.
- Headers that may be exposed to clients.
- Stable error codes and status mapping.

## 6. Status code policy

| Status | Gateway meaning |
|---:|---|
| 200 | Successful request with representation |
| 201 | Resource created |
| 202 | Work accepted for asynchronous processing |
| 204 | Successful request with no body |
| 400 | Malformed or invalid request syntax |
| 401 | Missing or invalid authentication |
| 403 | Authenticated caller lacks permission |
| 404 | Public route or resource not found |
| 409 | Conflict with current resource state |
| 413 | Request payload exceeds configured limit |
| 415 | Unsupported media type |
| 422 | Syntactically valid but semantically invalid input |
| 429 | Rate or concurrency policy exceeded |
| 500 | Unexpected gateway failure |
| 502 | Invalid or failed upstream response |
| 503 | Required upstream unavailable |
| 504 | Upstream deadline exceeded |

The gateway must not turn every upstream error into 200 or expose arbitrary upstream status codes without a documented mapping.

## 7. Error response contract

A proposed error envelope:

~~~json
{
  "error": {
    "code": "UPSTREAM_TIMEOUT",
    "message": "The requested service did not respond in time.",
    "correlationId": "req_example_123"
  }
}
~~~

Machine-readable codes are stable identifiers. Human-readable messages may evolve within the compatibility policy. Never return stack traces, internal hosts, credentials, database diagnostics, or raw upstream bodies.

## 8. Header policy

Use explicit allowlists. Remove hop-by-hop headers, untrusted identity headers, unsupported forwarded metadata, and cookies that are not required by the contract. Validate trace context and request IDs. Never blindly forward client-supplied Host, X-Forwarded-For, X-User-Id, X-Role, or equivalent identity metadata.

## 9. Pagination and large responses

For list routes, define page size limits, cursor behavior, ordering guarantees, and stable pagination under concurrent writes. Large exports should use an explicit streaming contract and dedicated resource limits rather than silently bypassing response limits.

## 10. Versioning and deprecation

Choose one public versioning approach and document it. Breaking changes require a migration plan, consumer notice, deprecation window, and contract tests. See [versioning and compatibility](./versioning-and-compatibility.md).

## 11. API security checklist

- [ ] Route exists in a reviewed registry.
- [ ] Service owner and destination are confirmed.
- [ ] Auth mode and permissions are explicit.
- [ ] Resource-level checks remain in the owning service.
- [ ] Request and response schemas are bounded.
- [ ] Error mapping is stable and safe.
- [ ] Timeout and retry behavior are approved.
- [ ] Quota policy and key are defined.
- [ ] Contract, integration, and negative security tests exist.

## 12. Current route inventory

There are no implemented or verified routes in this repository. Do not publish example paths as active endpoints. Populate the route register only from approved Tripzy service contracts.
