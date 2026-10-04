# Presentation: Module 3 — Generics, Exceptions & Collections (Weeks 10–13)

**Format:** Slide deck outline for lecture delivery (convert to slides in institution's template).

1. **Title slide** — Object Oriented Programming (Java): Module 3, Genericity & Robustness
2. **Why generics** — code snippet: an `Object`-based box needing an unchecked cast, vs. a
   `ClassCastException` at runtime
3. **Generic classes** — `Box<T>`/`Pair<T, U>` code snippet
4. **Bounded types & raw types** — one-slide warning: a raw type compiles but loses all type
   safety
5. **Generic methods** — code snippet: a generic `max` with its own `<T>`
6. **Wildcards** — `<? extends T>`/`<? super T>` read from a library signature
7. **Checked vs. unchecked exceptions** — the `Throwable`/`Exception`/`RuntimeException`
   hierarchy, diagrammed
8. **Custom exceptions & try-with-resources** — code snippet: a checked custom exception with
   `throws`, and an `AutoCloseable` resource closed automatically
9. **`List`/`ArrayList`** and **`Map`/`HashMap`** — two code snippets, side by side
10. **Lambdas & `Comparator`** — code snippet: sorting a `List<Student>` by GPA with a lambda
11. **Looking ahead** — "Next: software design — UML and SOLID" teaser

**Speaker notes:** connect every generics example back to the Collections Framework arriving at
the end of this module — by the time `List<T>`/`Map<K, V>` appear, their generic signatures
should already look familiar rather than new syntax.
