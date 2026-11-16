# 📦 Release management

## Current status

The repository has not published an operational gateway release. Version 0.1.0 in package metadata is a project scaffold version, not proof of a deployed runtime.

## 1. Release principles

- Build from a reviewed commit.
- Use immutable artifacts.
- Keep configuration external to the artifact.
- Validate security-critical configuration before serving traffic.
- Deploy progressively where infrastructure supports it.
- Keep a known-good rollback artifact.
- Coordinate gateway and upstream contract changes.
- Publish release notes with compatibility impact.

## 2. Release workflow

~~~mermaid
flowchart TD
  A[Release candidate commit] --> B[CI and security checks]
  B --> C[Review route/API contract changes]
  C --> D[Build immutable artifact]
  D --> E[Deploy staging]
  E --> F[Smoke, contract and security tests]
  F --> G{Approved?}
  G -->|No| H[Fix or abandon candidate]
  G -->|Yes| I[Canary release]
  I --> J[Monitor release indicators]
  J --> K{Healthy?}
  K -->|No| L[Rollback and investigate]
  K -->|Yes| M[Complete rollout]
  M --> N[Publish release notes]
~~~

## 3. Release checklist

- [ ] Version and changelog updated.
- [ ] CI and security checks reviewed.
- [ ] API compatibility reviewed.
- [ ] Upstream owners approved affected contracts.
- [ ] Environment and secret changes reviewed.
- [ ] Staging smoke and contract tests pass.
- [ ] Observability and alerts are ready.
- [ ] Rollback artifact and procedure are available.
- [ ] Release notes include known limitations.

## 4. Rollback considerations

A gateway rollback does not automatically roll back a downstream service or database migration. Prefer backward-compatible service changes and coordinated deployment windows. Record the deployed gateway commit, artifact digest, config version, and reason for rollback.

## 5. Versioning

Choose semantic versioning once the runtime API and compatibility commitments are defined. Keep package version, gateway API version, and deployment artifact version conceptually separate.
