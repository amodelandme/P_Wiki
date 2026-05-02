---
title: "Single Responsibility Principle"
type: concept
tags: [oop, solid]
sources: [introduction-to-domain-driven-design]
last_updated: 2026-05-02
---

# Single Responsibility Principle (SRP)

The **S** in SOLID, formulated by [[RobertCMartin]]. A class or module should have one reason to change — i.e., it should serve a single actor or responsibility.

In DDD, the building-block patterns naturally push toward SRP: an [[Entity]] owns identity and lifecycle, a [[ValueObject]] describes attributes, a [[Repository]] handles persistence of one [[Aggregate]] root, a [[DomainService]] captures one cross-cutting operation. When responsibilities blur, you usually drift toward [[AnemicDomainModel]] or [[BigBallOfMud]].

## Connections
- [[RobertCMartin]] — popularizer.
- [[DomainDrivenDesign]] — the DDD pattern catalog operationalizes SRP.
- [[introduction-to-domain-driven-design]] — Laribee's piece highlights SRP as a free benefit of following DDD patterns.
