# 🧑‍💻 Development guide

## Work from verified contracts

The current repository has no runtime implementation. Collect real Tripzy service contracts before choosing endpoints, auth claims, error mapping, or destination names.

## Recommended sequence

1. Confirm public API requirements and service ownership.
2. Define route contract records.
3. Choose Node version and HTTP framework.
4. Implement app/server composition and health endpoints.
5. Implement validated configuration.
6. Implement one approved route.
7. Add authentication, authorization and outbound allowlist.
8. Add timeout, size limits and error mapping.
9. Add unit/contract/integration/security tests.
10. Add telemetry and deployment configuration.

## Branch conventions

Examples: `feat/route-registry`, `feat/request-correlation`, `fix/header-filtering`, `docs/service-contracts`, `test/ssrf-regressions`.

## Coding rules

- Keep modules focused.
- Validate config once at startup.
- Avoid mutable global config.
- Keep upstream destinations in trusted configuration.
- Use outbound HTTP clients with explicit timeout and redirect policies.
- Do not retry unsafe methods automatically.
- Centralize public error mapping.
- Redact sensitive log fields.
- Avoid dependencies without a concrete need.
- Use supported Node.js LTS versions.

## Current commands

Only the placeholder `npm test` exists. No start/dev script is available. Add real commands when server and tests are committed and update docs in the same change.

## Pull requests

Explain route/service impact, auth/permission behavior, failure/timeout behavior, threat review, tests, config, rollout and rollback. Update API, service register, threat model and operations docs when relevant.
