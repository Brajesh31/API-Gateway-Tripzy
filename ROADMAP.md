# 🗺️ Roadmap

These milestones are proposals, not completed functionality.

## Phase 0: Contract discovery
- [ ] Confirm actual Tripzy services, owners, and environments.
- [ ] Obtain OpenAPI or equivalent contracts.
- [ ] Define API versioning and identity model.
- [ ] Approve upstream network and security boundaries.

## Phase 1: Minimal runtime
- [ ] Select supported Node.js LTS and HTTP framework.
- [ ] Add app/server composition and validated configuration.
- [ ] Add liveness/readiness endpoints.
- [ ] Add graceful shutdown and safe structured logging.
- [ ] Add a mock-upstream integration test.

## Phase 2: Secure routing
- [ ] Add explicit allowlisted route registry.
- [ ] Add identity verification and route authorization.
- [ ] Add header allowlist and SSRF protections.
- [ ] Add request/body/response limits and rate limiting.
- [ ] Add per-service timeout and retry budgets.

## Phase 3: Reliability
- [ ] Add contract tests for every upstream route.
- [ ] Add concurrency isolation and circuit behavior where justified.
- [ ] Add metrics, tracing, alerts and dashboards.
- [ ] Define SLOs with service owners.

## Phase 4: Production readiness
- [ ] Add reproducible deployment and rollback.
- [ ] Complete security assessment.
- [ ] Complete load and resilience testing.
- [ ] Publish compatibility and deprecation policy.
