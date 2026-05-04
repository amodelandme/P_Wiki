Reading through Martin's Clean Architecture 

Good timing — LSP trips up a LOT of developers, even experienced ones. Before I throw a wall of explanation at you, let me probe a little so I can meet you exactly where you're at.
What does Liskov Substitution Principle mean to you right now? Even a rough, imperfect definition — just tell me what's in your head when you hear it.
And a follow-up: where specifically is it breaking down for you? Is it:
The definition itself feels abstract/confusing?
You think you get it, but you can't spot violations in real code?
You understand the violation, but you're not sure why it matters in practice?
Something else entirely?
Don't worry about sounding wrong — I'd rather know your actual mental model than have you guess what I want to hear. That's how we actually fix the gap. 🎯

I'm struggling with understanding how Martin explains it in Clean Architecture

All I can really say is that developers like to be able to substitute components in code but to be careful before you crowd yourself out

Ha! "Crowd yourself out" — I love that your brain tried to make sense of it that way. There's actually a kernel of truth in there, which tells me something landed. But yeah, Martin's framing is dense. Let's rebuild this from scratch in plain English.
Forget Martin for a second. Let me ask you this:
You're at a job site. You hire a contractor. The contractor says:
"I follow standard building codes. Any licensed electrician can come behind me and understand my work."
Now imagine a different contractor who wires things in their own weird proprietary way — nothing standard, nothing predictable. The next electrician comes in and has no idea what's going on. Things break. Walls have to be torn open.
LSP is basically: if you say something is a type of thing, it needs to actually behave like that thing — not just look like it on paper.
Here's the classic .NET framing:
Ostrich inherits from Bird and gets a Fly() method — but ostriches can't fly. So if your code does this:
Csharp
The caller had a reasonable expectation — "I have a Bird, I can call Fly" — and got betrayed. That's the violation.
Now here's the question that connects this to what Martin is actually saying:
Why do you think this matters specifically for the caller of the code — not the thing being called?
Take a stab at it. I want to see if that framing clicks before we go deeper. 🎯

So I just reread the principle itself in light of what you said. The principle says that P should not have to modify its behavior if you substitute a subtype that TRULY, REALLY a subtype. Changes are required to accommodate a "subtype " that is a phoney 😆

YES. 🎉
That's it. That's actually it. And the way you said it is better than how most textbooks explain it.
"A phoney subtype" — write that down because that's your mental model now.
Let me just sharpen it one notch:
If P has to check what kind of thing it received before it knows how to behave — you've got a phoney.
Like this:
Csharp
See that if (bird is Ostrich)? That's the smoking gun. P had to modify its behavior. LSP violated.
Now here's where Martin's actual point connects — and why he puts LSP in an architecture book, not just an OOP book.
He's not just talking about classes. He's talking about interfaces, APIs, microservices, anything substitutable.
Imagine two payment processors — Stripe and PayPal — both implementing IPaymentProcessor. If your checkout code has to special-case PayPal because it behaves slightly differently... same violation, bigger scale.
So here's my question to push you to the next level:
How would you fix the Bird/Ostrich problem? Don't just say "remove Fly from Ostrich" — think about what the inheritance tree should actually look like. 🤔

This is some complexity that can be addressed by separating or decoupling bird behaviors from bird characteristics.

Absolutely nailed it.
That's the insight. You just independently arrived at interface segregation as the solution to an LSP violation. That's not a coincidence — the SOLID principles talk to each other.
Let me draw what your brain just described:
Let me sketch it out in code instead — equally clear:
Csharp
Now your caller looks like this:
Csharp
No phoneys can get in. The type system itself enforces the contract. 🎯
Here's what I want you to notice about what just happened in this conversation:
You started with "crowd yourself out" and within one session you've got a mental model sharp enough to independently derive interface segregation from first principles.
That's not memorizing SOLID. That's understanding it.
One final question to cement this — and this one's interview-grade 👇
Can you think of a real example from your own past work, or from your Feature Flag API project, where you might have accidentally created a phoney subtype?