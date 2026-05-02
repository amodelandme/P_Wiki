---
title: "What Makes List<T> So Efficient in .NET?"
type: source
tags: [dotnet, csharp, collections, data-structures, performance]
date: 2022-09-07
source_file: raw/What Makes List_T_ So Efficient in .NET_.md
---

## Summary
Zoran Horvat explains why .NET's `List<T>` is so widely used despite being backed by an array. The article reconstructs the underlying mechanism — a self-expanding array that doubles its capacity when full — and proves via amortized analysis that, on average, each element is written to memory only twice. This makes `List<T>` nearly as fast as a raw array while removing the fixed-size limitation.

## Key Claims
- `List<T>` is internally backed by a plain array; flexibility comes from automatic resizing, not a different data structure.
- When capacity is exhausted, `List<T>` allocates a new array of double the size, copies existing items over, and replaces the reference.
- Total writes to populate a list of size 2^n are bounded such that **each element is written ~2 times on average** (amortized cost).
- The cost of `List<T>` versus a raw array is therefore a 2× write multiplier — a small price for unlimited growth.
- Add operations are amortized O(1), but individual `Add` calls can incur intermittent delays when reallocation triggers.
- A naïve fixed-size array implementation throws `IndexOutOfRangeException` once filled; doubling the buffer is the standard fix.

## Key Quotes
> "Every location will be written two times on average." — closing of the amortized-cost analysis
> "The resizable list makes twice as many writes as the array of the same final size. That is the cost of using the List<T> versus the cost of using the plain array." — summary of the tradeoff

## Connections
- [[ZoranHorvat]] — author of the article and the related Pluralsight course *Collections and Generics in C#*.
- [[ListOfT]] — the .NET generic collection that is the article's subject.
- [[DynamicArray]] — the general data-structure pattern (capacity-doubling array) that `List<T>` implements.
- [[AmortizedAnalysis]] — the technique used to prove the 2× write bound.
- [[DotNet]] — the platform/runtime context for the discussion.

## Contradictions
None — this is the wiki's first source.
