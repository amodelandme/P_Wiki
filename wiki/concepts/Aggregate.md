---
title: "Aggregate"
type: concept
tags: [ddd, building-block, consistency]
sources: [ddd-reference-2015, introduction-to-domain-driven-design, ddd-guide-to-scalable-systems]
last_updated: 2026-05-02
---

# Aggregate

A cluster of [[Entity|entities]] and [[ValueObject|value objects]] treated as a single unit of consistency. One designated entity is the **aggregate root** — the only object outside code may hold a reference to. All access and modification of the aggregate's interior must go through the root.

## Why
- Constrains coupling: prevents the every-object-references-every-object [[BigBallOfMud]].
- Defines a transactional boundary: an aggregate either succeeds or fails as a unit; invariants hold at its boundary.

## Implications for API design
Per Dave Laribee in [[introduction-to-domain-driven-design]]: prefer `Policy.Renew()` over `Policy.CurrentPeriod().Renew()` — the latter violates the Law of Demeter and leaks internal structure. Aggregate roots should "just figure it out."

## Connections
- [[Entity]] — aggregate roots are entities.
- [[Repository]] — one repository per aggregate root is the conventional rule.
- [[DomainEvent]] — aggregates often emit events on state-changing operations.
- [[ddd-reference-2015]] — canonical definition.
