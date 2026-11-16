# 🔌 API contract and conventions

## Current status

No public API route is implemented. The expected package entry point does not exist, and there are no route handlers.

## Route contract requirements

Each route needs:
- Stable route ID.
- Public path, method, and API version.
- Owning Tripzy service and owner.
- Trusted upstream service identifier/path.
- Authentication mode and required permissions.
- Request/response schemas and payload limits.
- Status/error mapping.
- Timeout and retry policy.
- Idempotency requirements.
- Rate-limit group.
- Contract/integration test reference.

## Proposed route register

No real routes are registered. Use the [service contract register](./service-contracts.md) before implementing any endpoint. Generic examples are not proof of existing services.

## HTTP semantics

- GET and HEAD should be safe and read-only.
- POST should not be automatically retried.
- PUT may be retried only if the owning contract guarantees idempotency.
- PATCH retry behavior must be explicit.
- DELETE may have downstream side effects and requires service-owner confirmation.

## Proposed public error envelope

```json
{
  "error": {
    "code": "UPSTREAM_TIMEOUT",
    "message": "The requested service did not respond in time.",
    "correlationId": "req_example_123"
  }
}
```

This is a proposed contract, not an implemented response.

## Status code conventions

| Status | Intended use |
|---|---|
| 200 | Success with response body |
| 201 | Resource created |
| 202 | Asynchronous operation accepted |
| 204 | Success without response body |
| 400 | Malformed input |
| 401 | Missing or invalid authentication |
| 403 | Insufficient permission |
| 404 | Unknown route/resource |
| 409 | Domain conflict |
| 413 | Request too large |
| 422 | Semantically invalid input |
| 429 | Rate limit exceeded |
| 500 | Gateway internal failure |
| 502 | Invalid or failed upstream response |
| 503 | Dependency unavailable |
| 504 | Upstream deadline exceeded |

## Header policy

Never forward every incoming header. Allowlist required values, remove hop-by-hop headers, ignore client-supplied identity claims, and create gateway-owned request/correlation metadata only after validation. Do not blindly forward cookies.

## Versioning

Prefer explicit versioned paths or a documented media-type strategy. Define deprecation windows and compatibility expectations with owning teams. Do not silently route a stable public contract to a breaking upstream version.

## Contract review checklist

- [ ] Service owner and contract confirmed.
- [ ] Method/path/version approved.
- [ ] Authentication and permission documented.
- [ ] Request/response schemas documented.
- [ ] Payload and response limits set.
- [ ] Timeout/retry/idempotency reviewed.
- [ ] Error mapping and rate quota defined.
- [ ] Unit, contract and negative tests added.
