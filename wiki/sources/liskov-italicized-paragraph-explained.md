---
title: "Barbara Liskov's italicized paragraph explained"
type: source
tags: [oop, lsp, csharp, conversation]
date: 2026-05-02
source_file: raw/Barbara Liskov's italicized paragraph explained.md
---

## Summary
A Claude conversation decoding the formal italicized statement of the [[LiskovSubstitutionPrinciple]] from Robert C. Martin's *Clean Architecture*. The exchange translates Liskov's math-proof-style definition into plain English, walks through the canonical penguin/`Bird.Fly()` and `Square : Rectangle` LSP-violation examples in [[CSharp]], and emphasizes that an inheritance arrow is a *contract promise* — not just an "is-a" relation.

## Key Claims
- LSP plain-English form: if `S` is a subtype of `T`, anywhere a `T` is expected an `S` should be substitutable without the program noticing.
- A `NotImplementedException` thrown from an overridden method is a strong LSP-violation smell — it signals the subtype refuses to honor the base contract.
- The classic `Square : Rectangle` example violates LSP because `Rectangle` promises width and height are independent; `Square` collapses them, surprising callers (`shape.Area()` returns 100 instead of 50).
- The violation is "the program was surprised" — the experiential definition of a broken substitution contract.
- Direction matters: in the canonical example it is `Square` that is not a proper subtype of `Rectangle`, not the reverse — a subtle but common mix-up.

## Key Quotes
> "If you have two types, S and T, and S is supposed to be a subtype of T — then anywhere you use a T, you should be able to swap in an S, and the program shouldn't care."

> "LSP says: the arrow must be a promise, not just an 'is-a' relationship."

## Connections
- [[BarbaraLiskov]] — namesake of the principle.
- [[RobertCMartin]] — author of *Clean Architecture*, the book context for this conversation.
- [[LiskovSubstitutionPrinciple]] — the principle being explained.
- [[CSharp]] — language of the example code.

## Contradictions
- None with existing wiki content.
