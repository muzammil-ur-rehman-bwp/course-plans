# Lab Notes 8 — References and Midterm Review

**Concept recap:** a reference-type variable holds a reference to an object on the heap, not the
object itself; assigning one reference variable to another shares the same object; `null` means
"refers to no object," and using it throws `NullPointerException`.

**Common pitfalls:**
- Expecting `int[] b = a;` to copy the array — it does not; `a` and `b` now refer to the exact
  same array object, and mutating through either is visible through both.
- Forgetting to check for `null` before calling a method or accessing a field/element on a
  reference that might not have been assigned yet.
- Misreading a `NullPointerException` message as being about the variable's *name* rather than
  the specific expression that was `null` — modern JDKs (14+) name the exact null expression in
  the message, which is worth pointing students to directly.
- Confusing "two references are `==`" (same object) with "two references are `.equals()`" (same
  content, if the class defines it that way) — this is the Week 7 `String` lesson generalized to
  all objects.

**Debugging tip:** when you see `NullPointerException`, find the exact expression named in the
message and trace backward to find where that reference should have been assigned but wasn't.

**Instructor tip:** draw the stack/heap diagram live on the whiteboard for the `int[] b = a;`
example — students retain the shared-reference model far better from a picture than from text
alone.
