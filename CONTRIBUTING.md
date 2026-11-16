# 🤝 Contributing to API Gateway Tripzy

Thank you for considering a contribution. This repository is currently a Node.js package and architecture/documentation foundation. It does not yet contain a running gateway, public routes, authentication integration, or upstream service registry. Contributions must preserve that distinction.

## 🧭 Before opening a change

1. Read the [README](./README.md) and [documentation index](./docs/README.md).
2. Search existing issues and ADRs.
3. Confirm the affected Tripzy service contract and technical owner.
4. Define the problem, expected behavior, and acceptance criteria.
5. Identify security, privacy, timeout, retry, and failure implications.
6. Open an issue for substantial changes before implementation.

Do not invent service names, endpoints, identity claims, deployment URLs, performance numbers, or screenshots.

## 🛠️ Local preparation

~~~bash
git clone https://github.com/Brajesh31/API-Gateway-Tripzy.git
cd API-Gateway-Tripzy
git switch -c feat/your-change
npm install
~~~

The current repository has no runnable server. The test command is a placeholder that exits with an explanatory message. Add a real test suite and update the package scripts with the first runtime implementation.

## 🧱 Engineering principles

- Use explicit trusted service IDs and approved destinations.
- Fail closed on missing security-critical configuration or unknown routes.
- Keep domain logic and domain databases in owning services.
- Filter inbound and outbound headers.
- Set request, response, connection, and upstream time limits.
- Retry only when idempotency and retry budget are explicit.
- Keep logs structured and redacted.
- Avoid unnecessary dependencies.
- Use a supported Node.js LTS release.
- Keep configuration immutable after startup.
- Separate framework adapters from pure policy logic.

## 🧪 Tests expected

Choose tests based on the change:
- Unit tests for route matching, configuration, headers, retry decisions, and error mapping.
- Middleware tests for authentication, authorization, correlation IDs, and quotas.
- Contract tests for every registered upstream route.
- Integration tests using local mock HTTP servers.
- Security regression tests for SSRF, header spoofing, privilege escalation, and malformed inputs.
- Resilience tests for timeouts, disconnects, and dependency failures.

Never make production services a required dependency for ordinary CI.

## 📚 Documentation expected

A route change should update the API reference, service contract register, configuration, architecture/data-flow diagrams, threat model, tests, and operations guide. A security-sensitive change must include negative tests and a threat-model update.

## 🌿 Branch and commit conventions

Suggested branch names: feat/route-registry, fix/header-filtering, security/ssrf-protection, test/contract-suite, docs/service-contracts.

Suggested commit prefixes:
- feat: new runtime capability
- fix: defect correction
- security: security improvement
- test: test coverage
- docs: documentation
- ci: workflow automation
- build: dependency/build configuration
- refactor: behavior-preserving change

See [branching and commits](./docs/branching-and-commits.md).

## 🔍 Pull request checklist

Use the [pull request template](./.github/PULL_REQUEST_TEMPLATE.md). Include:
- Problem and expected behavior.
- Affected route IDs and service owners.
- Auth and permission changes.
- Failure, timeout, and retry behavior.
- Security and privacy impact.
- Actual tests and exact commands executed.
- Configuration and deployment impact.
- Rollout and rollback plan.
- Documentation and diagram updates.

## 🚫 Do not include

Production tokens, passwords, private keys, private customer data, unredacted logs, local .env files, private service endpoints, or unlicensed assets.

## 🧑‍⚖️ Community expectations

Follow the [Code of Conduct](./CODE_OF_CONDUCT.md). Security vulnerabilities must be reported privately using [SECURITY.md](./SECURITY.md), not as public issues.
