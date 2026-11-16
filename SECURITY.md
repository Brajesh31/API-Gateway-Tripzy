# 🔐 Security Policy

## 1. Current security status

This repository is not an operational gateway. It currently has no server, authentication, authorization, proxy, upstream routing, or runtime telemetry implementation. Do not expose it as a public API gateway.

The security controls described in the documentation are design requirements, not verified defenses. See [Security engineering](./docs/security.md) and [Threat model](./docs/threat-model.md).

## 2. Reporting a vulnerability

Report security concerns privately through [GitHub Security Advisories](https://github.com/Brajesh31/API-Gateway-Tripzy/security/advisories/new).

Do not publish exploit details, tokens, service credentials, internal addresses, customer data, or payload samples in public issues, discussions, or pull requests.

A useful report includes:
- Affected commit or deployment, if one exists.
- Impact and affected trust boundary.
- Preconditions and a sanitized reproduction.
- Expected versus observed behavior.
- Potential mitigation.
- Whether the issue may expose credentials or personal data.

## 3. Response expectations

The maintainer will review the report, confirm scope, coordinate with affected service owners where necessary, and plan a fix or mitigation. Response timing depends on severity and available evidence. Do not assume a fixed SLA unless one is published separately.

## 4. Required future controls

- Cryptographic token verification with issuer, audience, expiry, and algorithm policy.
- Route-level authorization and service-level resource checks.
- Default-deny route and upstream destination policy.
- SSRF, DNS rebinding, and arbitrary URL forwarding protections.
- Request, response, header, concurrency, and timeout limits.
- Rate limiting and safe retry/idempotency policy.
- Header filtering and sensitive-data redaction.
- Least-privilege credentials and network access.
- Security regression tests and incident runbooks.

## 5. Credential exposure

Revoke or rotate a leaked credential at its provider immediately. Deleting a value from the latest source does not invalidate it or remove it from Git history. Review access logs and affected service activity. If a secret entered Git history, assess history cleanup separately after rotation.

## 6. Dependency and workflow security

Review new dependencies, lockfiles, GitHub Actions permissions, third-party action versions, and workflow changes. Never expose production secrets to untrusted pull-request code. Dependency audit findings must be investigated, not hidden by lowering thresholds.

## 7. Supported versions

Only the default branch is maintained at this stage. Formal runtime support windows will be established after a gateway release exists.
