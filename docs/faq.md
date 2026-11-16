# ❓ Frequently asked questions

## Is this a functioning API gateway?

No. The repository currently contains package metadata and docs only. It has no server or route implementation.

## Why does the manifest reference index.js?

It is a future entry-point convention. The file is not currently present.

## Which Tripzy services are connected?

None are confirmed or configured here. Service IDs, URLs, owners, auth policies and contracts must come from the service teams.

## Which framework is used?

No HTTP framework has been selected or installed. Choose based on runtime support, team familiarity, security requirements and middleware needs.

## Does the gateway use a database?

No. A gateway should generally remain stateless and route domain operations to services that own the data.

## Is authentication implemented?

No. Token verification and route authorization are required before public traffic is enabled.

## Can it be deployed?

No. Add a server, health/readiness checks, validated config, verified upstream definitions, tests, and a deployment/rollback plan first.

## Why does npm test fail?

The package script is a placeholder that exits with an error.

## Does gateway authorization replace service authorization?

No. The gateway may enforce route-level access, but the service owning the data must still enforce resource-level policy.
