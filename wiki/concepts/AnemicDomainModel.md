---
title: "Anemic Domain Model"
type: concept
tags: [ddd, antipattern, oop]
sources: [anemic-domain-model, introduction-to-domain-driven-design, ddd-reference-2015]
last_updated: 2026-05-02
---

# Anemic Domain Model

An anti-pattern named by [[MartinFowler]] (with [[EricEvans]] in agreement): a domain that *looks* object-oriented — many classes named after domain nouns, rich relationships — but whose objects are bags of getters and setters with no behavior. All logic lives in service classes that operate on the data.

## Why it's a problem
- It's procedural design wearing OO clothing — opposite of "combine data and process together."
- You pay all the costs of a [[DomainModel]] (especially O/R mapping complexity) and none of its benefits.
- A well-designed Transaction Script (Fowler, *PoEAA*) would have been simpler and more honest.

## Common causes
- Developers from data-centric backgrounds.
- Frameworks that nudge in this direction (Fowler called out J2EE Entity Beans; analogous pressures exist in any framework that emphasizes DTOs over behavior).
- Fear of putting dependencies in entities (see [[introduction-to-domain-driven-design]] for Laribee's method-injection counter-pattern).

## How to push back
- Treat [[Entity|entities]] as units of *behavior*, not data.
- Keep the [[DomainService]] layer thin; reach for a service only when no entity is a natural home.
- Watch for the "service that takes an entity and acts on it" smell — usually the operation belongs *on* the entity.

## Connections
- [[MartinFowler]] / [[EricEvans]] — co-observers.
- [[DomainModel]] — what an anemic model fails to be.
- [[DomainService]] — the layer that absorbs logic when this anti-pattern takes hold.
- [[anemic-domain-model]] — primary source.
