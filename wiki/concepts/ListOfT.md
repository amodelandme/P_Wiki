---
title: "List<T>"
type: concept
tags: [dotnet, csharp, collections, data-structures]
sources: [what-makes-list-so-efficient-in-dotnet]
last_updated: 2026-05-02
---

# List<T>

`List<T>` is the most widely used generic collection in [[DotNet]]. It provides ordered, indexed, dynamically-resizable storage for elements of type `T`.

## Working Principle
Internally, `List<T>` is backed by a plain array. When `Add` is called and the underlying array is full, the list:
1. Allocates a new array of **double** the current capacity.
2. Copies all existing elements via `Array.Copy`.
3. Replaces the internal reference with the new array.

This makes it a textbook implementation of a [[DynamicArray]].

## Performance
By [[AmortizedAnalysis]], populating a list of final size `2^n` requires roughly `2 · 2^n` memory writes — i.e. **each element is written ~2 times on average**. Add is amortized O(1), though individual calls that trigger a resize are O(n).

This means `List<T>` is roughly 2× more expensive in writes than a fixed-size array, but offers unbounded growth in exchange.

## Connections
- [[DynamicArray]] — the underlying pattern.
- [[AmortizedAnalysis]] — the proof of its efficiency.
- [[DotNet]] — the runtime that ships it.
- [[ZoranHorvat]] — author of the explanatory article.
