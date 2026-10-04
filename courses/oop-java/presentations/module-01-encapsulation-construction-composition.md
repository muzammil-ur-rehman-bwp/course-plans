# Presentation: Module 1 — Encapsulation, Construction & Composition (Weeks 1–4)

**Format:** Slide deck outline for lecture delivery (convert to slides in institution's template).

1. **Title slide** — Object Oriented Programming (Java): Module 1, Foundations Revisited
2. **From CS1 to OOP** — recap of the prerequisite course's "intro to classes" week, and what
   this course adds starting today
3. **Encapsulation** — private fields vs. public interface, getters/setters, `this`
4. **Immutability** — code snippet slide: a `final`-fields-only class with no setters
5. **Constructor chaining** — diagram: `this(...)` chaining across overloaded constructors
6. **`equals`/`hashCode`/`toString`** — code snippet: the `equals(MyClass)` overload bug vs. a
   correct `equals(Object)` override, side by side
7. **No destructors** — one-slide contrast with a C++ Rule of Three; garbage collection instead
8. **Composition** — diagram: `Car` has-a `Engine`, with delegation
9. **Static vs. instance** — side-by-side diagram: one copy per class vs. one copy per object
10. **Looking ahead** — "Next: inheritance — is-a, extends, and super(...)" teaser

**Speaker notes:** open by explicitly connecting to the prerequisite course's Week 13 (which
stopped at private fields + a constructor + static recap) — this module is where that preview
becomes the real thing. End by planting "is-a vs. has-a" as the question Module 2 answers in
depth.
