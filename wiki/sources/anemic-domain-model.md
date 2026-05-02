---
title: "bliki: Anemic Domain Model"
type: source
tags: [ddd, antipattern, oop, fowler]
date: 2003-11-24
source_file: raw/bliki_ Anemic Domain Model.md
---

## Summary
[[MartinFowler]]'s short bliki entry naming and dissecting the **[[AnemicDomainModel]]** anti-pattern: domain objects with rich relationships but almost no behavior — bags of getters and setters whose logic lives in service classes. Fowler argues this is procedural style hiding behind OO syntax, paying the full cost of an object/relational mapping while losing the benefit of a behaviorally rich [[DomainModel]]. He references conversations with [[EricEvans]] and quotes *Domain-Driven Design* on layering.

## Key Claims
- Anemic models have noun-shaped objects and rich graphs but no behavior; logic lives in "service" classes that act *on* the data.
- This is **procedural design dressed up as OO** — and contradicts the basic premise of object orientation (data + process together).
- The anti-pattern incurs all costs of a domain model (mainly O/R mapping complexity) without yielding benefits — at that point, a Transaction Script (Fowler, *PoEAA*) would have been simpler.
- A proper *Service Layer* is **thin** and orchestrates rich domain objects; it does not absorb domain logic. Evans's "Application Layer" matches this — it has no business rules, only coordinates tasks.
- Likely root causes: developers from data-centric backgrounds; technologies that nudge you this way (Fowler cites J2EE Entity Beans, contrasting with POJO domain models).
- Heuristic: *"the more behavior you find in the services, the more likely you are to be robbing yourself of the benefits of a domain model."*

## Key Quotes
> "The fundamental horror of this anti-pattern is that it's so contrary to the basic idea of object-oriented design; which is to combine data and process together. The anemic domain model is really just a procedural style design."

> "If all your logic is in services, you've robbed yourself blind."

> Quoting Evans: "Application Layer ... It does not contain business rules or knowledge, but only coordinates tasks and delegates work to collaborations of domain objects in the next layer down."

## Connections
- [[MartinFowler]] — author.
- [[EricEvans]] — co-conspirator on the observation; quoted directly.
- [[AnemicDomainModel]] — the anti-pattern this article names.
- [[DomainModel]] — what the anti-pattern fails to be.
- [[DomainDrivenDesign]] — the broader school this argument supports.

## Contradictions
- None — directly reinforced by [[introduction-to-domain-driven-design]]'s "entities as units of behavior" advice and by Evans's emphasis on [[ModelDrivenDesign]] in [[ddd-reference-2015]].
