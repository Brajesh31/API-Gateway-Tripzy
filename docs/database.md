# 🗄️ Database and shared-state architecture

## 1. Current status

No database driver, ORM, database connection, schema, migration, or persistence layer exists in this repository. There is no evidence that the gateway currently stores any application data.

## 2. Default architecture: stateless gateway

A gateway should generally route requests and apply cross-cutting policies without becoming a system of record. Domain data belongs to the service that owns its lifecycle and business invariants.

Direct access from the gateway to every microservice database is discouraged because it:
- Couples the gateway to internal schemas.
- Bypasses service-owned validation and authorization.
- Creates multiple writers or readers for the same domain data.
- Makes independent service migrations harder.
- Expands the gateway's credential and network attack surface.

## 3. Shared state exceptions

A shared store may be justified for narrowly scoped platform capabilities:

| Use case | Data stored | Required design |
|---|---|---|
| Distributed rate limiting | Counters/window metadata | Atomic updates, expiry and outage policy |
| Idempotency | Key, request fingerprint, operation result reference | Atomic claim, retention and duplicate semantics |
| Short-lived coordination | Temporary locks or leases | TTL, fencing and failure recovery |
| Response caching | Approved response representation | Identity partitioning, TTL and invalidation |
| Security audit | Minimal event metadata | Retention, integrity and access controls |

These are optional design choices, not current features.

## 4. Idempotency state model

If a gateway-level idempotency store is justified, define:
- Key scope: caller, route, tenant, or resource.
- Request fingerprint: method, route and canonical body hash.
- In-progress state and lease expiry.
- Completed result reference or replay response.
- Behavior when upstream outcome is uncertain.
- Retention and cleanup.
- Atomic concurrency handling.
- Encryption and access control.

The domain service must still protect its own durable transaction. A gateway cache cannot guarantee exactly-once business execution by itself.

## 5. Caching rules

Caching is route-specific and must define cacheability, key composition, identity partitioning, TTL, stale behavior, invalidation, privacy, and consistency. Never share authenticated responses across different users or permission contexts. Do not cache mutation responses by default.

## 6. Backup and recovery

There is no database backup process to describe today. If shared persistence is added, define RPO/RTO with the platform owner, encryption, backup frequency, retention, access restrictions, restore tests, and regional recovery requirements.

## 7. Database review checklist

- [ ] The use case cannot be met safely without shared state.
- [ ] The data owner is named.
- [ ] The consistency and failure model are explicit.
- [ ] Retention and cleanup are defined.
- [ ] Secrets and data are encrypted appropriately.
- [ ] Access is least-privilege.
- [ ] Backup and restore are tested.
- [ ] The decision is recorded in an ADR.
