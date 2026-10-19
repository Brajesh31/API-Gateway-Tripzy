# API Gateway Tripzy

This repository is currently a minimal Node.js package scaffold for a planned API gateway for Tripzy microservices. The manifest describes the intended gateway responsibility, but no `index.js`, server framework, routes, tests, or service configuration are committed yet.

## Current state

- Package name: `api-gateway`
- Entry-point convention: `index.js` (not currently present)
- Runtime dependencies: none
- Test script: placeholder that intentionally exits with an error

Do not treat this repository as a deployable gateway until the server, routing policy, service discovery, authentication, observability, and tests are implemented.

## Planned responsibilities

The gateway may eventually provide a single client-facing boundary for Tripzy services, including request routing, authentication, rate limiting, correlation IDs, timeouts, retries, circuit breaking, response normalization, and health reporting. These are design goals only and must be specified before implementation.

## Development

Install the package from the repository root:

```bash
npm install
```

There is no runnable development command yet. The current `npm test` script is a placeholder and fails by design. Add a server and tests before documenting startup or deployment commands.

## Security

Never commit service credentials, signing keys, internal hostnames, or production routes. A gateway must fail closed for unauthorized routes, avoid forwarding untrusted headers, enforce payload limits, and protect internal services from direct public access.

## License

Original code and documentation are released under the MIT License.

Last updated: 2026-10-19 00:41 UTC
