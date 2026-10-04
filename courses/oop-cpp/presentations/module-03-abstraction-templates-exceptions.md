# Presentation: Module 3 — Abstract Classes, Templates & Exception Handling (Weeks 9–12)

**Format:** Slide deck outline for lecture delivery (convert to slides in institution's template).

1. **Title slide** — Object Oriented Programming (C++): Module 3, Abstraction & Generic Code
2. **Pure virtual functions** — code snippet: `virtual double area() const = 0;`
3. **Abstract base classes** — diagram: `Shape` cannot be instantiated; `Circle`/`Rectangle` can
4. **Interfaces-by-convention** — comparison slide: Java's `interface` keyword vs. C++'s
   all-pure-virtual abstract class
5. **Function templates** — code snippet: three near-duplicate `maxOf` overloads collapsing into
   one `template <typename T>` version
6. **Argument deduction vs. explicit instantiation** — diagram: when deduction works, when it
   needs help
7. **Class templates** — code snippet: `Stack<T>` backed by `std::vector<T>`
8. **Multiple type parameters** — code snippet: `Pair<T, U>`
9. **Why exceptions** — before/after: an ambiguous sentinel return value vs. a thrown exception
10. **Stack unwinding** — diagram: a three-level call chain unwinding to a `catch`
11. **The standard exception hierarchy** — diagram: `std::exception` and its common derived types
12. **Custom exceptions & catching correctly** — code snippet: deriving from `std::runtime_error`;
    catch-by-reference, most-specific-first ordering
13. **Looking ahead** — "Next: the STL, software design, and smart pointers" teaser

**Speaker notes:** this module is the most syntax-dense of the semester — pace generously, and
explicitly call back to Week 8's slicing lesson when covering catch-by-reference in slide 12, so
the connection lands rather than feeling like a new, unrelated rule.
