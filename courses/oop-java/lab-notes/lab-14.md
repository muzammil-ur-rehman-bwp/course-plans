# Lab Notes 14 — UML & Design Critique

**Concept recap:** a UML class diagram shows attributes/methods with visibility markers; a
hollow-triangle arrow denotes inheritance ("is-a"), a diamond connector denotes composition/
aggregation ("has-a"). SRP favors splitting a class with multiple unrelated reasons to change;
OCP favors a design that allows new behavior without editing existing code, typically via
polymorphism.

**Common pitfalls:**
- Drawing an inheritance arrow for a composition relationship or vice versa — always ask "is-a"
  or "has-a" before choosing the connector.
- A Task C refactor that just renames methods without actually separating the three
  responsibilities into three classes.
- A Task D refactor that replaces `instanceof` with a different kind of type-check (e.g. a string
  tag field) instead of genuine polymorphism — still an OCP violation, just a different-looking
  one.
- Over-applying SRP to the point of one-method classes with no real cohesion — the goal is
  *coherent* responsibility, not maximal fragmentation.

**Debugging tip:** if a refactored OCP solution still requires editing `totalArea` for a new
shape, the new shape's `area()` isn't actually being called polymorphically yet — check the
method signature's parameter type.

**Instructor tip:** have each group explain their UML diagram to another group verbally before
submitting — explaining a diagram out loud reliably surfaces "is-a"/"has-a" mix-ups.
