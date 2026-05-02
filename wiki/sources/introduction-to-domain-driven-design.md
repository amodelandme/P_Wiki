---
title: "Best Practice - An Introduction to Domain-Driven Design"
type: source
tags: [ddd, csharp, dotnet, msdn, architecture]
date: 2009-02-01
source_file: raw/Best Practice - An Introduction To Domain-Driven Design.md
---

## Summary
Dave Laribee's *MSDN Magazine* (Feb 2009) introduction to [[DomainDrivenDesign]], aimed at .NET developers. Using insurance policy management as a running example, it walks through the core DDD pattern catalog: [[UbiquitousLanguage]], [[BoundedContext]] / [[ContextMap]], [[Entity]], [[ValueObject]], [[Aggregate]], [[DomainService]] vs. application service, and [[Repository]]. It pushes hard on the [[SingleResponsibilityPrinciple]], the [[AnticorruptionLayer]] pattern, and the [[CoreDomain]] as where intellectual capital should be invested. Author: Dave Laribee (Microsoft Architecture MVP, 2007–2008).

## Key Claims
- DDD is a pattern language with solutions at every scale — Bounded Contexts at the top, modules in the middle, aggregate roots at the class level.
- Software is a *shadow* of the true Form (Plato analogy); model-driven design is iterative refinement toward that Form, evolved directly in code.
- The [[UbiquitousLanguage]] is non-negotiable — domain terms must show up as class names; *each Bounded Context owns its own ubiquitous language*.
- Treat entities as **units of behavior, not units of data** — guards against [[AnemicDomainModel]]. Inject dependencies into entity command methods rather than lifting behavior out into services.
- Value objects should be immutable (`readonly` fields in C#) and offer side-effect-free functions; e.g., `Money.AddFunds()` returns a new `Money`.
- Aggregate roots are the *only* entities outside code may reference. `Policy.Renew()` over `Policy.CurrentPeriod().Renew()` — avoids Law of Demeter violations and over-coupling.
- "Big Ball of Mud" results from failing to isolate Bounded Contexts.
- Repositories are themselves a kind of [[AnticorruptionLayer]] — they keep SQL/ORM concepts out of the model.
- One repository per aggregate root is the conventional rule.

## Key Quotes
> "Think of entities as units of behavior rather than as units of data."

> "Each context owns its own Ubiquitous Language."

> "Application Layer ... is kept thin. It does not contain business rules or knowledge, but only coordinates tasks and delegates work to collaborations of domain objects in the next layer down." — quoting Evans

## Connections
- Dave Laribee — author (no entity page; author-only ref).
- [[EricEvans]] — *Domain-Driven Design: Tackling Complexity in the Heart of Software* is the cited foundational text.
- [[DomainDrivenDesign]] — the article's subject.
- [[UbiquitousLanguage]], [[BoundedContext]], [[ContextMap]], [[Entity]], [[ValueObject]], [[Aggregate]], [[Repository]], [[DomainService]], [[AnticorruptionLayer]], [[CoreDomain]], [[BigBallOfMud]], [[ModelDrivenDesign]], [[LayeredArchitecture]] — DDD patterns covered.
- [[AnemicDomainModel]] — explicitly counter-positioned ("entities as units of behavior").
- [[CSharp]] / [[DotNet]] — examples are in C#, Visual Studio is the assumed IDE.

## Contradictions
- None directly. Reinforces and operationalizes the canonical DDD vocabulary in [[ddd-reference-2015]].
