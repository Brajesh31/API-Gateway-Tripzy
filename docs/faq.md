# FAQ

## Is this a working API gateway?

No. It is a package scaffold and documentation baseline; no server or route implementation is committed.

## What services does it route?

None are configured. Tripzy service boundaries and upstream URLs must be defined before implementation.

## Does it store data?

No database is present. A gateway should remain stateless unless a documented requirement introduces controlled persistence.

## Can I deploy it now?

No. Add a tested server, secure configuration, health checks, and deployment plan first.
