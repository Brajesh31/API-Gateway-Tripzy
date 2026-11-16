# 📚 Documentation library

This directory is the technical reference for API Gateway Tripzy. It separates verified repository facts from proposed runtime behavior. No guide should be interpreted as proof that a feature exists in code.

## 🧭 Start here

| If you need to... | Read |
|---|---|
| Understand project purpose and actual status | [Project overview](./project-overview.md), [Features](./features.md), [Limitations](./limitations.md) |
| Understand target architecture | [Architecture](./architecture.md), [System design](./system-design.md), [Data flow](./data-flow.md) |
| Find files and module responsibilities | [Project structure](./project-structure.md), [Modules](./modules.md) |
| Understand request and release workflows | [Workflow](./workflow.md), [Diagrams](./diagrams/README.md) |
| Propose a public API route | [API reference](./api.md), [Service contracts](./service-contracts.md), [Route template](./route-contract-template.md) |
| Design identity checks | [Authentication and authorization](./authentication-authorization.md) |
| Configure and run local development | [Installation](./installation.md), [Configuration](./configuration.md), [Usage](./usage.md) |
| Change the project safely | [Development](./development.md), [Coding standards](./coding-standards.md), [Branching and commits](./branching-and-commits.md) |
| Review security boundaries | [Security](./security.md), [Threat model](./threat-model.md), [Headers and proxy](./headers-and-proxy.md) |
| Design quotas and failure behavior | [Rate limiting](./rate-limiting.md), [Error handling](./error-handling.md), [Resilience](./resilience.md) |
| Plan tests | [Testing](./testing.md) |
| Prepare a release | [Deployment](./deployment.md), [Release management](./release-management.md), [Versioning](./versioning-and-compatibility.md) |
| Operate and recover the service | [Operations](./operations.md), [Observability](./observability.md), [Incident response](./incident-response.md), [Disaster recovery](./disaster-recovery.md), [Backup strategy](./backup-strategy.md) |
| Understand privacy and accessibility | [Privacy](./privacy-data-handling.md), [Data governance](./data-governance.md), [Accessibility](./accessibility.md), [Internationalization](./internationalization.md) |
| Understand current gaps and future work | [Technical debt](./technical-debt.md), [Limitations](./limitations.md), [Future scope](./future-scope.md), [Roadmap](../ROADMAP.md) |

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
- [Route contract template](./route-contract-template.md)
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
- [Coding standards](./coding-standards.md)
- [Branching and commits](./branching-and-commits.md)
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
- [Repository governance](../GOVERNANCE.md)
- [Maintainers](../MAINTAINERS.md)
- [Architecture Decision Records](./ADRs/README.md)
- [ADR template](./ADRs/template.md)

## 🚢 Operations and delivery

- [Deployment](./deployment.md)
- [Release management](./release-management.md)
- [Operations](./operations.md)
- [Observability](./observability.md)
- [Incident response](./incident-response.md)
- [Disaster recovery](./disaster-recovery.md)
- [Backup strategy](./backup-strategy.md)
- [Troubleshooting](./troubleshooting.md)

## 🧭 Planning and reference

- [Limitations](./limitations.md)
- [Future scope](./future-scope.md)
- [Technical debt register](./technical-debt.md)
- [References and standards](./references.md)
- [FAQ](./faq.md)

## 📊 Diagram sources

Editable Mermaid sources are stored in [diagrams/](./diagrams/). They cover target architecture, request lifecycle, overall workflow, data flow, error handling, contract lifecycle, and deployment pipeline. Keep them aligned with implementation and verified service contracts.

## 🧾 Documentation governance

1. Label proposed behavior clearly.
2. Never invent service names, destinations, APIs, security controls, screenshots, or performance results.
3. Use synthetic examples and reserved example domains.
4. Update API, service register, tests, security model, operations guide, and README when a route changes.
5. Include failure behavior, timeout, retry, idempotency, and quota details in route contracts.
6. Keep diagrams editable and link them from relevant guides.
7. Remove stale claims only after confirming the actual implementation.
8. Review local links, image references, Mermaid fences, and installation commands before merging.
