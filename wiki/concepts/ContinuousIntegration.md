---
title: "Continuous Integration"
type: concept
tags: [ddd, process]
sources: [ddd-reference-2015]
last_updated: 2026-05-02
---

# Continuous Integration

In DDD, Continuous Integration applies to the **model**, not just the codebase. Once a [[BoundedContext]] is defined, even small teams (3–4 people) tend to drift the model in different directions. CI is the practice of merging code and ideas frequently enough that fragmentation is caught before it solidifies.

Per [[EricEvans]]: *"Institute a process of merging all code and other implementation artifacts frequently, with automated tests to flag fragmentation quickly. Relentlessly exercise the [[UbiquitousLanguage]] to hammer out a shared view of the model as the concepts evolve in different people's heads."*

## Connections
- [[BoundedContext]] — what CI keeps coherent.
- [[UbiquitousLanguage]] — the medium through which model coherence is negotiated.
- [[ddd-reference-2015]] — canonical definition.
