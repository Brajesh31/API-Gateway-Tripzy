# 🏛️ Repository governance

## Purpose

This document describes decision-making and contribution governance for API Gateway Tripzy. It does not imply a formal organisation or a larger maintainer group than currently exists.

## Roles

| Role | Responsibility |
|---|---|
| Repository maintainer | Owns repository direction, reviews changes, manages releases |
| Gateway contributor | Implements and documents approved gateway behavior |
| Upstream service owner | Confirms service contract, ownership, and operational expectations |
| Security reviewer | Reviews identity, authorization, network boundaries, and abuse cases |
| Platform operator | Owns runtime, deployment, monitoring, and incident response |
| API consumer | Reports integration requirements and compatibility issues |

## Decision principles

- Security and correctness take precedence over convenience.
- Route definitions require verified service contracts.
- No endpoint, upstream URL, or identity provider is invented.
- Breaking changes require a compatibility and deprecation plan.
- Significant architectural trade-offs are recorded in ADRs.
- Runtime claims require code and test evidence.
- Documentation changes accompany behavior changes.

## Route approval

A new public route requires service-owner confirmation, public API review, authentication and permission definition, schema review, timeout/retry/idempotency policy, quota policy, contract tests, security review, and operational ownership.

## Change approval

Reviewers should assess correctness, security, test evidence, compatibility, maintainability, documentation, and rollout risk. A green CI run does not replace code review or service-owner approval.

## Security-sensitive changes

Changes to authentication, authorization, route destinations, header forwarding, TLS, secret handling, rate limits, or error disclosure require explicit security review and negative tests.

## Conflict resolution

Start with the relevant service contract and written architecture decisions. If disagreement remains, record options, consequences, risks, and the decision in an ADR. Revisit the decision when assumptions materially change.
