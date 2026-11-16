# ⚙️ Configuration and secret management

## Current status

The root environment example contains proposed names. No environment loader or validation code exists yet.

## Principles

- Keep configuration external to source code.
- Validate mandatory settings once at startup.
- Fail startup when security-critical configuration is missing or invalid.
- Separate local, CI, staging and production environments.
- Use a hosting secret manager for credentials.
- Never print the full environment.
- Treat browser-exposed variables as public.
- Keep route destinations in reviewed configuration, not request data.

## Proposed variable inventory

| Variable | Purpose | Secret? | Status |
|---|---|---:|---|
| `PORT` | Gateway listener | No | Example only |
| `LOG_LEVEL` | Log verbosity | No | Example only |
| `UPSTREAM_*_URL` | Approved destinations | Internal detail | Not configured |
| `JWT_ISSUER` | Expected issuer | No | Not configured |
| `JWT_AUDIENCE` | Expected audience | No | Not configured |
| `JWT_JWKS_URI` | Trusted signing-key endpoint | Integrity-critical | Not configured |
| `RATE_LIMIT_STORE_URL` | Shared quota store | May contain credentials | Not configured |
| `*_TIMEOUT_MS` | Per-service deadline | No | Not configured |

Do not add variables without a runtime consumer, validation rule and docs entry.

## Secret examples

Secrets include JWT signing keys, client secrets, service credentials, shared-store passwords, and webhook secrets. Store them in a manager, use least privilege, and rotate immediately after exposure. Keep only placeholders in Git.

## Token verification requirements

Define trusted issuer, audience, algorithm allowlist, key source/rotation, expiry/not-before, clock skew, required claims, permission mapping, and revocation strategy. Never treat decoded but unverified JWT claims as identity.

## Upstream URL validation

Validate URL scheme, hostname, port, path, resolved IP range and redirects. Reject arbitrary destinations, userinfo credentials in URLs, and unapproved address ranges. Network policy should reinforce application allowlists.

## Local setup

A dotenv dependency is not installed. Copying `.env.example` to `.env` currently has no runtime effect. Add and test environment loading alongside server startup.
