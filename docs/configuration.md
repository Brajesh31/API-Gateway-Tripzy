# ⚙️ Configuration and secret management

## Current status

The repository has a small `.env.example`, but no application code loads or validates these variables yet. The names below are proposed configuration contracts, not runtime-verified settings.

## Configuration principles

- Keep application configuration external to source code.
- Validate required values at startup.
- Fail startup when mandatory secrets or upstream definitions are missing.
- Separate development, staging, and production values.
- Keep secrets in the hosting platform's secret manager.
- Never print the complete environment to logs.
- Treat frontend-exposed variables as public.
- Prefer immutable, reviewed route configuration over user-controlled destinations.

## Proposed configuration inventory

| Variable | Purpose | Secret? | Status |
|---|---|---:|---|
| `PORT` | HTTP listener port | No | Example only |
| `LOG_LEVEL` | Logging verbosity | No | Example only |
| `UPSTREAM_*_URL` | Approved upstream base URL | Usually no, still internal | Not configured |
| `JWT_ISSUER` | Expected token issuer | No | Not configured |
| `JWT_AUDIENCE` | Expected token audience | No | Not configured |
| `JWT_JWKS_URI` | Trusted key endpoint | No, but integrity-critical | Not configured |
| `RATE_LIMIT_STORE_URL` | Distributed quota storage | May contain credentials | Not configured |
| `*_TIMEOUT_MS` | Service deadline | No | Not configured |

Do not add a variable until a runtime consumer, validation rule, and documentation entry exist.

## Secret handling

Examples of secrets:
- JWT signing private keys.
- Client secrets.
- Service-to-service credentials.
- Redis/database passwords when a shared store is adopted.
- Webhook signing secrets.

Use a managed secret store and least-privilege credentials. Rotate secrets on a documented schedule and immediately after exposure. Keep only placeholders in `.env.example`.

## Token verification requirements

Before accepting JWTs, define:
- Issuer and audience.
- Allowed algorithm list.
- Trusted key source and key rotation behavior.
- Token expiration and not-before policy.
- Clock-skew tolerance.
- Required claims and permission mapping.
- Revocation or short-lived token strategy.

Never trust the `alg` header to choose an arbitrary verification algorithm. Do not decode a JWT and treat unverified claims as identity.

## Upstream URL validation

The service registry must validate destination scheme, hostname, port, and route path. Reject arbitrary client-provided URLs, userinfo credentials in URLs, unexpected redirects, and disallowed address ranges. Keep upstream DNS/network policy under deployment control.

## Safe local setup

A future application may use a dotenv loader, but that dependency is not currently installed. Do not assume `npm start` loads `.env`. Add and test environment loading as part of the server implementation.
