# Presentation: Module 2 — Composition, Inheritance & Polymorphism I (Weeks 5–8)

**Format:** Slide deck outline for lecture delivery (convert to slides in institution's template).

1. **Title slide** — Object Oriented Programming (C++): Module 2, Relationships Between Classes
2. **"Has-a" vs. "is-a"** — side-by-side `Car`-has-`Engine` vs. `Dog`-is-`Animal` diagram
3. **Composition construction/destruction order** — diagram: member objects built before, torn
   down after, the enclosing constructor/destructor body
4. **Base/derived syntax** — code snippet: `class Derived : public Base`
5. **Constructor chaining** — diagram: derived constructor calling a specific base constructor
   via the initializer list
6. **Overriding & `override`** — code snippet, with a deliberate signature-mismatch typo caught
   by `override`
7. **Multiple inheritance & the diamond problem** — diagram: two bases sharing an ancestor,
   ambiguity highlighted
8. **Static vs. dynamic binding** — before/after slide: a call through `Animal*` without, then
   with, `virtual`
9. **Virtual destructors** — diagram: `delete` through a base pointer, leak without `virtual`,
   correct cleanup with it
10. **Object slicing** — code snippet: by-value `describe(Animal a)` losing `Dog`'s data
11. **Midterm review recap slide** — one-slide map of Weeks 1–8
12. **Looking ahead** — "Next: abstract classes, templates, exceptions" teaser

**Speaker notes:** Slides 8–10 (virtual functions, virtual destructors, slicing) are the
highest-stakes content in the course — spend disproportionate time here, and reuse the exact
code examples from `weekly-lecture-content/week-08.md` so students see identical code in lecture
and slides.
