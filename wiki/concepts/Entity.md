---
title: "Entity"
type: concept
tags: [ddd, building-block]
sources: [ddd-reference-2015, introduction-to-domain-driven-design, ddd-guide-to-scalable-systems]
last_updated: 2026-05-02
---

# Entity

A [[DomainModel]] object defined by **identity and lifecycle continuity**, not by its attributes. Two entities with identical fields are still different things if their identities differ.

Per [[EricEvans]]: *"When an object is distinguished by its identity, rather than its attributes, make this primary to its definition in the model. ... The model must define what it means to be the same thing."*

## Properties
- **Identifiable** — has a stable identifier (system-assigned or externally meaningful).
- **Stateful** — attributes may change as part of controlled domain behavior.
- **Behavioral** — entities are units of *behavior*, not data; see [[AnemicDomainModel]] for the anti-pattern.
- **Aggregateable** — entities often live inside an [[Aggregate]] under an aggregate root.

## Contrast
- [[ValueObject]] — defined by attributes, immutable, no identity.
- [[CSharpRecords]] — well-suited for [[ValueObject]]s but not entities (entities have identity-based equality, not value equality).

## Connections
- [[Aggregate]] — entities cluster under aggregate roots.
- [[Repository]] — entities are persisted/retrieved through repositories (one repo per aggregate root).
- [[ddd-reference-2015]] — canonical definition.
