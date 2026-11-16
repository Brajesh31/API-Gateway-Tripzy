# 🗄️ Database and state ownership

## Current status

No database driver, ORM, connection, or persistence layer is present in this repository.

## Default recommendation

Keep the API gateway stateless. Domain data belongs to the service that owns the business rules and lifecycle. Direct gateway access to every service database creates tight coupling, duplicates authorization rules and undermines service boundaries.

## Exceptions requiring explicit design

Shared gateway state may be justified for distributed rate limits, idempotency records, short-lived sessions, carefully scoped response caches, or security audit records.

Before adding storage, document owner, purpose, classification, retention, encryption, least-privilege access, consistency, failure behavior, backup/restore, and privacy impact.

## Caching

Caching must be route-specific. Define cache key, identity/permission partitioning, TTL, invalidation, stale behavior and data sensitivity. Never share authenticated responses across different permission contexts.

## Idempotency

If idempotency is implemented at the gateway, define key scope, request fingerprint, retention, duplicate response behavior, and uncertain upstream outcome handling. The domain service must still protect durable consistency.

## Backups

There is no database and no repository-managed backup process. If shared persistence is introduced, define operational ownership and test restore before production use.
