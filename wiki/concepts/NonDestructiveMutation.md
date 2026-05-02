---
title: "Non-destructive Mutation"
type: concept
tags: [immutability, functional, csharp]
sources: [using-record-types-in-cs]
last_updated: 2026-05-02
---

# Non-destructive Mutation

The pattern of producing a "modified" version of an immutable object by constructing a new instance that copies the original's state and overrides only the changed fields. The original instance remains untouched, so both versions can coexist — a cornerstone of functional and immutable-by-default designs.

In [[CSharp]], the `with` expression on [[CSharpRecords]] (and structs, since C# 10) gives this first-class syntax:

```csharp
Person jim = joe with { Name = "Jim" };
```

The compiler emits a copy that carries every property over and reassigns the listed ones via their `init` setters.

## Why it matters
- Enables immutable data without write-sites having to manually thread every unchanged field.
- Forward-compatible: when a new property is added to the record, every existing `with` expression automatically copies it without code changes.

## Connections
- [[CSharpRecords]] — the language feature `with` is built around.
- [[ValueObject]] — a common context for non-destructive updates.
- [[using-record-types-in-cs]] — primary source.
