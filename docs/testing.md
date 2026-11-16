# 🧪 Testing strategy

## Current state
No test framework or test files are present. The package test command is a placeholder that exits with status 1. Replace it with an actual suite before using tests as a release gate.

## Required layers
| Layer | Coverage | Suggested method |
|---|---|---|
| Unit | Route matching, config parsing, header filtering, error mapping | Node test runner or selected framework |
| Middleware | Authentication, authorization, request IDs, limits | Mock requests and responses |
| Contract | Every upstream request/response | OpenAPI/schema-based validation |
| Integration | Gateway against mock upstreams | Local test HTTP servers |
| Security | SSRF, header spoofing, privilege bypass | Adversarial regression cases |
| Resilience | Timeout, disconnect, retry and circuit behavior | Fault-injecting mock upstream |
| Load | Throughput, percentiles, concurrency and quotas | Controlled isolated load test |
| Smoke | Deployment readiness | Safe, non-destructive probes |

## Mandatory security regression tests
- Missing, malformed, expired, wrong-issuer, wrong-audience and invalid-signature tokens.
- Valid identity with insufficient permission.
- Authorization denial occurs before any upstream request.
- Spoofed identity headers are removed.
- Unknown route and unregistered destination fail closed.
- Redirects cannot escape the upstream allowlist.
- Oversized request and response are rejected safely.
- Rate-limit quota returns a documented 429.
- Unsafe mutations are not automatically retried.
- Upstream timeout maps to the documented public error.
- Public response contains no stack trace, internal host, or credential.

## Contract tests
For each registered service route validate method/path, auth requirement, permission, destination service ID, outbound path, request schema, response schema, status mapping, timeout, retry and idempotency behavior.

## Integration tests
Use local mock upstream servers, not production services. Test normal response, slow response, connection reset, malformed body, oversized response, unexpected redirect, upstream 4xx/5xx, and unavailable DNS/network behavior.

## Quality gates
All required tests pass. Lint and syntax checks pass. High/critical dependency advisories are remediated or have an explicit security-approved exception. Contract changes have service-owner approval. Security changes update the threat model.

## Test data
Use deterministic synthetic identities and payloads. Never commit real bearer tokens, customer data, or production URLs.
