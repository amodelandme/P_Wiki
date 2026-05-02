---
title: "Model-Driven Design"
type: concept
tags: [ddd, modeling]
sources: [ddd-reference-2015, introduction-to-domain-driven-design]
last_updated: 2026-05-02
---

# Model-Driven Design

The practice of binding code so tightly to the [[DomainModel]] that **a change to the code is a change to the model, and vice versa**. No analysis-vs-design split, no parallel UML universe — the code is the canonical expression of the model.

Per [[EricEvans]]: *"Design a portion of the software system to reflect the domain model in a very literal way, so that mapping is obvious."*

## Implications
- Class names, method names, and module structure must come from the [[UbiquitousLanguage]].
- Refactoring is motivated as often by domain insight as by technical concerns ("Refactoring Toward Deeper Insight").
- Modelers and coders cannot be separate roles — see Hands-on Modelers.

## Connections
- [[DomainModel]] — the thing being expressed.
- [[UbiquitousLanguage]] — the words the code uses.
- [[AnemicDomainModel]] — what happens when only the *shape* of the model is in code, not the *behavior*.
- [[ddd-reference-2015]] — canonical definition.
