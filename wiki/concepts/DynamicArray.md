---
title: "Dynamic Array"
type: concept
tags: [data-structures, algorithms]
sources: [what-makes-list-so-efficient-in-dotnet]
last_updated: 2026-05-02
---

# Dynamic Array

A **dynamic array** (a.k.a. self-expanding array, growable array) is a data structure that wraps a fixed-size array and transparently resizes it when capacity is exceeded. The canonical strategy is **capacity doubling**: when full, allocate a new buffer of 2× the current size and copy existing elements over.

## Why Doubling Works
Doubling — rather than incrementing by a constant — is what makes the structure efficient. Under [[AmortizedAnalysis]], the geometric growth ensures that, across all insertions, each element is copied a bounded number of times (roughly twice on average). Linear growth would degrade to O(n) per insertion in aggregate.

## Real-World Implementations
- [[ListOfT]] in [[DotNet]]
- `std::vector` in C++
- `ArrayList` in Java
- Python's `list`

## Connections
- [[ListOfT]] — concrete .NET implementation.
- [[AmortizedAnalysis]] — proof technique for its O(1) amortized append.
