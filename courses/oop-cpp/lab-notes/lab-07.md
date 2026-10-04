# Lab Notes 7 — Inheritance II (Overriding & Multiple Inheritance)

**Concept recap:** overriding redefines a base method with a matching signature; `override`
catches signature mismatches at compile time; name hiding hides *all* base overloads of a name,
not just the matching one; multiple inheritance can combine unrelated capabilities but risks the
diamond problem when bases share an ancestor.

**Common pitfalls:**
- Expecting `Dog::greet()` to leave `Animal::greet(int)` reachable without qualification — name
  hiding blocks it; this is a common and genuinely surprising trap the first time it's seen.
- Adding `override` to a function that isn't actually virtual yet (Week 8) — in this lab (before
  `virtual` is introduced), `override` on a non-virtual function is a compile error; only use it
  once the base method is `virtual`.
- Designing `Flyable`/`Swimmable` with a shared base "to reduce duplication," accidentally
  recreating the diamond problem the lab is specifically designed to avoid.
- Forgetting that multiple inheritance syntax is `class Duck : public Flyable, public Swimmable`
  (comma-separated), not two separate `:` clauses.

**Debugging tip:** a "request for member X is ambiguous" compiler error when calling a method
through a multiply-inherited class is the signature of a diamond problem — check whether two of
the class's base classes share a common ancestor.

**Instructor tip:** have students attempt Task D's diamond scenario in code briefly (even though
the task asks only for a comment) just to see the ambiguous-member compiler error firsthand,
before reverting to the comment-only answer.
