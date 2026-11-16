# 🔭 Future scope and engineering opportunities

## Status

This roadmap describes potential work, not committed functionality. Each item depends on actual Tripzy service contracts, security review, operational ownership, and measured need.

## 1. Foundation phase

- Implement a minimal Node.js HTTP server using a framework selected by maintainers.
- Add immutable, validated environment configuration.
- Add liveness and readiness endpoints.
- Add graceful shutdown and structured error handling.
- Establish a local mock-upstream test harness.

**Exit criteria:** server starts and stops deterministically, invalid configuration fails closed, and health behavior is tested.

## 2. Secure routing phase

- Build an explicit route registry.
- Add route-level authentication metadata.
- Integrate a confirmed identity provider.
- Implement route authorization and header filtering.
- Add destination allowlists and outbound network restrictions.
- Enforce request, response, and connection limits.

**Exit criteria:** every exposed route has an owner, permission, destination, schema, timeout, quota, and security tests.

## 3. Reliability phase

- Add distributed rate limiting only if multiple instances require shared quota state.
- Add per-service connection pools and concurrency limits.
- Add safe retry policy and idempotency integration where service owners support it.
- Add circuit breakers if failure isolation benefits justify their complexity.
- Add request cancellation and deadline propagation.

**Exit criteria:** fault-injection tests show that one dependency failure does not exhaust unrelated routes.

## 4. Observability phase

- Add structured redacted logging.
- Add metrics with bounded-cardinality labels.
- Add trace propagation and service spans.
- Define service-level indicators with owners.
- Add alerts and dashboards.

**Exit criteria:** an operator can follow a request across the gateway and an upstream without accessing private payloads or credentials.

## 5. Production readiness phase

- Add container/runtime deployment configuration.
- Add staging environment and smoke tests.
- Add canary rollout and rollback.
- Perform capacity and security assessments.
- Establish on-call ownership and incident review.

**Exit criteria:** release can be deployed, observed, and rolled back using documented procedures.

## 6. Optional advanced capabilities

| Capability | Add only when | Main trade-off |
|---|---|---|
| API key management | Partner integration requires it | Key lifecycle and revocation |
| Request transformation | Contract compatibility requires it | More coupling and schema complexity |
| Response caching | Data is safe and cache semantics are clear | Staleness and identity partitioning |
| Idempotency store | Mutations require safe replay | Shared state and consistency |
| GraphQL aggregation | Product requires cross-service query composition | Query cost and authorization complexity |
| gRPC gateway | A real gRPC service contract exists | Protocol translation and observability |
| Service discovery | Dynamic service topology requires it | Registry availability and trust |
| Canary routing | Release risk warrants traffic shaping | Routing state and rollout complexity |

## 7. Prioritisation rules

Prioritise security and correctness before convenience features. Do not introduce caching, service discovery, distributed state, or complex circuit behavior before the basic routing and identity contracts are stable.
