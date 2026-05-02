---
title: "C# Records"
type: concept
tags: [csharp, language-feature, immutability]
sources: [using-record-types-in-cs]
last_updated: 2026-05-02
---

# C# Records

A [[CSharp]] language feature introduced in C# 9 (reference type) and extended in C# 10 (`record struct` value type). Declaring `public record Person(string Name, DateOnly BirthDate);` causes the compiler to generate a full immutable class: a positional constructor, `init`-only public properties, a `Deconstruct` method, `Equals`/`GetHashCode` implementing [[ValueTypedEquality]], an `IEquatable<T>` implementation, and a debug-friendly `ToString`.

## Why they exist
Records collapse ~20 lines of boilerplate value-class code into one keyword, making it cheap to model immutable data — particularly [[ValueObject]]s in DDD.

## Notable mechanics
- **Init-only properties** — assignable only at construction; the instance is immutable thereafter.
- **[[NonDestructiveMutation]]** via the `with` expression: `var jim = joe with { Name = "Jim" };` copies all properties and overrides the listed ones.
- **Custom members** — records accept a body just like classes, so methods, factory functions, and computed properties can live alongside the positional declaration.
- **Vertical compatibility risk** — adding a positional component breaks every direct constructor call. A static `Create` factory introduced from day one absorbs that change.

## When not to use
Per [[ZoranHorvat]]: not for types that need injected dependencies, services, or mutable state.

## Connections
- [[CSharp]] — host language.
- [[DotNet]] — runtime; records require .NET 5+, with `DateOnly` and record structs in .NET 6.
- [[NonDestructiveMutation]] — the `with` keyword feature is enabled by record's compiler-generated copy constructor.
- [[ValueTypedEquality]] — auto-generated for records.
- [[ValueObject]] — natural target use case.
- [[using-record-types-in-cs]] — primary source.
