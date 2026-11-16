# 🧹 Coding standards and implementation conventions

## 1. Current status

The repository does not yet contain application source code. These standards apply when the gateway runtime is introduced. They should be adapted to the selected HTTP framework without weakening the security requirements.

## 2. JavaScript and Node.js

- Use ES modules consistently.
- Use explicit file extensions for local imports.
- Prefer const and immutable configuration.
- Use async/await for asynchronous operations.
- Avoid unhandled promise rejections.
- Avoid blocking CPU-heavy work on the event loop.
- Do not perform network work during module import.
- Use supported Node.js APIs and maintain dependencies.

## 3. Module design

- Keep application composition separate from server startup.
- Keep route definitions declarative and reviewable.
- Keep pure policy logic independent from the HTTP framework.
- Isolate external identity, quota, and telemetry providers behind adapters.
- Centralize error mapping and header filtering.
- Avoid circular imports and mutable global state.

## 4. Security conventions

- Never trust unverified token claims.
- Never use client input to choose an upstream hostname.
- Never forward all inbound headers by default.
- Never log secrets or full private payloads.
- Fail closed when a required security decision cannot be made.
- Validate configuration at startup.
- Bound all network and parsing operations.
- Do not retry non-idempotent operations automatically.

## 5. Error handling

Use typed/internal error categories and map them to stable public errors at one boundary. Preserve diagnostic cause for restricted logs, but do not return stack traces or private upstream information.

## 6. Naming

Use descriptive lower camelCase for variables and functions, PascalCase for classes/types where used, and lowercase kebab-case for multiword filenames. Route IDs and service IDs should be stable and not contain environment-specific hostnames.

## 7. Comments and documentation

Comments should explain why a non-obvious decision exists, not restate the code. Public route contracts, configuration variables, and security-sensitive behavior require documentation and tests.

## 8. Review checklist

- [ ] Behavior is explicit and testable.
- [ ] No hidden network or startup side effects.
- [ ] Configuration is validated.
- [ ] Error paths are handled.
- [ ] Timeouts and cancellation are present for network work.
- [ ] Sensitive data is redacted.
- [ ] Tests cover negative cases.
- [ ] Relevant docs and ADRs are updated.
