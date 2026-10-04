# Presentation: Module 4 — STL, Software Design, RAII & Capstone (Weeks 13–16)

**Format:** Slide deck outline for lecture delivery (convert to slides in institution's template).

1. **Title slide** — Object Oriented Programming (C++): Module 4, From Code to Craftsmanship
2. **`std::vector`/`std::map`** — code snippet slide: both are class templates students already
   understand the shape of
3. **Iterators** — diagram: `begin()`/`end()`, `*it`/`++it`, the same pattern across container
   types
4. **UML class diagrams** — notation slide: attributes, methods, inheritance vs. composition
   arrows
5. **Composition vs. inheritance, the design question** — the `Stack : public vector` anti-
   pattern vs. the correct composition fix
6. **SOLID: SRP** — before/after slide: `ReportGenerator` split into three focused classes
7. **SOLID: OCP** — before/after slide: `if`/`switch` on type replaced by polymorphism
8. **RAII, revisited** — one-slide timeline: destructors (Week 2) → exceptions (Week 12) → smart
   pointers (now), same underlying pattern throughout
9. **`unique_ptr` vs. `shared_ptr`** — decision-diagram slide: exclusive vs. shared ownership
10. **Debugging & testing OOP code** — breakpoints in a constructor; `assert`-based test examples
11. **The full course map** — Week 1 → Week 15 dependency chain diagram
12. **Capstone expectations recap** — rubric summary slide
13. **Looking ahead** — "Next: Data Structures & Algorithms" teaser

**Speaker notes:** this module closes the loop — explicitly connect Week 14's design principles
and Week 15's RAII back to every hierarchy built since Week 6, so students see the whole semester
as one coherent design discipline, not a list of separate features.
