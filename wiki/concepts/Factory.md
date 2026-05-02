---
title: "Factory"
type: concept
tags: [ddd, building-block]
sources: [ddd-reference-2015, ddd-guide-to-scalable-systems]
last_updated: 2026-05-02
---

# Factory

A DDD building block that encapsulates the logic of creating complex domain objects — typically [[Aggregate|aggregates]] whose construction requires non-trivial validation, dependency wiring, or invariant setup. The client gets a fully-formed, valid object without knowing how it was assembled.

## When to reach for it
- Construction violates the [[Aggregate]] root's responsibility (too many parameters, too much wiring).
- Several related objects must be created together to satisfy invariants.
- Construction logic itself is a domain concept (e.g., `OrderFactory.FromShoppingCart(cart)`).

## Connections
- [[Aggregate]] — factories typically produce aggregate roots.
- [[CSharpRecords]] — Zoran Horvat's static `Create` factory pattern (see [[using-record-types-in-cs]]) is the same idea applied at the language level for vertical compatibility.
- [[ddd-reference-2015]] — canonical definition.
