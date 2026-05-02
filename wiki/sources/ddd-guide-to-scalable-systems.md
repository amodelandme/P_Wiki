---
title: "Domain-Driven Design (DDD): A Guide to Building Scalable, High-Performance Systems"
type: source
tags: [ddd, architecture, cqrs, event-sourcing, microservices]
date: 2023-10-05
source_file: raw/Domain-Driven Design (DDD)_ A Guide to Building Scalable, High-Performance Systems.md
---

## Summary
Roman Glushach's 2023 Medium article surveys [[DomainDrivenDesign]] as a programming paradigm for scalable systems. It re-presents the standard DDD pattern catalog — [[Entity]], [[ValueObject]], [[Aggregate]], [[Repository]], [[DomainService]], [[DomainEvent]], [[Factory]], [[BoundedContext]], [[UbiquitousLanguage]] — through a four-layer architecture (UI / Application / Domain / Infrastructure) and connects DDD to modern practices: CQRS, Event Sourcing, Microservices, and asynchronous messaging. Compared to Evans's canonical reference, Glushach folds these adjacent architectural patterns *inside* the DDD presentation rather than treating them as later additions.

## Key Claims
- DDD origin story: domain models popularized by Richard P. Gabriel (1980s–90s), formalized by Eric Evans's 2003/2004 book.
- Architecture: four explicit layers — UI is thin and uses DTOs; Application coordinates use cases statelessly; Domain holds business logic and is isolated; Infrastructure implements domain-defined interfaces.
- Three core principles: focus on the core domain and domain logic; base complex designs on models of the domain; collaborate continuously with domain experts.
- Domain objects should be: encapsulated, cohesive, immutable where possible, and identifiable.
- Value objects: immutable, equatable by attributes, replaceable, shareable across the model without side effects.
- Aggregates are units of consistency and transactional integrity, accessed only through the aggregate root.
- CQRS separates command (writes via the domain model) from query (reads from a read-optimized projection).
- Event Sourcing stores the history of state changes as a sequence of events, allowing reconstruction of past model states.
- Bounded Context manages model complexity by segregating subdomains; the same word can mean different things in different contexts (matches Evans).
- Microservices and messaging are presented as natural deployment fits for cleanly bounded contexts.

## Key Quotes
> "It's not just a collection of data structures, but a living representation of the business domain, encapsulating its behavior and logic."

> "A ubiquitous language is a set of terms and expressions that are shared by developers and domain experts, and used consistently throughout the project."

## Connections
- Roman Glushach — author (no entity page; author-only ref).
- [[EricEvans]] — credited as the formalizer of DDD.
- [[DomainDrivenDesign]] — the article's subject.
- [[UbiquitousLanguage]], [[BoundedContext]], [[Entity]], [[ValueObject]], [[Aggregate]], [[Repository]], [[DomainService]], [[DomainEvent]], [[Factory]], [[LayeredArchitecture]], [[DomainModel]] — patterns covered.
- [[CQRS]], [[EventSourcing]] — modern DDD-adjacent architectural patterns this article integrates into the DDD story.

## Contradictions
- Minor framing tension with [[ddd-reference-2015]]: Evans positions CQRS and Event Sourcing as **post-2004 adjacent architectures** that DDD systems may use; Glushach folds them in as DDD practices. Both views are defensible; flag for any synthesis on "what counts as DDD" vs. "what tends to ship with DDD".
- Coins the term "Commando" for what is conventionally called a Command (in Command Query Responsibility Segregation). This appears to be a translation/typo idiosyncrasy of this source — treat as **Command** in any synthesis.
