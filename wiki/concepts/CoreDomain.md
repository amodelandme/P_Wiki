---
title: "Core Domain"
type: concept
tags: [ddd, strategic-design]
sources: [ddd-reference-2015, introduction-to-domain-driven-design]
last_updated: 2026-05-02
---

# Core Domain

The subset of a system's domain that is most distinctive and most valuable — the part where the business has a competitive edge and where custom modeling effort pays off.

Strategic implication: **invest disproportionate intellectual capital in the core domain**. Generic subdomains (auth, billing, general ledger) usually warrant off-the-shelf or boring-but-reliable solutions. The core is where DDD's heavy machinery — careful [[ModelDrivenDesign]], deep [[UbiquitousLanguage]] cultivation, refactoring toward deeper insight — earns its keep.

## Connections
- [[DomainDrivenDesign]] — focusing on the core is one of the three foundational principles.
- [[BoundedContext]] — the core domain typically lives in one (or a small number of) bounded context(s).
- [[ddd-reference-2015]] — canonical definition.
