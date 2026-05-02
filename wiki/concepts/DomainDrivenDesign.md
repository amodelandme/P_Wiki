---
title: "Domain-Driven Design"
type: concept
tags: [ddd, methodology, architecture]
sources: [ddd-reference-2015, introduction-to-domain-driven-design, anemic-domain-model, ddd-guide-to-scalable-systems]
last_updated: 2026-05-02
---

# Domain-Driven Design (DDD)

A software-design approach formalized by [[EricEvans]] in *Domain-Driven Design: Tackling Complexity in the Heart of Software* (2004). Per the canonical [[ddd-reference-2015]], DDD is:

1. **Focus on the [[CoreDomain]]**.
2. **Explore models in creative collaboration** between domain practitioners and software practitioners.
3. **Speak a [[UbiquitousLanguage]]** within an explicitly [[BoundedContext|bounded context]].

## Pattern Catalog
- **Building blocks** ([[ModelDrivenDesign]] tactics): [[Entity]], [[ValueObject]], [[DomainEvent]], [[DomainService]], [[Aggregate]], [[Repository]], [[Factory]], [[LayeredArchitecture]].
- **Strategic design**: [[BoundedContext]], [[ContextMap]], [[AnticorruptionLayer]], [[CoreDomain]], [[BigBallOfMud]].
- **Adjacent / post-2004**: [[CQRS]], [[EventSourcing]], Hexagonal Architecture, microservices.

## What it is *not*
- Not a [[AnemicDomainModel|bag-of-getters]] design with logic in services — that defeats the point.
- Not a database schema dressed in objects.
- Not a silver bullet for simple CRUD; the cost-benefit only pencils out for complex domains.

## Connections
- [[EricEvans]] — formalizer.
- [[MartinFowler]] — major communicator and pattern documentarian.
- [[ddd-reference-2015]] — canonical short reference.
