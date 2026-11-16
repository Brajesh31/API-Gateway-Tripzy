# 📖 Gateway and distributed-systems glossary

| Term | Meaning in this project |
|---|---|
| API Gateway | Controlled entry point that routes client requests to approved services |
| Upstream | Backend service called by the gateway |
| Downstream | A dependency or service further along the request path |
| Route registry | Reviewed mapping from public method/path to service ID and policy |
| Route ID | Stable internal identifier for a public route |
| Authentication | Verifying who or what is making a request |
| Authorization | Deciding whether an authenticated identity may perform an action |
| JWT | JSON Web Token, a claims format that must be cryptographically verified before trust |
| JWKS | JSON Web Key Set used to publish public verification keys |
| Issuer | Identity provider expected to issue a credential |
| Audience | Intended recipient of a credential |
| Scope | Permission claim describing allowed operations |
| RBAC | Role-based access control |
| ABAC | Attribute-based access control |
| SSRF | Server-side request forgery, where an attacker induces a server to contact an unintended destination |
| Open proxy | Proxy that forwards requests to arbitrary destinations |
| Correlation ID | Identifier used to connect logs/events for one request |
| Trace | Distributed record of work across services |
| Span | One timed operation within a trace |
| SLO | Service-level objective, a target for a service indicator |
| SLI | Service-level indicator, a measured reliability or performance signal |
| p95 / p99 | 95th/99th percentile latency |
| Rate limit | Policy restricting request frequency over a defined interval |
| Concurrency limit | Maximum number of simultaneous operations |
| Backpressure | Mechanism that prevents overload from growing without bound |
| Retry budget | Bound on retry attempts or retry time |
| Idempotency | Property that repeating an operation has the same intended effect as performing it once |
| Circuit breaker | Failure-control pattern that temporarily stops calls to an unhealthy dependency |
| Bulkhead | Isolation boundary that prevents one dependency from consuming all resources |
| Liveness | Probe indicating whether a process is alive and progressing |
| Readiness | Probe indicating whether an instance can accept traffic |
| Contract test | Test verifying compatibility with a documented service interface |
| ADR | Architecture Decision Record documenting a significant design choice |
| Canary release | Gradual release to a small portion of traffic before full rollout |
| Fail closed | Deny access when a required security decision cannot be made |
| Fail open | Continue operation when a dependency is unavailable, only where explicitly judged safe |
