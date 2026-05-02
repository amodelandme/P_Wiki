---
title: "Context Map"
type: concept
tags: [ddd, strategic-design]
sources: [ddd-reference-2015, introduction-to-domain-driven-design]
last_updated: 2026-05-02
---

# Context Map

A diagram (or document) showing the [[BoundedContext|bounded contexts]] in a system and the relationships between them — Shared Kernel, Customer/Supplier, Conformist, [[AnticorruptionLayer]], Open-Host Service, Published Language, Separate Ways, Partnership, [[BigBallOfMud]].

## Why
Without a context map, integration between teams is ad-hoc, terms drift between contexts, and "Big Ball of Mud" subsystems quietly leak into clean ones. The map makes the political and technical relationships explicit so they can be designed instead of inherited.

## Connections
- [[BoundedContext]] — the nodes on the map.
- [[AnticorruptionLayer]] — one of several inter-context relationship patterns.
- [[BigBallOfMud]] — recognized as a *kind* of relationship (or warning) on the map.
- [[ddd-reference-2015]] — canonical pattern catalog.
