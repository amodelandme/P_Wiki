# Wiki Index

This file is maintained by the LLM. Updated on every ingest.

## Overview
- [Overview](overview.md) — living synthesis across all sources

## Sources
- [What Makes List<T> So Efficient in .NET?](sources/what-makes-list-so-efficient-in-dotnet.md) — Zoran Horvat's amortized-cost explanation of why `List<T>` is fast despite resizing.
- [Using Record Types in C#](sources/using-record-types-in-cs.md) — Zoran Horvat's tour of C# 9/10 records, `with` expressions, and value-object modeling.
- [Barbara Liskov's italicized paragraph explained](sources/liskov-italicized-paragraph-explained.md) — Plain-English decoding of LSP with the Square/Rectangle violation in C#.
- [Best Practice — An Introduction to Domain-Driven Design](sources/introduction-to-domain-driven-design.md) — Dave Laribee's MSDN walkthrough of the DDD pattern catalog using insurance-policy modeling.
- [Bliki: Anemic Domain Model](sources/anemic-domain-model.md) — Martin Fowler naming the anti-pattern of behavior-less domain objects.
- [Domain-Driven Design Reference (2015)](sources/ddd-reference-2015.md) — Eric Evans's official, CC-BY pattern-summary booklet — the canonical short DDD reference.
- [DDD: A Guide to Building Scalable, High-Performance Systems](sources/ddd-guide-to-scalable-systems.md) — Roman Glushach's modern DDD survey integrating CQRS, Event Sourcing, and microservices.

## Entities
- [Zoran Horvat](entities/ZoranHorvat.md) — .NET author/trainer, Principal Consultant at Coding Helmet.
- [Eric Evans](entities/EricEvans.md) — formalized Domain-Driven Design; author of the 2004 "Big Blue Book" and the 2015 DDD Reference.
- [Martin Fowler](entities/MartinFowler.md) — software-architecture author; coined "Anemic Domain Model"; long-running bliki.
- [Barbara Liskov](entities/BarbaraLiskov.md) — Turing-laureate computer scientist; namesake of the Liskov Substitution Principle.
- [Robert C. Martin](entities/RobertCMartin.md) — "Uncle Bob"; coined the SOLID acronym; author of *Clean Architecture*.

## Concepts
### .NET / C#
- [.NET](concepts/DotNet.md) — Microsoft's managed runtime and BCL.
- [C#](concepts/CSharp.md) — Microsoft's primary .NET language; recent versions emphasize immutability and value semantics.
- [C# Records](concepts/CSharpRecords.md) — C# 9/10 keyword that auto-generates immutable, value-equality classes/structs.
- [Non-destructive Mutation](concepts/NonDestructiveMutation.md) — `with`-expression pattern for copy-and-modify on immutable objects.
- [Value-typed Equality](concepts/ValueTypedEquality.md) — equality by contents rather than reference identity.
- [List<T>](concepts/ListOfT.md) — .NET's resizable generic list, backed by a doubling array.
- [Dynamic Array](concepts/DynamicArray.md) — capacity-doubling array pattern underlying many list types.
- [Amortized Analysis](concepts/AmortizedAnalysis.md) — averaging cost over operation sequences; proves dynamic-array append is O(1).

### DDD core vocabulary
- [Domain-Driven Design](concepts/DomainDrivenDesign.md) — Evans's methodology for modeling complex business domains.
- [Domain](concepts/Domain.md) — the real-world problem space the software addresses.
- [Domain Model](concepts/DomainModel.md) — software representation of selected aspects of the domain.
- [Ubiquitous Language](concepts/UbiquitousLanguage.md) — shared vocabulary spanning code, conversation, and tests within a context.
- [Bounded Context](concepts/BoundedContext.md) — explicit boundary within which a model applies.
- [Context Map](concepts/ContextMap.md) — diagram of bounded contexts and the relationships between them.
- [Anticorruption Layer](concepts/AnticorruptionLayer.md) — translation layer that protects a context from a foreign model.
- [Continuous Integration](concepts/ContinuousIntegration.md) — frequent merging applied to keep a model coherent within its bounded context.
- [Model-Driven Design](concepts/ModelDrivenDesign.md) — practice of binding code so tightly to the model that they evolve together.
- [Layered Architecture](concepts/LayeredArchitecture.md) — UI / Application / Domain / Infrastructure separation that protects domain code.
- [Core Domain](concepts/CoreDomain.md) — the differentiating, value-bearing slice of the domain — where modeling effort pays off.
- [Big Ball of Mud](concepts/BigBallOfMud.md) — anti-pattern of an architecture-less, tightly-coupled system.

### DDD building blocks
- [Entity](concepts/Entity.md) — domain object defined by identity and lifecycle.
- [Value Object](concepts/ValueObject.md) — immutable, identity-less domain object defined by attributes.
- [Aggregate](concepts/Aggregate.md) — cluster of entities + value objects forming a consistency unit, accessed only via the aggregate root.
- [Repository](concepts/Repository.md) — collection-like abstraction for storing and retrieving aggregates.
- [Domain Service](concepts/DomainService.md) — stateless object holding domain logic that doesn't fit on any entity.
- [Domain Event](concepts/DomainEvent.md) — first-class record of something domain experts care about; immutable past occurrence.
- [Factory](concepts/Factory.md) — encapsulates creation of complex aggregates.

### DDD-adjacent / OOP principles
- [Anemic Domain Model](concepts/AnemicDomainModel.md) — anti-pattern: behavior-less domain objects.
- [Liskov Substitution Principle](concepts/LiskovSubstitutionPrinciple.md) — the L in SOLID; subtypes must honor the supertype's contract.
- [Single Responsibility Principle](concepts/SingleResponsibilityPrinciple.md) — the S in SOLID; one reason to change per class.
- [CQRS](concepts/CQRS.md) — Command Query Responsibility Segregation; separates write and read paths.
- [Event Sourcing](concepts/EventSourcing.md) — persistence pattern where the source of truth is an append-only log of domain events.

## Syntheses
