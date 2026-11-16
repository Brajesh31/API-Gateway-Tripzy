# 🗺️ API Gateway Tripzy roadmap

> All milestones are proposed. No runtime capability is marked complete merely because a design document exists.

## Phase 0: Contract discovery and product decisions

- [ ] Confirm actual Tripzy services, technical owners, and environments.
- [ ] Obtain OpenAPI or equivalent service contracts.
- [ ] Define public API consumers and versioning expectations.
- [ ] Confirm identity provider, token format, issuer, audience, and claims.
- [ ] Agree route-level permissions and service-level authorization.
- [ ] Approve upstream network and security boundaries.
- [ ] Decide hosting and deployment ownership.

**Exit criteria:** service ownership and API/identity contracts are documented and approved.

## Phase 1: Minimal runtime foundation

- [ ] Select supported Node.js LTS and HTTP framework.
- [ ] Add application composition and server lifecycle.
- [ ] Add immutable, validated environment configuration.
- [ ] Add liveness and readiness endpoints.
- [ ] Add graceful shutdown and structured error handling.
- [ ] Add request correlation.
- [ ] Add local mock-upstream integration tests.

**Exit criteria:** server starts safely, invalid configuration fails closed, and basic lifecycle tests pass.

## Phase 2: Secure routing

- [ ] Add explicit allowlisted route registry.
- [ ] Add identity verification through the confirmed provider.
- [ ] Add deny-by-default route authorization.
- [ ] Add header allowlist and trusted proxy handling.
- [ ] Add SSRF and redirect protections.
- [ ] Add request/response limits and rate limiting.
- [ ] Add per-service timeout and retry budgets.

**Exit criteria:** every exposed route has an approved owner, permission, destination, schema, quota, timeout, and security tests.

## Phase 3: Reliability and observability

- [ ] Add contract tests for every upstream route.
- [ ] Add concurrency isolation and circuit behavior where justified.
- [ ] Add metrics, tracing, redacted logs, alerts, and dashboards.
- [ ] Define SLOs and escalation paths with service owners.
- [ ] Add resilience and controlled load tests.
- [ ] Complete incident and recovery exercises.

**Exit criteria:** failures are isolated, requests are diagnosable, and operational owners can respond.

## Phase 4: Production readiness

- [ ] Add reproducible deployment and rollback.
- [ ] Complete security assessment.
- [ ] Complete capacity and resilience testing.
- [ ] Verify secret rotation and recovery procedures.
- [ ] Publish compatibility and deprecation policy.
- [ ] Establish on-call ownership and release process.

**Exit criteria:** production traffic can be deployed, monitored, and rolled back using tested procedures.

## Future opportunities

Consider partner API key management, caching, idempotency storage, service discovery, GraphQL aggregation, or gRPC translation only when actual requirements justify their added complexity. See [Future scope](./docs/future-scope.md).
