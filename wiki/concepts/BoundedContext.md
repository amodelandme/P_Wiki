---
title: "Bounded Context"
type: concept
tags: [ddd, strategic-design]
sources: [ddd-reference-2015, introduction-to-domain-driven-design, ddd-guide-to-scalable-systems]
last_updated: 2026-05-02
---

# Bounded Context

Per [[EricEvans]]: *"A description of a boundary (typically a subsystem, or the work of a particular team) within which a particular model is defined and applicable."*

Multiple models always exist on a large project. Pretending otherwise produces buggy, ambiguous code. A bounded context **explicitly** sets where a model applies — by team, by application area, and by physical artifacts (code bases, schemas).

Within the context: a single [[UbiquitousLanguage]], standardized process, [[ContinuousIntegration]] to prevent drift. Across contexts: translation via a [[ContextMap]], possibly with an [[AnticorruptionLayer]].

## Why it matters
Without explicit bounded contexts, codebases drift toward [[BigBallOfMud]] — a single overloaded "Policy" or "User" object accreting contradictory responsibilities from every team that touches it.

## Connections
- [[ContextMap]] — how multiple bounded contexts relate.
- [[UbiquitousLanguage]] — each context owns its own.
- [[AnticorruptionLayer]] — protects a context from a foreign model.
- [[ddd-reference-2015]] — canonical definition.
