# 🌿 Branching strategy and commit conventions

## 1. Purpose

A consistent branch and commit history makes security review, release preparation, and rollback easier. This repository currently has a small documentation-focused scope, but the same conventions can scale as runtime code is introduced.

## 2. Branch strategy

Work on focused branches from the default branch. Suggested names:
- feat/route-registry
- feat/identity-adapter
- feat/request-context
- fix/header-filtering
- security/ssrf-protection
- test/contract-suite
- docs/service-contracts

Avoid long-lived branches that combine unrelated runtime, documentation, and workflow changes.

## 3. Commit message format

Use a short prefix and imperative description:

~~~text
feat: add explicit route registry
fix: reject untrusted forwarding headers
security: validate upstream redirect targets
test: cover expired token handling
docs: expand service contract guide
ci: validate Mermaid and local documentation links
~~~

## 4. Commit scope

A commit should represent one coherent change. Keep generated files, lockfiles, source, and docs consistent. Do not include secrets, temporary exports, local environment files, or unrelated formatting churn.

## 5. Pull request workflow

1. Create a focused branch.
2. Link an issue or ADR.
3. Implement and test the change.
4. Update relevant docs and diagrams.
5. Review the diff for accidental files and secret exposure.
6. Run required checks.
7. Open a PR with service-owner and security context.
8. Merge only after required reviews and CI gates.

## 6. Release tags

Use annotated tags for releases once the runtime and compatibility policy are established. Keep gateway API version, npm package version, and deployment artifact version distinct.

## 7. Rollback support

Keep changes small and release artifacts immutable. Document configuration migrations and upstream compatibility. A revert of gateway code may not undo an upstream database or contract migration.
