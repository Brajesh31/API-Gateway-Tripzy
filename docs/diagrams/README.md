# 📊 Diagram library

All files in this directory are editable Mermaid source files. GitHub renders Mermaid in Markdown when diagrams are embedded in supported Markdown contexts. Standalone .mmd files remain source assets and can be rendered by Mermaid-compatible tooling.

## Diagram catalogue

| Source | Diagram type | Purpose |
|---|---|---|
| [target-architecture.mmd](./target-architecture.mmd) | Flowchart | Client, edge, gateway, policy, registry and upstream boundaries |
| [request-lifecycle.mmd](./request-lifecycle.mmd) | Flowchart | Request validation, identity, authorization, routing and response |
| [overall-workflow.mmd](./overall-workflow.mmd) | Flowchart | Full request decision path |
| [data-flow.mmd](./data-flow.mmd) | Flowchart | Request data, identity context, upstream and telemetry |
| [error-handling.mmd](./error-handling.mmd) | Flowchart | Failure classification and safe public mapping |
| [deployment-pipeline.mmd](./deployment-pipeline.mmd) | Flowchart | CI, staging, canary, monitoring and rollback |
| [contract-lifecycle.mmd](./contract-lifecycle.mmd) | State diagram | Service contract review and lifecycle |

## Usage

Embed a diagram in a Markdown file using a Mermaid fenced code block. Keep the original source here when it is reused across documents. Add a text explanation near complex diagrams so the meaning is not conveyed by visuals alone.

## Accuracy rules

- These diagrams describe proposed behavior until matching runtime code exists.
- Generic service labels are placeholders, not Tripzy service names.
- Do not show an auth provider, database, cache, or message broker as deployed unless verified.
- Update diagrams when route policy, trust boundaries, or release workflows change.
- Validate syntax and review the rendered output in GitHub after edits.
