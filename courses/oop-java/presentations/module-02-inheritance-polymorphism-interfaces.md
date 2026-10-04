# Presentation: Module 2 — Inheritance, Polymorphism & Interfaces (Weeks 5–9)

**Format:** Slide deck outline for lecture delivery (convert to slides in institution's template).

1. **Title slide** — Object Oriented Programming (Java): Module 2, Relationships & Dynamic
   Behavior
2. **`extends` and access control** — `protected` revisited from the subclass's point of view
3. **Constructor chaining with `super(...)`** — diagram: a subclass constructor invoking its
   superclass constructor
4. **Single inheritance, no diamond problem** — why Java allows only one `extends`, teaser for
   interfaces later this module
5. **Overriding vs. overloading** — side-by-side code snippet, same signature vs. different
   parameter list
6. **`@Override`** — code snippet: a signature typo caught at compile time
7. **Virtual by default** — one-slide contrast with languages requiring an explicit `virtual`
   keyword; `final`/`private`/`static` as the three opt-outs
8. **No slicing in Java** — diagram: a reference passed by value, never a truncated object copy
9. **Abstract classes** — code snippet: `abstract class Shape` with one abstract and one
   concrete method
10. **Interfaces** — `implements`, multiple interfaces, and the diamond problem avoided by
    construction
11. **Interfaces vs. abstract classes** — decision-guide slide
12. **Looking ahead** — "Next: generics — type-safe, reusable code" teaser; capstone introduced
    this module

**Speaker notes:** this module covers the midterm's back half and the capstone's introduction —
pace Weeks 7–8 carefully, since dynamic dispatch and abstract classes are the conceptual
foundation for everything that follows, including the capstone's required polymorphic design.
