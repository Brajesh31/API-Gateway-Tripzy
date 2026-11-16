# ⚙️ Configuration and secret management

## 1. Current status

The root .env.example contains proposed variable names only. No environment loader, configuration module, or startup validation exists. Copying the example file into a local .env file currently has no runtime effect.

## 2. Configuration principles

- Configuration is external to source code.
- Security-critical settings are mandatory and validated at startup.
- Invalid or ambiguous values prevent startup.
- Local, CI, staging, and production environments are separate.
- Secrets are injected from a secret manager.
- The application never logs the complete environment.
- Browser-exposed variables are treated as public.
- Upstream destinations are selected from trusted configuration, never request data.

## 3. Variable catalogue

| Variable | Intended purpose | Example | Secret? | State |
|---|---|---|---:|---|
| PORT | Listener port | 3000 | No | Example only |
| LOG_LEVEL | Structured log threshold | info | No | Example only |
| REQUEST_TIMEOUT_MS | Overall request budget | 10000 | No | Example only |
| MAX_REQUEST_BODY_BYTES | Maximum inbound body size | 1048576 | No | Example only |
| JWT_ISSUER | Trusted token issuer | https://identity.example.invalid/ | No | Not configured |
| JWT_AUDIENCE | Expected token audience | tripzy-api | No | Not configured |
| JWT_JWKS_URI | Trusted key-set location | Reserved example URL | Integrity-critical | Not configured |
| UPSTREAM_*_URL | Fixed service destination | Reserved example URL | Internal | Not configured |
| RATE_LIMIT_STORE_URL | Shared quota store | Managed endpoint | May include credentials | Not configured |

Only add a variable when a runtime module consumes it and a validation rule is documented.

## 4. Validation requirements

- PORT must be an integer in the allowed range.
- Timeout values must be positive, bounded integers.
- Body-size limits must be positive and compatible with the edge proxy.
- URLs must use approved schemes and hostnames.
- Identity issuer and audience must be exact expected values.
- Key sources must be trusted and use TLS.
- Unknown or duplicate route destination identifiers should fail startup.
- Secrets must not be empty or fall back to insecure defaults in production.

## 5. Secret categories

Examples include signing keys, client secrets, service credentials, database credentials for any approved shared store, and webhook secrets. Store them in a managed secret service, grant least privilege, and rotate immediately after exposure.

Never commit a production .env file. The repository ignore rules should exclude local environment files while retaining .env.example.

## 6. JWT and identity settings

Before implementing authentication, obtain the identity provider's issuer, audience, algorithms, signing-key rotation model, required claims, permission mapping, expiry behavior, and revocation expectations. Do not invent these values.

## 7. Upstream destination settings

The service registry should use stable service IDs. Configuration validation should check scheme, hostname, port, allowed address ranges, DNS resolution, and redirect policy. Network egress controls should reinforce application-level allowlists.

## 8. Environment matrix

| Setting | Local | CI | Staging | Production |
|---|---|---|---|---|
| Identity provider | Mock/test issuer | Deterministic test keys | Isolated issuer | Approved production issuer |
| Upstreams | Local mock servers | Local mocks | Staging services | Approved internal services |
| Secrets | Local-only ignored file | Ephemeral test values | Secret manager | Secret manager |
| Telemetry | Console/test exporter | No production export | Staging backend | Restricted production backend |

## 9. Configuration change procedure

1. Document purpose, owner, type, and default.
2. Add parsing and validation tests.
3. Update .env.example with a non-secret placeholder.
4. Update deployment configuration and secret manager.
5. Review whether a restart or rolling reload is required.
6. Update runbooks and rollback instructions.

## 10. Security checklist

- [ ] No real secrets in Git.
- [ ] Startup fails on invalid required configuration.
- [ ] Environment output is not logged.
- [ ] Destination allowlists are enforced.
- [ ] Secret rotation is documented.
- [ ] CI uses no production credentials.
- [ ] Configuration changes are reviewed.
