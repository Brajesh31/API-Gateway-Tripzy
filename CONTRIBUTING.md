# 🤝 Contributing

API Gateway Tripzy is currently a scaffold. Contributions must not imply that a route, service, identity provider, or runtime feature exists before it is implemented and tested.

## Before coding

1. Read the README and relevant engineering guide.
2. Confirm the affected Tripzy service contract and owner.
3. Define public route semantics, authentication and permission requirements.
4. Identify security, timeout and failure cases.
5. Open an issue for substantial changes.

## Local workflow

```bash
git clone https://github.com/Brajesh31/API-Gateway-Tripzy.git
cd API-Gateway-Tripzy
npm install
```

There is no runnable server yet. The current test command is a placeholder. Add actual tests and a real test script with the first implementation.

## Implementation principles

- Use explicit trusted service IDs and destinations.
- Fail closed on missing auth configuration or unknown routes.
- Keep domain logic and databases in owning services.
- Filter request/response headers.
- Set request, response and upstream size/time limits.
- Retry only when idempotency and retry budget are explicit.
- Keep logs structured and redacted.
- Avoid unnecessary dependencies.
- Use supported Node.js LTS versions.

## Tests expected

Add unit tests for pure logic, contract tests for each service route, integration tests with mock upstream servers, and security regression tests for authorization bypass, SSRF, header spoofing, malformed payloads, and timeout behavior.

## Documentation expected

Update API, service register, architecture, configuration, threat model, testing and operations docs when corresponding behavior changes.

## Pull requests

Use the template. Include actual commands/results, service owner approval, security impact, deployment notes, and sanitized evidence. Never attach secrets or private customer data.
