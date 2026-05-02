---
title: "Domain Event"
type: concept
tags: [ddd, building-block, event-driven]
sources: [ddd-reference-2015, ddd-guide-to-scalable-systems]
last_updated: 2026-05-02
---

# Domain Event

Per [[EricEvans]]: *"Something happened that domain experts care about."* A first-class domain object representing a discrete past occurrence — immutable, timestamped, often carrying the identities of involved entities.

A pattern **introduced after the 2004 book**, formalized in the [[ddd-reference-2015]] and now considered standard.

## Properties
- **Immutable** — events record the past; the past doesn't change.
- **Descriptive** — named after what happened (`PolicyRenewed`, `OrderShipped`), not what should happen.
- **Self-identifying** — identity can be derived from `(timestamp, entity-id, type)` so duplicates can be detected across nodes.

## Why it matters
- In distributed systems, an entity's state can be inferred from the domain events known to a node — no need for global consistency.
- Domain events are the foundation of [[EventSourcing]] and a common companion to [[CQRS]].
- Decouples cross-aggregate side effects: an aggregate emits an event; subscribers react asynchronously.

## Connections
- [[Aggregate]] — aggregates emit events on state changes.
- [[EventSourcing]] — store-of-events pattern built on domain events.
- [[CQRS]] — commands often emit domain events that update read models.
- [[ddd-reference-2015]] — canonical (post-2004) definition.
