# 🔐 Security Policy

## Current security status

This repository is not yet an operational gateway. It has no server, authentication, authorization, proxy, or upstream routing implementation. Do not expose it as a public API gateway.

## Reporting a vulnerability

Report security concerns privately through [GitHub Security Advisories](https://github.com/Brajesh31/API-Gateway-Tripzy/security/advisories/new). Do not publish exploit details, tokens, service credentials, internal addresses, or customer data in public issues.

Include affected commit/deployment, impact, trust boundary, sanitized reproduction, preconditions and suggested mitigation.

## Required future controls

- Cryptographic token verification.
- Route and resource-level authorization.
- Default-deny route and upstream destination policy.
- SSRF and arbitrary URL forwarding protections.
- Request/response/header limits and timeouts.
- Rate limiting and concurrency controls.
- Header filtering and sensitive-data redaction.
- Safe retry/idempotency policy.
- Least-privilege credentials and network access.
- Security regression tests.

## Credential exposure

Revoke or rotate a leaked credential at its provider immediately. Deleting it from the latest source does not invalidate it or remove it from Git history. Review access logs and affected service activity.

## Supported versions

Only the default branch is maintained at this stage. Formal support windows will be established after a runtime release.
