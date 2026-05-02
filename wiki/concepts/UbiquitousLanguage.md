---
title: "Ubiquitous Language"
type: concept
tags: [ddd, communication]
sources: [ddd-reference-2015, introduction-to-domain-driven-design, ddd-guide-to-scalable-systems]
last_updated: 2026-05-02
---

# Ubiquitous Language

Per [[EricEvans]]: *"A language structured around the domain model and used by all team members within a [[BoundedContext|bounded context]] to connect all the activities of the team with the software."*

The same vocabulary appears in domain-expert conversations, requirements, code (class names, methods), tests, and diagrams. A change in the language **is** a change to the model.

## Crucial caveat
**Each [[BoundedContext]] owns its own ubiquitous language.** "Policy" in claims auditing and "Policy" in core workflow are *different things* even if they share an identifier — they have different invariants, different lifecycles, and different responsibilities.

## Connections
- [[BoundedContext]] — the scope inside which a ubiquitous language applies.
- [[ModelDrivenDesign]] — code expresses the language; refactoring the language refactors the code.
- [[DomainDrivenDesign]] — one of the three foundational principles.
- [[ddd-reference-2015]] — canonical definition.
