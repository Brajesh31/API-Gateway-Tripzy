# 🔖 API versioning and compatibility policy

## 1. Purpose

The gateway creates a public contract that may outlive the internal implementation behind it. Versioning must protect clients from accidental breaking changes while allowing service teams to evolve their APIs.

No public API version is implemented in this repository yet. The policy below must be agreed before publishing routes.

## 2. Versioning strategy

Choose one consistent public versioning mechanism, such as a versioned path prefix or a documented media-type version. Do not mix mechanisms without a clear reason.

A path-based example could look like /api/v1/resource. This is an illustration, not an active Tripzy route.

## 3. Compatibility expectations

Generally compatible changes may include adding optional response fields, adding a new route, or broadening accepted input only when it does not create ambiguity. Potentially breaking changes include removing/renaming fields, changing types, altering authentication, changing error codes, narrowing accepted input, changing status semantics, or changing idempotency behavior.

## 4. Deprecation lifecycle

~~~mermaid
stateDiagram-v2
  [*] --> Active
  Active --> Deprecated: Notice and migration guide
  Deprecated --> SunsetScheduled: Date and replacement announced
  SunsetScheduled --> Retired: Sunset completed
  Deprecated --> Active: Deprecation withdrawn
  SunsetScheduled --> Active: Sunset cancelled
  Retired --> [*]
~~~

A deprecation notice should identify affected routes, reason, replacement, migration steps, target sunset date, and support owner. The notice period must be agreed with consumers and legal/product requirements.

## 5. Upstream version independence

A public route may temporarily map to a different internal service version, but the gateway must preserve the public contract. Do not forward incompatible upstream response fields or status behavior without review.

## 6. Contract verification

For each public route, maintain:
- Request schema and response schema.
- Required/optional fields.
- Error codes and status mappings.
- Authentication and permission behavior.
- Idempotency and retry semantics.
- Deprecation and migration notes.
- Automated compatibility tests.

## 7. Change checklist

- [ ] Is the change compatible with existing clients?
- [ ] Are request and response schemas updated?
- [ ] Are error codes/statuses stable?
- [ ] Is auth behavior unchanged or intentionally versioned?
- [ ] Are contract tests updated?
- [ ] Are consumers and service owners notified?
- [ ] Is a migration and rollback path documented?
