# Week 8 Summary — Intro to Objects & References; Midterm Review

**Key takeaways:**
- A reference-type variable holds a reference to an object on the heap, not the object itself;
  assigning one reference variable to another shares the same object, not a copy.
- `null` means "refers to no object"; using a `null` reference throws `NullPointerException`.
- The general rule from Week 7 extends to all reference types: `==` compares identity, not
  content; `.equals()` (when properly overridden, as `String` does) compares content.
- The stack holds local variables/references for in-progress calls; the heap holds the actual
  objects, automatically reclaimed by the garbage collector once unreachable.

**You should now be able to:** trace reference diagrams by hand; explain `null` and
`NullPointerException`; distinguish `==` from `.equals()` for general objects; summarize Weeks
1–8 for the midterm.

**This week:** Quiz 4 (2D arrays, strings, objects/references) — see `quizzes/quiz-04.md`.

**Next week:** Midterm Exam, then classes and objects — defining your own types.
