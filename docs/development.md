# 🧑‍💻 Development guide

## 1. Development status

The current branch contains package metadata and documentation, not an executable gateway. There is no application source, HTTP framework, server entry point, or automated test suite. This guide describes the sequence and quality bar for introducing the runtime safely.

## 2. Start with service discovery

Before implementing routes, collect from each Tripzy service owner:
- Service identifier and technical owner.
- Internal base URL and network boundary.
- OpenAPI or equivalent contract.
- Authentication and service-to-service identity requirements.
- Resource-level authorization rules.
- Request and response schemas.
- Error/status semantics.
- Latency, availability and concurrency expectations.
- Idempotency guarantees.
- Test environment and escalation channel.

Do not derive endpoints from frontend calls or assumptions without owner confirmation.

## 3. Recommended implementation sequence

1. Select a supported Node.js LTS version and HTTP framework.
2. Add application composition separate from process startup.
3. Implement strict environment parsing and validation.
4. Add liveness/readiness and graceful shutdown.
5. Add route registry with fixed service IDs.
6. Add request context and safe error envelope.
7. Integrate identity verification using a confirmed provider contract.
8. Add route authorization and header filtering.
9. Add outbound client with fixed destinations, timeout and response limits.
10. Add rate/concurrency controls and retry policy.
11. Add telemetry and incident signals.
12. Add deployment and rollback configuration.

Implement one approved route end to end before generalising the framework.

## 4. Repository workflow

~~~bash
git clone https://github.com/Brajesh31/API-Gateway-Tripzy.git
cd API-Gateway-Tripzy
git switch -c feat/route-registry
npm install
~~~

The commands above prepare a working copy. There is no server command to run yet. The current test command is a placeholder failure until real tests are introduced.

## 5. Branch and commit conventions

Suggested branches:
- feat/route-registry
- feat/request-context
- feat/authentication-adapter
- fix/header-filtering
- test/ssrf-regressions
- docs/service-contracts

Suggested commit prefixes:
- feat: user-visible or runtime capability
- fix: defect correction
- test: automated test change
- docs: documentation-only change
- build: dependency or build configuration
- ci: workflow or automation change
- security: security-focused improvement
- refactor: behavior-preserving structural change

## 6. Coding standards

- Use ESM consistently with explicit file extensions.
- Keep configuration immutable after startup.
- Avoid side effects during module import.
- Separate framework adapters from pure policy logic.
- Use async/await with explicit error boundaries.
- Bound network operations with deadlines and cancellation.
- Avoid arbitrary dynamic imports and request-selected destinations.
- Keep logging structured and redacted.
- Do not catch errors only to discard them.
- Keep dependencies minimal and maintained.

## 7. Test expectations

Add unit tests for pure policy logic, middleware tests for identity/authorization, contract tests for every upstream, integration tests with local mock servers, and security regression tests. Never use production services as an automated test dependency.

## 8. Documentation requirements

A route change must update API reference, service register, architecture/data-flow diagrams, configuration, threat model, testing and operations docs. A security-sensitive change requires negative tests and a threat-model update.

## 9. Pull request quality bar

A pull request should include:
- Problem and expected behavior.
- Affected route IDs and service owners.
- Auth and permission changes.
- Failure and timeout behavior.
- Security and privacy impact.
- Automated tests and exact commands run.
- Configuration and deployment changes.
- Rollout and rollback plan.
- Documentation and diagram updates.

Do not report tests as passing unless they were executed successfully.
