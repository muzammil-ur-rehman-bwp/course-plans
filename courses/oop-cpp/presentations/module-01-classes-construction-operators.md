# Presentation: Module 1 — Classes, Construction & Operator Overloading (Weeks 1–4)

**Format:** Slide deck outline for lecture delivery (convert to slides in institution's template).

1. **Title slide** — Object Oriented Programming (C++): Module 1, Foundations Revisited
2. **From CS1 to OOP** — recap of the prerequisite course's "intro to classes" week, and what
   this course adds starting today
3. **Access specifiers** — `public`/`private`/`protected` side-by-side, with the `protected`
   teaser for inheritance
4. **`const`-correctness** — code snippet slide: a `const` getter and why it matters
5. **Member initializer lists** — diagram: initializer list vs. assignment-in-body, and why
   `const`/reference members require the former
6. **The Rule of Three** — diagram: destructor + copy constructor + copy-assignment, and the
   shallow-copy double-free failure case
7. **Operator overloading as member functions** — code snippet: `Fraction::operator+`
8. **Why `operator<<` can't be a member** — diagram: left-hand operand must be the stream
9. **`friend`** — what it grants, and why it's used narrowly
10. **Looking ahead** — "Next: composition and inheritance — has-a vs. is-a" teaser

**Speaker notes:** open by explicitly connecting to the prerequisite course's Week 13 (which
stopped at private data + a constructor) — this module is where that preview becomes the real
thing. End by planting "has-a vs. is-a" as the question Module 2 answers.
