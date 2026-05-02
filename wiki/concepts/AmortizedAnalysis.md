---
title: "Amortized Analysis"
type: concept
tags: [algorithms, complexity, analysis]
sources: [what-makes-list-so-efficient-in-dotnet]
last_updated: 2026-05-02
---

# Amortized Analysis

A technique for analyzing the cost of a sequence of operations on a data structure, where individual operations may be expensive but their **average cost over the sequence** is small. It justifies treating operations like [[DynamicArray]] append as O(1) even though some calls trigger O(n) reallocations.

## Application: Capacity-Doubling Arrays
For a [[DynamicArray]] that doubles when full, growing to size `2^n` requires copying `1 + 2 + 4 + … + 2^(n-1) = 2^n − 1` elements across all reallocations, plus `2^n` writes for the inserts themselves. Total writes ≈ `2 · 2^n`, so **each element is written ~2 times on average**.

## Connections
- [[DynamicArray]] — the canonical example.
- [[ListOfT]] — the .NET instance whose performance this technique proves.
