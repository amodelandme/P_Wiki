---
title: "Big Ball of Mud"
type: concept
tags: [ddd, antipattern, architecture]
sources: [ddd-reference-2015, introduction-to-domain-driven-design]
last_updated: 2026-05-02
---

# Big Ball of Mud

A system with no discernible architecture — sprawling, expedient, tightly coupled. Coined by Brian Foote and Joseph Yoder in their 1999 paper of the same name. Cataloged in [[ddd-reference-2015]] as a relationship style on a [[ContextMap]] — sometimes the right move is to *recognize* a Big Ball of Mud, fence it off, and refuse to model inside it.

## How it forms
- No explicit [[BoundedContext|bounded contexts]] — every team adds to the same shared model.
- One overloaded class (`Policy`, `User`, `Order`) accretes contradictory responsibilities until everything depends on everything.
- Behavior leaks across [[LayeredArchitecture|layers]]; UI code holds business rules, business rules embed SQL.

## How DDD pushes back
- Identify and explicitly bound contexts.
- Use an [[AnticorruptionLayer]] when integrating with a Big Ball of Mud you can't fix.
- Keep the [[CoreDomain]] out of the mud at all costs.

## Connections
- [[BoundedContext]] / [[ContextMap]] — the antidotes.
- [[AnticorruptionLayer]] — the protective wrapper.
- [[ddd-reference-2015]] — canonical entry.
