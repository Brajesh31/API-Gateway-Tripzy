# 🎯 Project overview

## What this repository is

API Gateway Tripzy is a Node.js project intended to become a controlled client-facing entry point for a Tripzy microservices ecosystem. The current repository is a package and documentation scaffold, not an operational gateway.

## Problem to address

As clients interact with multiple services, they can become coupled to different hostnames, authentication conventions, error formats, and reliability behavior. A gateway can centralise cross-cutting controls while leaving domain logic with the service that owns it.

## Goals

1. Define a secure and explicit public API boundary.
2. Maintain a reviewed route and upstream registry.
3. Centralise identity verification, route authorization, rate limits, and request constraints.
4. Make upstream failure behavior predictable and observable.
5. Preserve service ownership of business rules and persistence.
6. Provide repeatable contract, integration, security, and resilience tests.

## Non-goals

Unless a product decision explicitly changes scope, the gateway should not:
- Reimplement booking, trip, user, payment, or search domain rules.
- Connect directly to each microservice database.
- Accept arbitrary client-provided upstream URLs.
- Expose internal service discovery details.
- Become a generic open proxy.
- Turn downstream partial failure into misleading success.

## Current repository evidence

- A root package manifest exists.
- The manifest points at a future `index.js`, but that file is absent.
- No HTTP framework or proxy/auth dependencies are installed.
- No route handlers or upstream destinations are configured.
- The current test script is a placeholder that exits with an error.
- The environment example contains suggested names only.
- Existing project docs do not prove that runtime features exist.

## Actors

| Actor | Expected interaction |
|---|---|
| Web/mobile client | Uses stable public API routes |
| Partner integration | Uses explicitly exposed and authorized endpoints |
| Identity provider | Issues/verifies identity credentials |
| Upstream service | Owns domain operations and records |
| Platform operator | Configures, deploys, monitors and recovers gateway |
| Security reviewer | Reviews auth, route boundaries and abuse controls |

## Success measures to define

Candidate measures include request success ratio, p95/p99 latency, upstream timeouts, auth/authorization rejection rates, rate-limit correctness, trace propagation coverage, and API compatibility. Numeric targets must be agreed with product and service owners, not invented in this repository.
