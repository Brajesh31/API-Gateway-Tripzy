# 🛡️ Security engineering guide

> **Current posture:** No gateway server, proxy route, authentication middleware, or authorization implementation exists. This document is a required design baseline, not evidence of implemented protection.

## 1. Security objectives

The gateway will sit between external callers and internal services. It should reduce exposure by providing a narrow public API, enforcing verified identity and route permissions, restricting outbound destinations, bounding resource use, and protecting operational data.

The gateway must not become an open proxy or a single privileged component that trusts caller-controlled identity or destination data.

## 2. Authentication

Authentication verifies the caller's identity. Before implementation, confirm the identity provider and obtain its formal token contract.

Required controls:
- Verify cryptographic signatures using trusted keys.
- Pin an explicit algorithm allowlist.
- Validate issuer and audience.
- Validate expiration and not-before claims.
- Apply a documented clock-skew allowance.
- Validate required subject and tenant/client claims.
- Define key rotation and cache refresh behavior.
- Define revocation and token lifetime strategy.
- Separate user identity from gateway-to-service identity.
- Fail closed for protected routes when verification cannot be trusted.

Never treat a decoded JWT as verified identity. Never log raw tokens.

## 3. Authorization

- Default-deny all protected routes.
- Declare required permission for every route.
- Derive identity only from verified claims.
- Strip client-supplied user, role, tenant, and permission headers.
- Enforce route permission before outbound requests.
- Keep object/resource-level checks in the domain service.
- Test horizontal and vertical privilege escalation.
- Define behavior for missing, unknown, or stale permission claims.

## 4. SSRF and open-proxy prevention

The upstream destination must be selected from a trusted registry. Do not accept arbitrary hostnames, ports, schemes, URLs, or redirect targets from request path, query, body, or headers.

Required defenses:
- Use stable service identifiers rather than client-supplied URLs.
- Restrict schemes and ports.
- Validate hostname and resolved address ranges.
- Block cloud metadata endpoints and unapproved private ranges.
- Protect against DNS rebinding.
- Disable redirects by default or revalidate every redirect target.
- Apply network egress allowlists in addition to application checks.
- Reject URL userinfo credentials.
- Never expose a generic fetch or proxy endpoint.

## 5. Header security

Remove hop-by-hop headers and untrusted forwarding metadata. Forward only headers required by the service contract. Recreate trusted identity context only after verification. Avoid forwarding cookies unless a reviewed contract requires them. Bound header count and size. Remove internal diagnostics from public responses.

## 6. Request and response constraints

Set limits for:
- Header count and aggregate size.
- URL/path/query length.
- Request body size and content type.
- Response body size.
- Connection duration and idle timeout.
- Overall request deadline.
- Per-upstream timeout.
- Global and per-service concurrency.
- Decompression ratio and streaming behavior.

Reject oversized or malformed inputs before expensive processing.

## 7. Rate limiting and abuse controls

Define quota identity, policy scope, window/algorithm, burst behavior, retry response, and store failure policy. Avoid using raw tokens as keys. Use trusted subject/client/tenant dimensions only after identity verification. Apply unauthenticated limits using a carefully defined network identity and trusted proxy chain.

## 8. Retry and mutation safety

Retries are a security and correctness concern, not merely a performance feature. Never automatically retry non-idempotent operations unless the owning service contract provides an idempotency mechanism. Bound attempts and total retry time. Do not amplify traffic during an outage.

## 9. Logging and error disclosure

Never log tokens, cookies, passwords, signing keys, or full private payloads. Return stable public error codes and correlation IDs. Keep stack traces and detailed dependency diagnostics in restricted telemetry. Sanitize user-controlled log fields to prevent injection.

## 10. Supply-chain security

- Commit and review lockfiles when dependencies are introduced.
- Audit dependency advisories.
- Remove unused dependencies.
- Use minimal GitHub Actions permissions.
- Pin third-party workflow actions to full commit SHAs as a hardening measure.
- Avoid exposing production secrets to untrusted pull-request code.
- Review workflow changes as privileged code.
- Scan repository history for accidental secrets.

## 11. Threat-driven test requirements

Test invalid/expired tokens, wrong issuer/audience/algorithm, missing permissions, spoofed headers, unknown routes, arbitrary destination attempts, DNS rebinding, redirect escape, oversized bodies, slow upstreams, retry storms, and telemetry leakage.

## 12. Production security gate

Do not send public traffic until authentication, authorization, destination allowlisting, SSRF protection, header filtering, request limits, rate limiting, deadlines, secret management, security tests, and incident response have been implemented and reviewed.
