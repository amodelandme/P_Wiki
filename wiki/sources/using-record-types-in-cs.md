---
title: "Using Record Types in C#"
type: source
tags: [csharp, dotnet, records, immutability, value-objects]
date: 2022-05-13
source_file: raw/Using Record Types in C#.md
---

## Summary
[[ZoranHorvat]] introduces C# 9 record types (and C# 10 record structs) as a compact way to declare immutable, value-equality classes. A single `record` keyword generates a constructor, init-only properties, `Equals`/`GetHashCode`, `Deconstruct`, and `ToString` — eliminating ~20 lines of boilerplate. The article also covers non-destructive mutation via the `with` expression, using static factory methods for vertical compatibility, and adding custom members.

## Key Claims
- A record is just a regular class; the compiler synthesizes its members.
- Records implement value-typed equality out of the box, so they work as keys in dictionaries, hash sets, `Distinct()`, and `GroupBy()`.
- Properties on a positional record are `init`-only — beyond construction the object is immutable.
- The `with` expression performs non-destructive mutation: it copies the source instance and overrides only the listed properties.
- Adding a new positional component breaks all direct constructor calls; using a static `Create` factory from the start protects against this.
- The `with` expression is forward-compatible — newly-added properties are copied automatically without touching call sites.
- Records are the right modeling choice for [[ValueObject]]s in DDD; they are the wrong choice when dependencies, services, or mutation are needed.
- C# 10 adds `record struct`, allowing records to be either reference or value types.

## Key Quotes
> "All that code you see above, over twenty lines of code, fits into a single keyword: record. That is the principal reason why you should use records."

> "Do not overdo that! If you need dependencies, services, let alone mutation, then the record is obviously not the right choice for you."

> "When modeling any type where value-typed equality is a natural attribute, or when you are modeling Value Objects in DDD terms for example, record is just for you."

## Connections
- [[ZoranHorvat]] — author.
- [[CSharp]] — language feature introduced in C# 9 / C# 10.
- [[DotNet]] — record types ship with .NET 5+ / .NET 6.
- [[CSharpRecords]] — primary concept this source defines.
- [[NonDestructiveMutation]] — the `with` expression pattern.
- [[ValueTypedEquality]] — equality semantics records provide by default.
- [[ValueObject]] — DDD modeling pattern records are well-suited for.

## Contradictions
- None with existing wiki content. Complements [[what-makes-list-so-efficient-in-dotnet]] as a second [[ZoranHorvat]] piece on .NET ergonomics, but at the language-feature rather than collection-internals level.
