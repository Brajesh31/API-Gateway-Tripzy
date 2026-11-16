# 🖼️ Visual assets

This directory contains original vector artwork used to make the repository easier to scan. These are architecture illustrations, not screenshots of a running gateway.

## Asset inventory

| Asset | Type | Intended use | Status |
|---|---|---|---|
| [Project banner](./banners/api-gateway-tripzy.svg) | SVG hero banner | README header | Present |
| [Gateway architecture](./diagrams/gateway-architecture.svg) | SVG architecture illustration | README architecture section | Present |
| [Request security flow](./illustrations/request-security-flow.svg) | SVG workflow illustration | README request flow section | Present |
| [Mermaid diagrams](../docs/diagrams/README.md) | Editable diagram source | Architecture, sequence, data, error, deployment and policy guides | Present |

## Screenshot policy

The repository does not currently contain a running API server or user interface. Therefore, there are no genuine application screenshots, dashboard captures, mobile screenshots, or live-demo images to publish.

Do not create a mock screenshot and label it as a real product screenshot. When an actual implementation is available, capture genuine UI or API tooling output from a known commit and document the environment and version.

## Asset requirements

- Prefer SVG for diagrams and scalable illustrations.
- Keep alt text descriptive and concise.
- Avoid external tracking resources inside SVG files.
- Do not embed credentials, private endpoints, or personal data.
- Keep diagrams consistent with the documented trust boundaries.
- Preserve editable source files for diagrams whenever practical.
- Check rendering in GitHub after each asset change.

## Adding a new visual

1. Confirm the visual explains a real feature or design decision.
2. Use synthetic labels and placeholder services where implementation is unknown.
3. Save the original asset in the appropriate directory.
4. Add a descriptive inventory row here.
5. Embed it in the relevant guide with useful alt text.
6. Validate the local path in CI.
