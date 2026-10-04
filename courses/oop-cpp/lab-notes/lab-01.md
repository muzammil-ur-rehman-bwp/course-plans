# Lab Notes 1 — Encapsulation

**Concept recap:** `public` members form a class's interface; `private` members are hidden
implementation detail; `protected` is visible to future derived classes only. Getters read
private data; setters validate before writing it. A `const` member function promises not to
modify the object and is required to be callable through a `const` reference.

**Common pitfalls:**
- Leaving data members `public` "just for this lab" — defeats the point of the exercise and loses
  validation.
- Forgetting `const` on a read-only method, which then can't be called on a `const Rectangle&`
  parameter (a compile error worth learning to read now, since it recurs all semester).
- Validating in the constructor but forgetting to apply the *same* validation in the setter,
  letting an object become invalid after construction even though it started out valid.
- Writing a setter that returns a value or has side effects beyond the object itself — keep
  setters focused on validating and storing.

**Debugging tip:** if a `const` method fails to compile with an "assignment of member in a const
function" error, you've likely tried to modify a data member inside a method that shouldn't be
modifying anything — double check whether the method truly needs to be non-`const` instead.

**Instructor tip:** demonstrate what breaks when `Rectangle`'s `width_`/`height_` are made public
and some unrelated code sets `width_` to a negative number directly — the single clearest
motivating example for encapsulation, worth repeating from the prerequisite course.
