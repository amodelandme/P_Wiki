---
title: "Overview"
type: synthesis
tags: []
sources: [what-makes-list-so-efficient-in-dotnet, using-record-types-in-cs, liskov-italicized-paragraph-explained, introduction-to-domain-driven-design, anemic-domain-model, ddd-reference-2015, ddd-guide-to-scalable-systems]
last_updated: 2026-05-02
---

# Overview

*This page is maintained by the LLM. It is updated on every ingest to reflect the current synthesis across all sources.*

The wiki has cohered around **idiomatic, design-aware [[DotNet]] / [[CSharp]] development with a strong [[DomainDrivenDesign]] backbone**. Three threads run through the corpus:

1. **Collection internals.** [[ZoranHorvat]]'s [[what-makes-list-so-efficient-in-dotnet|article on List<T>]] reconstructs the [[DynamicArray]] doubling pattern and uses [[AmortizedAnalysis]] to show each element is written ~2× on average — a small constant-factor cost in exchange for transparent resizing.

2. **Modeling values immutably.** [[ZoranHorvat]]'s [[using-record-types-in-cs|piece on records]] shows how [[CSharpRecords]] (C# 9/10) collapse value-class boilerplate into one keyword, giving [[ValueTypedEquality]], `init`-only properties, and [[NonDestructiveMutation]] via `with`. Records map directly onto Evans's [[ValueObject]] pattern.

3. **Domain-Driven Design.** Four sources triangulate the DDD vocabulary:
   - [[ddd-reference-2015]] — Eric Evans's canonical short reference; defines [[UbiquitousLanguage]], [[BoundedContext]], [[Entity]], [[ValueObject]], [[Aggregate]], [[Repository]], [[DomainService]], [[DomainEvent]], [[Factory]], [[LayeredArchitecture]], [[ContextMap]], [[AnticorruptionLayer]], [[CoreDomain]].
   - [[introduction-to-domain-driven-design]] — Dave Laribee operationalizes the catalog in C#/.NET with insurance-policy examples.
   - [[anemic-domain-model]] — [[MartinFowler]] names the anti-pattern that makes DDD's pattern catalog necessary in the first place.
   - [[ddd-guide-to-scalable-systems]] — Roman Glushach folds [[CQRS]] and [[EventSourcing]] in alongside the classic patterns (a slight framing difference from Evans, who treats them as DDD-adjacent rather than core).

The OO-foundations layer is held up by [[liskov-italicized-paragraph-explained]] — a plain-English [[LiskovSubstitutionPrinciple]] decoding via the canonical `Square : Rectangle` violation, which sits squarely under the SOLID umbrella that DDD presupposes.

The unifying thread: **be deliberate about the cost of the abstractions you reach for, and prefer behaviorally rich, value-semantic types where identity isn't load-bearing.** Records, value objects, aggregates, and a thin application layer over a fat domain layer are all expressions of the same instinct.

## Open Threads
- The DDD reference's deeper coverage (pp. 14–58 — Services, Modules, Aggregates, Repositories, Factories, Supple Design, Distillation, Large-scale Structure) is summarized only at the TOC level; deeper Evans phrasing on those patterns may warrant a re-ingest.
- No comparisons yet between `List<T>` and other .NET collections (`LinkedList<T>`, `HashSet<T>`, `Dictionary<TKey,TValue>`).
- No coverage of `List<T>` removal/insertion-in-middle costs; only append.
- No discussion of memory pressure or GC implications of doubling.
- `record struct` mentioned but not explored — value-type vs reference-type record trade-offs are open.
- DDD coverage is concept-heavy; no in-the-wild case study yet (no source documenting how a real codebase applied — or failed to apply — these patterns).
- Tactical-vs-strategic DDD balance: sources skew toward tactical patterns. Evans's strategic chapters (Distillation, Large-scale Structure) are referenced but not fleshed out.
- Framing tension to track: Glushach treats CQRS/Event Sourcing as part of DDD; Evans treats them as adjacent. Worth a synthesis if more sources weigh in.
- LSP coverage is shallow (one Claude conversation); the canonical Liskov–Wing 1994 paper would be the natural depth source.
- DDD presupposes SOLID but only [[LiskovSubstitutionPrinciple]] and [[SingleResponsibilityPrinciple]] have pages; OCP, ISP, DIP are unaccounted-for.
