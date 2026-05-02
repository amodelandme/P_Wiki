---
title: "Value-typed Equality"
type: concept
tags: [equality, semantics, csharp]
sources: [using-record-types-in-cs]
last_updated: 2026-05-02
---

# Value-typed Equality

Equality semantics where two objects are equal iff their fields/properties are equal, regardless of reference identity. Contrast with reference equality, where two distinct allocations are unequal even if they hold identical content.

In [[CSharp]], reference types default to reference equality. Implementing value equality manually means writing strongly- and weakly-typed `Equals` overrides plus `GetHashCode` and (idiomatically) `IEquatable<T>` — boilerplate that [[CSharpRecords]] generate for free.

## Why it matters
Value equality is what lets objects act as dictionary keys, deduplicate via `Distinct()`, and group via `GroupBy()` based on content rather than identity. Without it, those LINQ operators silently treat content-equal records as distinct.

## Connections
- [[CSharpRecords]] — auto-generates value equality.
- [[ValueObject]] — the DDD pattern that requires value equality by definition.
- [[using-record-types-in-cs]] — primary source.
