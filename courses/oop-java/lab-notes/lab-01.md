# Lab Notes 1 — Encapsulation

**Concept recap:** `private` fields plus public getters/setters form a class's controlled
interface. `this` disambiguates a field from a same-named parameter. An immutable class has only
`final` fields, set once in the constructor, and no setters.

**Common pitfalls:**
- Leaving fields `public` "just for this lab" — defeats the point of the exercise and loses
  validation.
- Forgetting `this.x = x;` and writing `x = x;` instead, which assigns the parameter to itself and
  leaves the field unset.
- Validating in the constructor but forgetting to apply the *same* validation in the setter,
  letting an object become invalid after construction even though it started out valid.
- Adding a setter to `Point2D` "just in case" — defeats the immutability requirement of Task C.

**Debugging tip:** if a field appears to silently stay at its default value (`0`/`0.0`/`null`)
after construction, check for the `x = x;` self-assignment bug before anything else.

**Instructor tip:** demonstrate what breaks when `Rectangle`'s `width`/`height` are made public
and some unrelated code sets `width` to a negative number directly — the single clearest
motivating example for encapsulation, worth repeating from the prerequisite course.
