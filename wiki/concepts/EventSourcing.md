---
title: "Event Sourcing"
type: concept
tags: [architecture, ddd-adjacent, persistence]
sources: [ddd-guide-to-scalable-systems, ddd-reference-2015]
last_updated: 2026-05-02
---

# Event Sourcing

A persistence pattern where the system's source of truth is an append-only log of [[DomainEvent|domain events]]. Current state is derived by replaying events; you can reconstruct the model at any past point in time.

## Properties
- **Audit by construction** — every state change is a recorded event, not just an after-the-fact log entry.
- **Time travel** — replay events up to time `T` to see what the model looked like then.
- **Temporal queries** become natural ("what was this customer's balance on 2024-12-31?").

## Trade-offs
- Schema evolution is harder — old events stay in the log; new readers must handle old shapes.
- Read models must be projected; queries against the event log directly are slow. Often paired with [[CQRS]].
- Eventual consistency is easier to live with than to enforce away.

## Provenance in DDD
Per [[EricEvans]]'s acknowledgements in [[ddd-reference-2015]], Event Sourcing (alongside [[CQRS]]) was the major post-2004 architectural shift in DDD-adjacent practice — credited primarily to Greg Young and Udi Dahan.

## Connections
- [[DomainEvent]] — the unit of storage.
- [[CQRS]] — frequent companion (write side: append events; read side: project into read models).
- [[DomainDrivenDesign]] — adjacent rather than core.
