---
title: "Value Object"
type: concept
tags: [ddd, design-pattern, immutability]
sources: [using-record-types-in-cs, ddd-reference-2015, introduction-to-domain-driven-design, ddd-guide-to-scalable-systems]
last_updated: 2026-05-02
---

# Value Object

A [[DomainDrivenDesign]] building block: a small, immutable object defined entirely by the values of its attributes rather than by an identity. Two value objects with the same fields are the same thing — money amounts, dates, addresses, coordinates, exposures.

Per [[EricEvans]]: *"When you care only about the attributes and logic of an element of the model, classify it as a value object. ... Treat the value object as immutable. Make all operations Side-effect-free Functions that don't depend on any mutable state. Don't give a value object any identity."*

## Defining traits
- **No identity** — equal by attributes, not by reference; contrast [[Entity]].
- **Immutability** — once constructed, never mutated; updates produce new instances ([[NonDestructiveMutation]]).
- **[[ValueTypedEquality]]** — equality based on contents.
- **Side-effect-free functions** — operations return new values; state is never observably mutated.
- **Replaceable / shareable** — safe to share across the model without aliasing concerns.
- **Self-validating** — typically reject invalid inputs at construction.

## Implementation in C#
[[CSharpRecords]] are the idiomatic vehicle: immutability, value equality, and a copy-with-mutation expression in a single keyword. Pre-records, the same effect was hand-rolled with `readonly` fields and explicit `Equals`/`GetHashCode` (see Laribee's `Money` example in [[introduction-to-domain-driven-design]]).

## Connections
- [[Entity]] — the contrast: identity-based instead of attribute-based.
- [[CSharpRecords]] — the modern C# implementation vehicle.
- [[ValueTypedEquality]] — required equality semantics.
- [[NonDestructiveMutation]] — the natural update pattern.
- [[ddd-reference-2015]] — canonical definition.
