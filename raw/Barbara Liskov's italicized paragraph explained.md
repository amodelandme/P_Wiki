---
title: "Barbara Liskov's italicized paragraph explained"
source: "https://claude.ai/chat/274393f5-0770-4039-9b70-18fb904df4e9"
author:
published:
created: 2026-05-02
description: "Claude conversation with 6 messages"
tags:
  - "clippings"
---
**You**

Explain the paragraph in italics by Barbara liskov

---

## Breaking Down Liskov's Brain-Melting Definition 🧠

That italic paragraph is written like a math proof, which is why it reads like alphabet soup. Let's decode it.

### What She Actually Said (in human)

> "If you have two types, S and T, and S is supposed to be a subtype of T — then anywhere you use a T, you should be able to swap in an S, and the program shouldn't care."

That's it. That's the whole thing.

### A Concrete Example

Think of it like this:

```
T = Bird       (the parent/base)
S = Penguin    (the subtype)
P = a program that calls bird.Fly()
```

If your program expects a `Bird` and you sneak in a `Penguin`, does it break? **YES** — because penguins can't fly.

That's an **LSP violation**. `Penguin` is *not* a proper subtype of `Bird`, even though biologically it is one. 🐧

### The Mental Model

```
Parent type (T)
      │
      ▼
  Subtype (S)
```

LSP says: **the arrow must be a promise**, not just an "is-a" relationship. The subtype must *honor the contract* of the parent — same behavior, same expectations.

### The Real-World C# Tell

```csharp
// 🚨 LSP Violation
class Penguin : Bird {
    public override void Fly() => throw new NotImplementedException();
}
```

That `NotImplementedException` is a giant red flag 🚩 — you're breaking the promise the base class made.

---

What book is that? Looks like Uncle Bob's *Clean Architecture*?

---

**You**

Yeah. It's Clean Architecture. Just finished the chapter on LSP.

---

**Claude**

Nice! Uncle Bob is a rite of passage. 🎩

So since it's fresh — let me probe you a little. Don't worry, this is low stakes, just me seeing where your head's at.

Here's a scenario:

---

```csharp
public class Rectangle
{
    public virtual int Width { get; set; }
    public virtual int Height { get; set; }

    public int Area() => Width * Height;
}

public class Square : Rectangle
{
    public override int Width
    {
        set { base.Width = value; base.Height = value; }
    }

    public override int Height
    {
        set { base.Width = value; base.Height = value; }
    }
}
```

Then somewhere in your app:

```csharp
Rectangle shape = new Square();
shape.Width = 5;
shape.Height = 10;
Console.WriteLine(shape.Area()); // What prints? And is this an LSP violation?
```

**Two questions:**

1. What does `shape.Area()` print?
2. Is this an LSP violation — and *why or why not*?

Take your time. Think it through out loud if you want, that's actually the best way to work through it. 🧠

---

**You**

1. an integer representing the area of a rectangle. But it's instantiating a "Square". Makes no sense.
2. Yes, because a rectangle is not a proper subtype of Square. Square sides all change together while a rectangle's sides, by definition, can change independently.

---

**Claude**

## Solid! You've Got the Core Idea 🎯

### On Question 1 — Let's Be Precise

It doesn't just print "an integer" — it prints **100**.

Here's why:

```csharp
Rectangle shape = new Square();
shape.Width = 5;    // Square sets BOTH width AND height to 5
shape.Height = 10;  // Square sets BOTH width AND height to 10

// At this point: width = 10, height = 10
shape.Area() // 10 * 10 = 100 😵
```

You expected 50 (5 × 10). You got 100. **The program was surprised.** That surprise *is* the violation.

### On Question 2 — Small Flip!

You've got the concept dead right, but the direction is backwards. It's:

> ❌ **Square** is not a proper subtype of **Rectangle**

`Square` inherits from `Rectangle` in this code — that's the problematic relationship. A Square can't honor the Rectangle contract because rectangles allow independent side changes. Squares don't.

### The Golden Rule You Just Described

```
Rectangle contract says:
"I promise Width and Height are independent."

Square says:
"Lol, no they're not."

LSP: 🚨 Contract broken.
```

---

You nailed the *why* though — that's honestly the harder part. Most people memorize the definition without understanding *what a broken contract actually feels like* at runtime.

What did Uncle Bob propose as the fix? Do you remember? 👀