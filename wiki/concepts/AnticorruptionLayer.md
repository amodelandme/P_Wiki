---
title: "Anticorruption Layer"
type: concept
tags: [ddd, strategic-design, integration]
sources: [ddd-reference-2015, introduction-to-domain-driven-design]
last_updated: 2026-05-02
---

# Anticorruption Layer (ACL)

A translation layer between two [[BoundedContext|bounded contexts]] that prevents the foreign model from "corrupting" the local one. Inputs from the foreign system are translated into the local [[UbiquitousLanguage]]; outputs are translated back. The local model never sees the foreign vocabulary.

## Where you'll meet it
- Wrapping a legacy system behind a clean interface (Michael Feathers's "seam" pattern).
- Integrating with a third-party API that uses unrelated terminology.
- A [[Repository]] is itself a kind of ACL — keeping SQL/ORM concepts out of the [[DomainModel]].

## Connections
- [[BoundedContext]] — what an ACL protects.
- [[ContextMap]] — ACL is one relationship style on the map.
- [[Repository]] — special-case ACL between domain and persistence.
- [[ddd-reference-2015]] — canonical pattern.
