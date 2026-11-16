# 🛡️ Security engineering guide

## Current security posture

No gateway server or runtime control is implemented. The following requirements are mandatory design controls, not existing defenses.

## Authentication

- Verify credentials cryptographically using a trusted issuer and keys.
- Enforce issuer, audience, expiry, not-before and allowed algorithms.
- Reject unverifiable credentials.
- Never use decoded but unverified claims.
- Keep service credentials server-side.
- Define client identity separately from gateway-to-service identity.

## Authorization

- Default-deny.
- Associate every protected route with an explicit permission.
- Enforce resource-level access in the service owning the data.
- Do not trust client-supplied user/role headers.
- Ensure denials happen before outbound requests.
- Test privilege escalation and cross-account access.

## SSRF and open-proxy prevention

- Route only to registered service identifiers.
- Never accept arbitrary URL/hostname from requests.
- Restrict outbound destinations with network policy.
- Validate schemes, hosts, ports, DNS resolution and redirect targets.
- Block metadata endpoints and unapproved private ranges.
- Never expose a generic fetch/forward URL endpoint.

## Header handling

- Strip hop-by-hop headers.
- Forward only explicit allowlisted headers.
- Remove untrusted identity headers and recreate only from verified claims.
- Avoid forwarding cookies unless required and reviewed.
- Remove internal service diagnostics from public responses.
- Bound header sizes.

## Request and resilience controls

- Limit request/response body, header, URL, concurrency and duration.
- Validate content type and encodings.
- Rate-limit by approved identity dimensions.
- Use per-service deadlines.
- Retry only safe/idempotent operations within a bounded budget.
- Bound decompression and streaming behavior.
- Avoid a single slow service exhausting all gateway resources.

## Logging and errors

Never log tokens, cookies, passwords, private keys, or full private payloads. Return stable public error codes and correlation IDs. Keep detailed diagnostics in restricted logs. Never expose stack traces or internal hosts.

## Supply chain

Use lockfiles, review dependency advisories, use minimal GitHub Actions permissions, pin third-party actions to full commit SHAs as a hardening step, and never expose production secrets to untrusted pull-request code.

## Production gate

Do not send public traffic until authentication, authorization, upstream allowlisting, SSRF protections, request limits, header filtering, timeouts, rate limiting, security tests and operational logging have been implemented and reviewed.
