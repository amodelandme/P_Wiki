---
title: "CQRS"
type: concept
tags: [architecture, ddd-adjacent, event-driven]
sources: [ddd-guide-to-scalable-systems, ddd-reference-2015]
last_updated: 2026-05-02
---

# CQRS — Command Query Responsibility Segregation

An architectural pattern that splits a system's API into two halves:

- **Commands** — write paths; mutate state via the [[DomainModel]] (`CreateCustomer`, `RenewPolicy`).
- **Queries** — read paths; return data, typically from a denormalized read model optimized for the query shape, separate from the write model.

## Why
- The data shape that's natural for enforcing invariants (deeply nested aggregates) is rarely the shape that's natural for displaying lists or running reports.
- Commands and queries have different scaling characteristics; separating them lets each scale independently.
- Often paired with [[EventSourcing]] (commands emit [[DomainEvent|domain events]] that are projected into one or more read models).

## Provenance in DDD
Per [[EricEvans]]'s 2014 acknowledgements in [[ddd-reference-2015]], CQRS (Greg Young, Udi Dahan) and Event Sourcing were *the* major architectural shift since the 2004 book. Evans treats them as DDD-adjacent rather than as DDD itself.

## Connections
- [[DomainEvent]] — the typical write-side output that drives read-model projections.
- [[EventSourcing]] — frequent companion pattern.
- [[DomainDrivenDesign]] — adjacent rather than core.
