---
title: "Domain Service"
type: concept
tags: [ddd, building-block]
sources: [ddd-reference-2015, introduction-to-domain-driven-design, ddd-guide-to-scalable-systems]
last_updated: 2026-05-02
---

# Domain Service

A stateless object that captures domain logic which **doesn't belong to any single [[Entity]] or [[ValueObject]]** — typically operations or processes that span multiple domain objects, or that domain experts speak about as first-order verbs.

## When to reach for it
- The behavior names a process, not a thing (e.g., `PolicyRenewalProcessor`, `MoneyExchange`).
- Behavior involves multiple aggregates or has external dependencies that don't fit cleanly inside a single entity.

## Caution
Per [[MartinFowler]] in [[anemic-domain-model]]: pulling behavior out into services *too aggressively* hollows out the domain model. The default should be to put logic on entities; reach for a domain service only when there is no natural home.

## Domain service vs. application service
- **Domain service** — stateless, holds domain logic, lives in the domain layer.
- **Application service** — orchestrates use cases (auth, transactions, DTO mapping, infrastructure plumbing); thin, lives in the application layer; *no* business rules.

## Connections
- [[Entity]] — preferred home for behavior; domain services exist for what doesn't fit.
- [[AnemicDomainModel]] — what happens when too much logic migrates into services.
- [[LayeredArchitecture]] — domain services and application services live in different layers.
