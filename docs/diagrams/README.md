# 📊 Diagram library

This directory stores editable Mermaid source files. GitHub can render Mermaid code fences embedded in Markdown. Standalone .mmd files remain source assets and can be rendered with Mermaid-compatible tooling.

## Diagram catalogue

| Source | Diagram type | Purpose |
|---|---|---|
| [target-architecture.mmd](./target-architecture.mmd) | Architecture flowchart | Client, edge, gateway, policy, registry and upstream boundaries |
| [request-lifecycle.mmd](./request-lifecycle.mmd) | Request flowchart | Request validation, identity, authorization, routing and response |
| [overall-workflow.mmd](./overall-workflow.mmd) | End-to-end flowchart | Full request decision path |
| [api-request-sequence.mmd](./api-request-sequence.mmd) | Sequence diagram | Client, gateway, identity, policy and upstream interaction |
| [authentication-authorization.mmd](./authentication-authorization.mmd) | Security flowchart | Credential verification and permission decision |
| [service-registration.mmd](./service-registration.mmd) | Process flowchart | Contract review and route activation |
| [rate-limiting.mmd](./rate-limiting.mmd) | Policy flowchart | Quota and concurrency decisions |
| [failure-isolation.mmd](./failure-isolation.mmd) | Resilience flowchart | Deadline, retry decision and resource release |
| [module-dependencies.mmd](./module-dependencies.mmd) | Dependency graph | Proposed runtime module boundaries |
| [data-flow.mmd](./data-flow.mmd) | Data-flow diagram | Request data, identity context, upstream and telemetry |
| [error-handling.mmd](./error-handling.mmd) | Decision flowchart | Failure classification and safe public mapping |
| [deployment-pipeline.mmd](./deployment-pipeline.mmd) | Delivery flowchart | CI, staging, canary, monitoring and rollback |
| [contract-lifecycle.mmd](./contract-lifecycle.mmd) | State diagram | Service contract review and lifecycle |

## How to embed a diagram

Use a Mermaid fenced code block in Markdown and keep the source readable. If a diagram is reused, link to its standalone source from the relevant guide. Add a plain-language explanation around complex diagrams for readers who cannot use the visual.

## Accuracy and accessibility rules

- These diagrams describe proposed behavior until matching runtime code exists.
- Generic service labels are placeholders, not Tripzy service names.
- Do not show an identity provider, database, cache, message broker, or deployment platform as deployed unless verified.
- Update diagrams when route policy, trust boundaries, or release workflows change.
- Review the rendered output in GitHub after edits.
- Use clear node labels and explain complex flows in text.
