# Lab Notes 5 — Inheritance I

**Concept recap:** `extends` gives a subclass every non-private field and method of its
superclass. `protected` is visible to the superclass and its subclasses, but not to outside code;
`private` stays hidden even from a subclass. A subclass constructor must invoke a superclass
constructor via `super(args)` as its first statement, or implicitly via the superclass's
no-argument constructor.

**Common pitfalls:**
- Forgetting `super(args)` when the superclass has no no-argument constructor — a compile error
  that confuses students expecting a runtime problem instead.
- Putting `super(args)` anywhere but the first statement — also a compile error.
- Trying to access a `private` superclass field directly from a subclass — remember that only
  `protected`/`public` members cross the inheritance boundary.
- Assuming a subclass automatically gets a matching constructor — constructors are not
  inherited; each subclass must declare its own.

**Debugging tip:** "constructor Vehicle() undefined" from the compiler on a subclass almost
always means an implicit `super()` call was inserted because `super(args)` was omitted, and
`Vehicle` has no matching no-argument constructor.

**Instructor tip:** show the single-inheritance restriction directly (`class C extends A, B` as
an immediate compile error) to make Week 5's "no multiple class inheritance" point concrete,
ahead of the interfaces story in Week 9.
