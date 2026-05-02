---
title: "Repository"
type: concept
tags: [ddd, building-block, persistence]
sources: [ddd-reference-2015, introduction-to-domain-driven-design, ddd-guide-to-scalable-systems]
last_updated: 2026-05-02
---

# Repository

A collection-like abstraction for persisting and retrieving [[Aggregate|aggregates]]. The repository hides storage details — SQL, ORM, document store, files — behind a domain-oriented interface (`Find`, `Save`, `Delete`, query specifications).

## Conventional rules
- **One repository per aggregate root** — not per entity.
- The repository interface is defined in the domain layer; concrete implementations live in infrastructure.
- Repositories are themselves a kind of [[AnticorruptionLayer]]: they keep persistence concepts (SQL, ORM annotations, stored procedures) from leaking into the [[DomainModel]].

## Connections
- [[Aggregate]] — what repositories store and dispense.
- [[LayeredArchitecture]] — repository interface in domain layer, implementation in infrastructure.
- [[AnticorruptionLayer]] — repositories are a special case.
- [[ddd-reference-2015]] — canonical definition.
