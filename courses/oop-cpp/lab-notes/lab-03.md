# Lab Notes 3 — Operator Overloading I

**Concept recap:** overloaded arithmetic operators as member functions take the left-hand operand
as the implicit object and the right-hand operand as the parameter; `operator+`/`operator-`
should return a new object and leave both operands unchanged; `operator+=` is the one that
mutates `*this`.

**Common pitfalls:**
- Mutating `*this` inside `operator+`, which breaks code that expects `a + b` to leave `a` and
  `b` unchanged (a correctness bug, not just a style issue).
- Forgetting `const` on `operator+`/`operator==`/`operator<`, which then can't be called on
  `const Fraction` objects or through `const` references.
- Defining `operator==` and `operator<` inconsistently (e.g. comparing numerators directly without
  cross-multiplying denominators), which silently breaks sorting/searching on your type later in
  the course.
- Infinite recursion: implementing `operator+` by calling itself instead of delegating to
  `operator+=` or building the result directly — easy to introduce by copy-pasting the wrong
  operator's body.

**Debugging tip:** if a program using your operator seems to hang or crash with a stack overflow,
check first whether one overloaded operator accidentally calls itself (directly or through another
overload) instead of doing actual work.

**Instructor tip:** show `1/2 == 2/4` evaluating `true` with the cross-multiplication
implementation, and ask students to predict what would happen (incorrectly) if `operator==`
instead compared `num_` and `den_` directly without cross-multiplying.
