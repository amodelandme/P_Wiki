---
title: "Domain-Driven Design Reference (2015)"
type: source
tags: [ddd, reference, evans, canonical, patterns]
date: 2015-03-01
source_file: raw/DDD_Reference_2015-03.pdf
---

## Summary
Eric Evans's official *Domain-Driven Design Reference: Definitions and Pattern Summaries* (CC-BY-4.0, March 2015). A 59-page pattern-summary booklet derived from his 2004 "Big Blue Book" *Domain-Driven Design: Tackling Complexity in the Heart of Software*, plus several patterns introduced after the book — notably **[[DomainEvent]]**, **Partnership**, and **[[BigBallOfMud]]**. The reference is structured in six parts: (I) Putting the Model to Work, (II) Building Blocks of Model-Driven Design, (III) Supple Design, (IV) Context Mapping for Strategic Design, (V) Distillation for Strategic Design, (VI) Large-scale Structure for Strategic Design.

## Key Claims
- DDD is a three-point philosophy: **focus on the [[CoreDomain]]; explore models in collaboration between domain and software practitioners; speak a [[UbiquitousLanguage]] within an explicit [[BoundedContext]]**.
- Canonical definitions: *domain* = sphere of knowledge/activity; *model* = a system of abstractions describing aspects of a domain; *context* = the setting that determines a statement's meaning; *bounded context* = boundary within which a particular model is defined.
- "Statements about a model can only be understood in a context."
- The Pattern Language Overview shows the connectivity: [[ModelDrivenDesign]] structures via [[Entity]] / [[ValueObject]] / [[DomainEvent]] / [[DomainService]] / [[Aggregate]] / [[Repository]] / [[Factory]], all wrapped in [[LayeredArchitecture]], coordinated by [[BoundedContext]] and [[ContextMap]] at the strategic level.
- *Entities*: defined by **identity and lifecycle continuity**, not attributes. "The model must define what it means to be the same thing."
- *Value Objects*: defined only by attributes; **immutable**, with side-effect-free functions; no identity.
- *Domain Events* (post-2004 addition): "Something happened that domain experts care about." Immutable records of past occurrences; useful in distributed systems where state can be inferred from known events.
- *Layered Architecture*: isolate the domain model layer from UI, application, and infrastructure. "The key goal here is isolation" — Hexagonal Architecture serves the same goal.
- *Continuous Integration* applies to the model: merge frequently within a Bounded Context to prevent model fragmentation.
- *Hands-on Modelers*: anyone shaping the model must touch the code; modelers separated from implementation produce impractical models.
- *Refactoring Toward Deeper Insight*: the goal isn't a "right" model but an iterative refinement driven by domain insight, not just code transformations.
- The 2014 acknowledgements credit CQRS / Event Sourcing (Greg Young, Udi Dahan), NoSQL, lighter functional/decoupled stacks, and Vaughn Vernon's *Implementing Domain-Driven Design* as the major post-2004 shifts.

## Key Quotes
> "Domain-Driven Design is an approach to the development of complex software in which we: 1. Focus on the core domain. 2. Explore models in a creative collaboration of domain practitioners and software practitioners. 3. Speak a ubiquitous language within an explicitly bounded context."

> "When an object is distinguished by its identity, rather than its attributes, make this primary to its definition in the model. ... The model must define what it means to be the same thing."

> "Treat the value object as immutable. Make all operations Side-effect-free Functions that don't depend on any mutable state."

> "Recognize that a change in the language is a change to the model."

## Connections
- [[EricEvans]] — author.
- [[DomainDrivenDesign]] — this is the canonical short reference.
- All core DDD concepts: [[UbiquitousLanguage]], [[BoundedContext]], [[ContextMap]], [[ModelDrivenDesign]], [[LayeredArchitecture]], [[Entity]], [[ValueObject]], [[DomainEvent]], [[DomainService]], [[Aggregate]], [[Repository]], [[Factory]], [[CoreDomain]], [[AnticorruptionLayer]], [[BigBallOfMud]].
- [[MartinFowler]] — credited for clear communication of DDD concepts and definitive pattern documentation.

## Contradictions
- None within the wiki. This source is the canonical anchor; secondary sources ([[introduction-to-domain-driven-design]], [[ddd-guide-to-scalable-systems]], [[anemic-domain-model]]) align with it. Glushach's piece adds CQRS and Event Sourcing as in-domain practices, which Evans positions as adjacent, post-2004 architectural options rather than core DDD.

## Note
This source page summarizes pages 1–13 of the PDF directly (Bounded Context, Ubiquitous Language, Continuous Integration, Model-Driven Design, Hands-on Modelers, Refactoring Toward Deeper Insight, Layered Architecture, Entities, Value Objects, Domain Events) plus the table of contents and Pattern Language Overview. Coverage of Services, Modules, Aggregates, Repositories, Factories, Supple Design, Context Mapping (pp. 14–38), Distillation (pp. 39–46), and Large-scale Structure (pp. 47–58) is by reference only and may warrant a deeper re-ingest if a specific concept needs canonical Evans phrasing.
