# 🧯 Disaster recovery and continuity

## 1. Current status

No production gateway is deployed from this repository, and no runtime state store or backup system exists. This document is a planning baseline. RTO and RPO values must be agreed with business and platform owners rather than invented.

## 2. Recovery objectives

Define:
- **RTO:** maximum acceptable time to restore the gateway service.
- **RPO:** maximum acceptable data loss for any approved shared state.
- Critical public routes and dependencies.
- Required regions, network paths, certificates, and secrets.
- Recovery authority and escalation path.

A stateless gateway should be reproducible from a trusted artifact and external configuration. Shared state changes the recovery model and must be treated separately.

## 3. Failure scenarios

| Scenario | Recovery focus |
|---|---|
| Gateway process crash | Restart healthy instance and inspect startup logs |
| Deployment regression | Roll back immutable artifact |
| Invalid configuration | Restore known-good config version |
| Identity provider outage | Fail protected routes closed and use provider recovery |
| Upstream service outage | Isolate affected routes and coordinate service owner |
| Quota store outage | Apply approved per-policy fail-open/fail-closed behavior |
| Region/network outage | Restore routing/network path according to platform plan |
| Credential compromise | Revoke/rotate credentials and redeploy trusted config |

## 4. Recovery sequence

~~~mermaid
flowchart TD
  A[Incident declared] --> B[Assess scope and affected services]
  B --> C{Security compromise?}
  C -->|Yes| D[Contain and revoke credentials]
  C -->|No| E[Identify process, config, network or dependency failure]
  D --> F[Restore trusted artifact and configuration]
  E --> F
  F --> G[Validate health and security controls]
  G --> H[Run route smoke and contract checks]
  H --> I{Recovery verified?}
  I -->|No| J[Continue containment and investigation]
  I -->|Yes| K[Restore traffic gradually]
  K --> L[Monitor and complete incident review]
~~~

## 5. Recovery prerequisites

- Source repository and trusted release artifact.
- Deployment manifests and environment configuration history.
- Secret manager access and credential rotation procedure.
- DNS, TLS certificate, and network ownership.
- Upstream service contacts and recovery status.
- Runbooks and alert dashboards.
- Backups and restore instructions for any approved shared state.

## 6. Backup strategy

The default stateless gateway has no application database to back up. Back up or version-control only the configuration and operational state that needs recovery. Never store raw production secrets in Git or ordinary backups.

If a shared quota/idempotency store is added, define backup need, consistency, retention, restore tests, encryption, and owner in an ADR.

## 7. Recovery testing

Perform tabletop exercises before launch and technical recovery tests after deployment. Test artifact rollback, config restoration, secret rotation, certificate renewal, network dependency recovery, and shared-state restoration where applicable.

## 8. Post-recovery review

Record actual recovery time, data loss, missed dependencies, unclear ownership, and runbook gaps. Update recovery objectives only with business approval.
