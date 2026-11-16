# 🛡️ Security engineering guide

## Security posture

No gateway server is currently implemented, so no runtime authentication, authorization, rate limiting, proxy protections, or TLS behavior can be claimed. This document defines the minimum design requirements.

## 1. Authentication

- Verify tokens cryptographically using a trusted issuer and key source.
- Enforce issuer, audience, expiry, not-before, and allowed algorithms.
- Reject missing, malformed, expired, or unverifiable credentials.
- Never use decoded but unverified token claims for authorization.
- Keep service credentials server-side.
- Define service-to-service identity separately from client identity.

## 2. Authorization

- Default to deny.
- Map every protected route to an explicit permission.
- Enforce resource-level authorization in the owning service where the gateway lacks authoritative domain context.
- Do not trust role or user headers supplied by clients.
- Verify authorization occurs before any upstream call.
- Test privilege escalation and cross-account access cases.

## 3. SSRF and open-proxy prevention

- Only route to registered service IDs.
- Never accept a URL or hostname from a request parameter.
- Restrict outbound destinations through network policy.
- Validate URL scheme, hostname, port, resolved IP ranges, and redirects.
- Block metadata endpoints and unapproved private networks.
- Do not create a generic URL-forwarding endpoint.

## 4. Header handling

- Strip hop-by-hop headers.
- Filter forwarded headers against an allowlist.
- Replace, do not trust, gateway identity and correlation headers.
- Avoid forwarding cookies unless explicitly required.
- Remove internal service diagnostic headers before returning responses.
- Prevent request/response header size abuse.

## 5. Request controls

- Limit body size, header size, URL length, and request duration.
- Validate content type and accepted encodings.
- Rate-limit by meaningful identity dimensions.
- Apply concurrency limits per upstream.
- Reject unsupported methods and ambiguous route matching.
- Set conservative decompression limits if compression is enabled.

## 6. Resilience controls

- Use per-service timeout budgets.
- Avoid unbounded retries and retry storms.
- Retry only safe/idempotent operations under explicit policy.
- Use circuit breakers only with observable state transitions.
- Bound response size and streaming behavior.
- Ensure a downstream outage cannot exhaust all gateway resources.

## 7. Logging and error handling

- Never log passwords, tokens, cookies, authorization headers, or private payloads.
- Use stable public error codes.
- Include a correlation ID in client-safe error responses.
- Store detailed diagnostics only in restricted logs.
- Avoid exposing stack traces, upstream hostnames, IP addresses, or internal route names.

## 8. Supply chain

- Commit npm lockfiles when dependencies are introduced.
- Review dependency advisories before release.
- Keep GitHub Actions permissions minimal.
- Pin third-party actions to full commit SHAs for higher supply-chain assurance.
- Avoid running untrusted pull request code with production secrets.
- Run secret scanning and dependency auditing.

## Security release gate

A production gateway is not ready until authentication, authorization, upstream allowlisting, request limits, header filtering, timeouts, rate limits, security tests, and operational logging are implemented and reviewed.
