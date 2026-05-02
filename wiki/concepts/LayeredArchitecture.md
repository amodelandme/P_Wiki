---
title: "Layered Architecture"
type: concept
tags: [ddd, architecture]
sources: [ddd-reference-2015, ddd-guide-to-scalable-systems, introduction-to-domain-driven-design]
last_updated: 2026-05-02
---

# Layered Architecture

Per [[EricEvans]]: isolate the [[DomainModel]] from UI, application, and infrastructure code so the domain layer can be "rich enough and clear enough to capture essential business knowledge and put it to work."

The conventional four layers (per Glushach):

1. **User Interface** — thin; uses DTOs/view models; never references domain objects directly.
2. **Application** — stateless coordination of use cases; no business rules; orchestrates domain objects and infrastructure.
3. **Domain** — business logic and rules; the [[DomainModel]] lives here; depends on nothing below it.
4. **Infrastructure** — databases, message brokers, third-party APIs; implements interfaces declared by the domain layer.

Evans notes the **key goal is isolation** — Hexagonal Architecture (ports & adapters) serves the same goal differently and is equally valid.

## Connections
- [[DomainModel]] — what the architecture protects.
- [[Repository]] — interface in the domain layer, implementation in infrastructure (a worked example of the dependency rule).
- [[AnticorruptionLayer]] — closely related insulation pattern at context boundaries.
- [[ddd-reference-2015]] — canonical definition.
