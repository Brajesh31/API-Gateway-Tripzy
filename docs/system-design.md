# 🧠 System design and architectural constraints

> Status: design document. No gateway runtime is implemented. Decisions marked as proposals require confirmation during implementation.

## 1. Design objective

Create a stable, secure HTTP boundary between external clients and independently owned Tripzy services. The gateway centralises cross-cutting transport concerns without becoming a second domain application.

The goal is not merely to reduce the number of client URLs. It is to make routing, identity, authorization, resource budgets, error contracts, and operational behavior explicit and reviewable.

## 2. Quality attributes

| Attribute | Design intent | Verification |
|---|---|---|
| Security | Default-deny, fixed destinations, verified identity | Security tests and threat review |
| Reliability | Bound dependency failures and isolate services | Fault injection and resilience tests |
| Performance | Predictable latency and bounded concurrency | Load test with p95/p99 reporting |
| Compatibility | Stable public contract independent of internal topology | Contract tests and version policy |
| Operability | Correlated, redacted telemetry | Trace/log correlation exercises |
| Maintainability | Small modules and explicit route definitions | Code review and architecture checks |
| Portability | Avoid unnecessary provider coupling | Reproducible build and environment contract |
| Privacy | Minimise data copied into logs and shared state | Data inventory and retention review |

## 3. Proposed logical components

1. **Edge boundary:** TLS termination, connection handling, and trusted proxy metadata.
2. **Gateway application:** HTTP lifecycle and middleware composition.
3. **Request context:** Correlation ID, timing, and verified caller context.
4. **Identity adapter:** Signature and claim verification against a trusted identity contract.
5. **Policy evaluator:** Route permission, quota, and concurrency decision.
6. **Route registry:** Fixed public route to approved service mapping.
7. **Upstream client:** Filtered forwarding with deadline and response limits.
8. **Error mapper:** Stable public error envelope and status translation.
9. **Telemetry adapter:** Redacted logs, metrics, and trace propagation.

No module above is present as source code yet. See [modules](./modules.md) for proposed boundaries.

## 4. Request processing order

The order is security-sensitive. A future implementation should not allow a handler to skip or reorder critical checks.

~~~mermaid
flowchart TD
  A[Receive request] --> B[Parse and enforce request limits]
  B --> C[Resolve exact method and path]
  C --> D{Known route?}
  D -->|No| X404[Return 404]
  D -->|Yes| E[Create request context]
  E --> F[Verify identity if required]
  F --> G{Verified?}
  G -->|No| X401[Return 401]
  G -->|Yes| H[Evaluate permission]
  H --> I{Allowed?}
  I -->|No| X403[Return 403]
  I -->|Yes| J[Apply quota and concurrency policy]
  J --> K{Allowed?}
  K -->|No| X429[Return 429]
  K -->|Yes| L[Resolve trusted service destination]
  L --> M[Forward with fixed deadline]
  M --> N[Filter response and map errors]
  N --> O[Emit redacted telemetry]
~~~

A public route may be anonymous only when its contract explicitly says so. Unknown routes must never fall through to a generic URL forwarder.

## 5. Statelessness and shared state

Default to stateless gateway replicas. A shared store may be introduced for distributed rate limits, idempotency records, short-lived coordination, or a narrowly scoped cache. Every such addition needs an ADR describing consistency, failure mode, retention, privacy, and operational ownership.

Do not use a gateway database as a mirror of all service databases. Domain services remain the source of truth for domain records.

## 6. Resilience model

- Each inbound request has an overall deadline.
- Each upstream call has a smaller service-specific deadline.
- Concurrency is bounded per service and globally.
- Retry policy is method- and contract-specific.
- Circuit breakers, if introduced, need observable open, half-open, and closed transitions.
- A single upstream outage must not make unrelated routes unavailable.
- The gateway should fail fast when a required security dependency is unavailable, unless a reviewed policy explicitly permits a safe alternative.

## 7. Failure domains

| Failure domain | Expected containment |
|---|---|
| One upstream service | Fail only routes dependent on that service |
| Identity provider/key source | Fail protected requests closed; report dependency health |
| Quota store | Apply an explicit fail-open/fail-closed policy per risk class |
| Telemetry vendor | Do not block request processing on non-critical export |
| Gateway instance | Load balancer routes to healthy replicas |
| Configuration error | Prevent unsafe startup rather than guessing defaults |

## 8. Decisions requiring confirmation

- HTTP framework and Node.js support policy.
- Identity provider, token format, issuer, audience, key rotation, and revocation.
- Real upstream services, owners, paths, and network allowlists.
- Whether a distributed quota store is needed.
- Public API versioning and compatibility commitments.
- Hosting platform, TLS termination, and trusted proxy chain.
- Logging and tracing vendors, retention, and access controls.

Record decisions in [ADRs](./ADRs/README.md). Do not treat examples in this document as approved production settings.
