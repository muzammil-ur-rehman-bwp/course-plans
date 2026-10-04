# Lab Notes 8 — Abstract Classes

**Concept recap:** an `abstract` class cannot be instantiated; an abstract method has no body and
must be implemented by every concrete subclass. A concrete method on an abstract class is shared
by all subclasses as-is. Code written against the abstract type can drive a mix of concrete
subclasses correctly via dynamic dispatch.

**Common pitfalls:**
- Forgetting to mark the class itself `abstract` while declaring an abstract method inside it —
  this is a compile error (a class with any abstract method must itself be declared `abstract`).
- A concrete subclass that forgets to override an inherited abstract method — also a compile
  error, not a silent gap.
- Writing a body for an abstract method by mistake — an abstract method must have no body at all
  (just a signature ending in `;`).
- Testing only with one concrete subclass, missing a bug that only shows up when the `List<Shape>`
  holds a genuine mix of types.

**Debugging tip:** "Circle is not abstract and does not override abstract method area() in
Shape" names exactly the missing override — implement it, don't suppress the error.

**Instructor tip:** have students predict `describe()`'s output for a `Rectangle` *before*
running it, to check they understand dynamic dispatch is resolving `area()` to the subclass's
version even though `describe()` is only defined once, in `Shape`.
