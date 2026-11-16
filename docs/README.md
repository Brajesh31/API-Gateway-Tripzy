# 📚 Documentation library

This directory is the technical reference for API Gateway Tripzy. It separates current repository facts from proposed runtime behavior. No guide should be interpreted as proof that a feature exists in code.

## 🧭 Start here

| If you need to... | Read |
|---|---|
| Understand the project's purpose and actual status | [Project overview](./project-overview.md) and [Limitations](./limitations.md) |
| Understand the target architecture | [Architecture](./architecture.md) and [System design](./system-design.md) |
| Find the current files and their responsibilities | [Project structure](./project-structure.md) and [Modules](./modules.md) |
| Understand the intended request pipeline | [Workflow](./workflow.md), [Data flow](./data-flow.md), and [Diagrams](./diagrams/README.md) |
| Define an API route | [API reference](./api.md) and [Service contracts](./service-contracts.md) |
| Design identity checks | [Authentication and authorization](./authentication-authorization.md) |
| Configure the future runtime | [Configuration](./configuration.md) and [Installation](./installation.md) |
| Build or change the project | [Development](./development.md) and [Usage](./usage.md) |
| Review security boundaries | [Security](./security.md), [Threat model](./threat-model.md), and [Headers and proxy](./headers-and-proxy.md) |
| Design quotas and failure behavior | [Rate limiting](./rate-limiting.md), [Error handling](./error-handling.md), and [Resilience](./resilience.md) |
| Plan tests | [Testing](./testing.md) |
| Prepare for release | [Deployment](./deployment.md), [Release management](./release-management.md), and [Versioning](./versioning-and-compatibility.md) |
| Operate and recover the service | [Operations](./operations.md), [Observability](./observability.md), [Incident response](./incident-response.md), and [Future disaster recovery](./disaster-recovery.md) |
| Understand privacy and accessibility | [Privacy](./privacy-data-handling.md), [Data governance](./data-governance.md), [Accessibility](./accessibility.md), and [Internationalization](./internationalization.md) |
| Understand current gaps and future work | [Technical debt](./technical-debt.md), [Limitations](./limitations.md), [Future scope](./future-scope.md), and [Roadmap](../ROADMAP.md) |

## 🏗️ Architecture and product

- [Project overview](./project-overview.md)
- [Features and implementation status](./features.md)
- [Architecture](./architecture.md)
- [System design](./system-design.md)
- [Project structure](./project-structure.md)
- [Module boundaries](./modules.md)
- [Workflow](./workflow.md)
- [Data flow](./data-flow.md)
- [Glossary](./glossary.md)

## 🔌 API and service integration

- [API reference and conventions](./api.md)
- [Service contract register](./service-contracts.md)
- [Usage patterns](./usage.md)
- [Versioning and compatibility](./versioning-and-compatibility.md)
- [Authentication and authorization](./authentication-authorization.md)
- [Headers and proxy trust](./headers-and-proxy.md)
- [Error handling](./error-handling.md)
- [Rate limiting](./rate-limiting.md)
- [Resilience](./resilience.md)

## ⚙️ Engineering and quality

- [Installation](./installation.md)
- [Development](./development.md)
- [Configuration](./configuration.md)
- [Database and shared state](./database.md)
- [Testing strategy](./testing.md)
- [Performance and capacity](./performance.md)
- [Accessibility](./accessibility.md)
- [Internationalization](./internationalization.md)

## 🔐 Security, privacy and governance

- [Security engineering](./security.md)
- [Threat model](./threat-model.md)
- [Privacy and data handling](./privacy-data-handling.md)
- [Data governance](./data-governance.md)
- [Governance](../GOVERNANCE.md)
- [Architecture Decision Records](./ADRs/README.md)

## 🚢 Operations and delivery

- [Deployment](./deployment.md)
- [Release management](./release-management.md)
- [Operations](./operations.md)
- [Observability](./observability.md)
- [Incident response](./incident-response.md)
- [Disaster recovery](./disaster-recovery.md)
- [Troubleshooting](./troubleshooting.md)

## 🧭 Planning and reference

- [Limitations](./limitations.md)
- [Future scope](./future-scope.md)
- [Technical debt register](./technical-debt.md)
- [References and standards](./references.md)
- [FAQ](./faq.md)

## 📊 Diagram sources

Editable Mermaid sources are stored in [diagrams/](./diagrams/). They cover target architecture, request lifecycle, overall workflow, routing, data flow, error handling, contract lifecycle, and deployment pipeline. Each diagram must be kept consistent with implementation and service contracts.

## 🧾 Documentation governance

1. Label proposed behavior clearly.
2. Never invent service names, destinations, APIs, security controls, screenshots, or performance results.
3. Use synthetic examples and reserved example domains.
4. Update API, service register, tests, security model, operations guide, and README together when a route changes.
5. Include failure behavior, timeout, retry, idempotency, and quota details in route contracts.
6. Keep diagrams editable and link them from the relevant guide.
7. Remove stale claims only after confirming the actual implementation.
8. Review local links, image references, Mermaid fences, and installation commands before merging.
