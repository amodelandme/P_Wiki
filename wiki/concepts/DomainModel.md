---
title: "Domain Model"
type: concept
tags: [ddd, modeling]
sources: [ddd-reference-2015, anemic-domain-model, ddd-guide-to-scalable-systems, introduction-to-domain-driven-design]
last_updated: 2026-05-02
---

# Domain Model

Per [[EricEvans]]: *"A system of abstractions that describes selected aspects of a [[Domain]] and can be used to solve problems related to that domain."*

The model lives in code (and the [[UbiquitousLanguage]]), not in diagrams or documents. It is intentionally selective — leaving out what isn't relevant — and explicit about what *is*.

## What a healthy domain model looks like
- **Behavioral**, not anemic — see [[AnemicDomainModel]].
- **Isolated** from UI, persistence, and infrastructure concerns ([[LayeredArchitecture]]).
- Composed of [[Entity|entities]], [[ValueObject|value objects]], [[Aggregate|aggregates]], [[DomainService|domain services]], and [[DomainEvent|domain events]].
- Refined iteratively against domain insight, not just code transformations (Refactoring Toward Deeper Insight).

## Connections
- [[DomainDrivenDesign]] — the methodology built around this object.
- [[AnemicDomainModel]] — what happens when a domain model has no behavior.
- [[ModelDrivenDesign]] — the practice of keeping code and model tightly aligned.
