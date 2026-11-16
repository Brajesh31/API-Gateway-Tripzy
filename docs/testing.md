# 🧪 Testing strategy and verification plan

## 1. Current status

No application test framework or test files are present. The current npm test script is an intentional placeholder that exits with status 1. Replace it with a real suite before treating tests as a release gate.

## 2. Test pyramid

~~~mermaid
flowchart TD
  E2E[Small number of end-to-end smoke tests]
  Contract[Service contract and integration tests]
  Middleware[Middleware and policy tests]
  Unit[Large base of unit tests]
  Unit --> Middleware --> Contract --> E2E
~~~

Most policy logic should be verified with fast deterministic unit tests. Network and end-to-end tests should use local mocks and controlled environments.

## 3. Required test layers

| Layer | Coverage | Example method |
|---|---|---|
| Unit | Route matching, config parsing, header filtering, error mapping | Node test runner or selected framework |
| Middleware | Authentication, authorization, request IDs, limits | Mock request/response adapters |
| Contract | Every upstream request/response | OpenAPI/schema validation |
| Integration | Gateway against upstream stubs | Local HTTP test servers |
| Security | SSRF, header spoofing, privilege bypass | Adversarial regression tests |
| Resilience | Timeout, disconnect, retry and circuit behavior | Fault-injecting mock upstream |
| Load | Throughput, percentiles, concurrency and quotas | Isolated controlled load test |
| Smoke | Deployed readiness | Safe, non-destructive probes |

## 4. Unit test catalogue

- Exact route matching, method mismatch, path parameters, and ambiguous definitions.
- Required and optional configuration values.
- Invalid URL, port, timeout, and size configuration.
- Header allowlist and hop-by-hop removal.
- Correlation ID validation and regeneration.
- Retry decision by method and idempotency contract.
- Public error mapping for known internal failures.
- Metric label cardinality policy.

## 5. Authentication and authorization tests

- Missing, malformed, expired, not-yet-valid, wrong-issuer, wrong-audience, wrong-algorithm, and invalid-signature tokens.
- Unknown signing key and key rotation behavior.
- Valid identity without required permission.
- Cross-user and cross-tenant resource request.
- Spoofed user, role, and tenant headers.
- Denial occurs before any outbound request.
- Identity provider/key source unavailable.

## 6. Contract tests

For every registered route validate method/path, authentication, permissions, destination service ID, upstream path, request schema, response schema, status mapping, header behavior, timeout, retry, idempotency, and quota semantics.

## 7. Integration tests

Use local mock upstream servers, never production endpoints. Cover normal response, slow response, connection reset, malformed body, oversized response, redirect, upstream 4xx/5xx, unavailable DNS, and client disconnect.

## 8. Mandatory security regression tests

- Unknown route and unregistered destination fail closed.
- Arbitrary scheme, hostname, port, and redirect cannot escape the allowlist.
- Metadata endpoints and unapproved private IPs are blocked.
- Oversized request/response is safely rejected.
- Quota exhaustion returns documented 429.
- Unsafe mutations are not retried automatically.
- Public errors contain no stack trace, internal host, or credential.
- Logs and traces contain no token or sensitive payload.

## 9. Test data and environment

Use deterministic synthetic identities and payloads. Keep signing keys and fixtures test-only. Do not commit real bearer tokens, customer records, production URLs, or private payloads. Use isolated service containers or local mock servers.

## 10. CI quality gates

- Package metadata parses.
- Source syntax checks pass when source exists.
- Markdown and internal links validate.
- Actual tests run when test files exist.
- Dependency advisories are reviewed.
- Secret scanning passes.
- Mermaid and image references are checked.
- Workflow permissions remain minimal.

A skipped test stage must be reported as skipped, not passed.

## 11. Release gate

All required tests pass; security-critical findings are remediated or explicitly accepted by an authorised owner; contract changes have service-owner approval; and documentation is updated with implementation evidence.
