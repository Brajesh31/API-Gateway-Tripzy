# 🔌 API contract and conventions

## Implementation status

No public API endpoint is implemented in this repository. The package's expected `index.js` file does not exist. The routes below are design guidance only.

## Route register requirements

Every route must be registered explicitly and reviewed with its owning service. Each route entry must declare:

| Field | Meaning |
|---|---|
| Route ID | Stable internal identifier |
| Public path | Versioned client-facing path |
| Methods | Exact allowed HTTP methods |
| Upstream ID | Key from trusted service configuration |
| Authentication | Required, optional, or public with rationale |
| Permission | Explicit required scope/role/policy |
| Request contract | Content type, schema, size limit |
| Response contract | Public response envelope and data schema |
| Timeout | Per-service request deadline |
| Retry | Allowed methods/statuses and idempotency behavior |
| Rate limit | Client/user quota group |
| Owner | Service/team responsible for domain operation |

## Proposed route shape

Do not copy this into production without matching it to actual Tripzy contracts.

| Public route example | Owning service | Status |
|---|---|---|
| `GET /api/v1/example` | Undecided | Illustration only |

## HTTP method semantics

- GET: read-only and safe by contract.
- HEAD: same resource metadata semantics as GET where supported.
- POST: create/command; do not automatically retry.
- PUT: full replacement and idempotent only when service contract guarantees it.
- PATCH: partial update; retry safety must be explicitly documented.
- DELETE: potentially idempotent at HTTP semantics level, but downstream side effects must be reviewed.

## Public error envelope

A stable public format should avoid exposing raw upstream bodies:

```json
{
  "error": {
    "code": "UPSTREAM_TIMEOUT",
    "message": "The requested service did not respond in time.",
    "correlationId": "req_example_123"
  }
}
```

This is a proposed shape. Exact fields and status mapping must be adopted consistently before routes are released.

## Status code conventions

| Status | Intended use |
|---|---|
| 200 | Successful request with response body |
| 201 | Resource created |
| 202 | Accepted for asynchronous processing |
| 204 | Successful operation without response body |
| 400 | Malformed request or invalid path/query |
| 401 | Missing or invalid authentication |
| 403 | Authenticated caller lacks permission |
| 404 | Unknown public route or resource |
| 409 | Domain conflict from upstream |
| 413 | Request body exceeds configured limit |
| 422 | Syntactically valid but semantically invalid data |
| 429 | Rate limit exceeded |
| 502 | Invalid or failed upstream response |
| 503 | Gateway or dependency temporarily unavailable |
| 504 | Upstream deadline exceeded |

## Header policy

Never forward all incoming headers blindly. Create an explicit allowlist. Remove hop-by-hop headers and untrusted identity headers. Set gateway-owned correlation and forwarding headers only after validating the request context. Do not forward client-supplied `X-User-Id`, `X-Role`, or similar claims as authoritative identity.

## Versioning

Prefer explicit versioned paths or a clearly documented media-type strategy. Define deprecation windows and compatibility guarantees with service owners. Do not silently route an old public contract to a breaking upstream version.

## Contract review checklist

- [ ] Owner and service contract confirmed.
- [ ] Method/path/version approved.
- [ ] Authentication and permission requirements written.
- [ ] Request and response schemas documented.
- [ ] Payload limits defined.
- [ ] Timeout and retry policy reviewed.
- [ ] Idempotency behavior stated.
- [ ] Error mapping defined.
- [ ] Rate-limit group assigned.
- [ ] Contract and negative tests added.
