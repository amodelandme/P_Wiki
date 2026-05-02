---
title: "Liskov Substitution Principle"
type: concept
tags: [oop, solid, subtyping]
sources: [liskov-italicized-paragraph-explained]
last_updated: 2026-05-02
---

# Liskov Substitution Principle (LSP)

Formulated by [[BarbaraLiskov]] (1987 keynote, 1994 Liskov–Wing paper) and popularized as the **L** in SOLID by [[RobertCMartin]]. Plain-English form:

> *If `S` is a subtype of `T`, anywhere a `T` is expected an `S` should be substitutable without the program noticing.*

## What "subtype" really means here
LSP says inheritance is a *contract promise*, not just an "is-a" relationship. The subtype must honor every behavioral expectation the base type made — preconditions, postconditions, invariants. Biological "is-a" intuitions (a penguin *is* a bird) routinely lead to LSP violations (penguins can't `Fly()`).

## Canonical violations
- `class Penguin : Bird` overriding `Fly()` to throw `NotImplementedException` — the exception itself is the smell.
- `class Square : Rectangle` collapsing width and height: `shape.Width = 5; shape.Height = 10; shape.Area()` returns 100, not 50. The caller was *surprised*; that surprise is the violation.

## Direction matters
Common mix-up: in the rectangle/square example, **`Square` is not a proper subtype of `Rectangle`**, not the other way around. The base contract (`Rectangle`) promises width and height are independent; `Square` cannot honor that, so the inheritance arrow points the wrong way.

## Connections
- [[BarbaraLiskov]] — namesake.
- [[RobertCMartin]] — popularized as part of SOLID.
- [[liskov-italicized-paragraph-explained]] — primary source for this wiki.
