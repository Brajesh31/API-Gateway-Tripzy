# 🧨 Threat model and abuse-case analysis

> This is a design-level threat model, not a penetration test. Runtime code is absent, so every mitigation below remains a requirement until implemented and tested.

## 1. Scope and assumptions

The proposed system accepts HTTP requests from external clients, verifies identity, evaluates route policy, and forwards approved requests to internal services. Actual Tripzy service topology, identity provider, network configuration, and deployment platform are not yet confirmed.

## 2. Assets

- User/client identity and permission claims.
- Service-to-service credentials.
- Trusted upstream destination registry.
- Upstream service availability and data integrity.
- Public API contracts and compatibility guarantees.
- Logs, metrics, traces, and security audit events.
- CI/CD credentials and release artifacts.
- Configuration and signing-key metadata.

## 3. Trust boundaries

~~~mermaid
flowchart LR
  Client[Untrusted client] --> Edge[Public edge]
  Edge --> Gateway[Gateway boundary]
  Gateway --> Identity[Identity provider]
  Gateway --> Registry[Trusted route registry]
  Registry --> Services[Private upstream services]
  Gateway --> Telemetry[Restricted observability]
  CI[CI identity] --> Artifact[Build artifact]
  Artifact --> Runtime[Deployment environment]
  Secrets[Secret manager] -. controlled access .-> Gateway
~~~

## 4. Threat register

| Threat | Attack path | Impact | Required mitigation |
|---|---|---|---|
| Broken authentication | Forged, expired, or wrong-audience token accepted | Impersonation | Signature and claim verification |
| Broken authorization | Caller accesses another user's resource | Data exposure | Route permissions plus service object checks |
| SSRF | Caller controls upstream host or redirect | Internal network access | Fixed registry, DNS/redirect checks, egress policy |
| Open proxy | Arbitrary URL forwarding endpoint | Abuse and internal scanning | No client-selected destinations |
| Header spoofing | Fake user/role/tenant header | Privilege escalation | Strip and recreate trusted headers |
| Request smuggling | Edge and app parse request differently | Policy bypass | Consistent parser/proxy behavior and regression tests |
| Denial of service | Large bodies, slow clients, unbounded concurrency | Availability loss | Size/time/concurrency limits and quotas |
| Retry storm | Outage causes repeated upstream calls | Cascading failure | Retry budget and idempotency checks |
| Information disclosure | Raw upstream exception returned | Internal detail exposure | Stable public errors and redaction |
| Dependency compromise | Vulnerable or malicious package/action | Runtime or CI compromise | Lockfile, audit, review, pinned actions |
| Misconfiguration | Route points to unintended service | Data leakage | Startup validation and route approval |
| Telemetry leakage | Logs include token or private payload | Credential/privacy exposure | Redaction, access control and retention |

## 5. Abuse-case walkthroughs

### A. Destination injection

An attacker submits a URL in a query parameter or body and attempts to make the gateway contact a metadata endpoint. The request must never influence the destination selection. The gateway resolves a service ID from the fixed route registry and applies network egress restrictions.

### B. Identity header spoofing

An attacker sends X-User-Id or X-Role with an elevated value. The gateway strips these headers and creates a trusted context only from verified credentials. The upstream must authenticate the gateway and apply its own resource authorization.

### C. Retry after uncertain mutation

A mutation reaches the service, commits, and then loses its response. The gateway must not assume the operation failed. A retry is allowed only when the route contract provides idempotency semantics.

### D. Telemetry secret leakage

An exception contains an Authorization header or query value. The logging layer must redact sensitive fields before serialization. Tests should inspect emitted logs, not only HTTP responses.

## 6. Security test catalogue

- Missing, malformed, expired, wrong-issuer, wrong-audience, wrong-algorithm, and invalid-signature tokens.
- Valid identity with insufficient permission.
- Cross-user and cross-tenant resource access.
- Spoofed identity headers.
- Unknown route and unregistered service ID.
- URL schemes, ports, private addresses, metadata endpoints, DNS rebinding, and redirects.
- Chunked, compressed, malformed, and oversized request bodies.
- Oversized upstream response and slow streaming.
- Mutation timeout with uncertain upstream outcome.
- Log and trace redaction.
- Dependency/workflow permission regression.

## 7. Risk priority

| Priority | Examples | Release impact |
|---|---|---|
| P0 | Authentication, authorization, SSRF, destination allowlist, secret exposure, request limits | Blocks internet exposure |
| P1 | Contract tests, telemetry redaction, concurrency isolation, incident runbooks | Blocks general availability |
| P2 | Independent assessment, chaos testing, formal SLO/error budget | Maturity work |

## 8. Residual risk and ownership

Each accepted risk must have a named owner, rationale, compensating control, and review date. Do not mark a mitigation complete until implementation evidence and regression tests exist. Internal services must not assume that gateway route authorization replaces their own resource-level access checks.
