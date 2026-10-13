# Architecture

No gateway server is currently implemented. The intended architecture is a public gateway routing authorized client requests to Tripzy microservices through explicit upstream configuration.

Before implementation, define service ownership, route versions, authentication boundary, service discovery, timeout/retry policy, health semantics, and whether the gateway performs aggregation or only proxying.
