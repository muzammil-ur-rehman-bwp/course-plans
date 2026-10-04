# Lab Notes 9 — Classes and Objects Basics

**Concept recap:** a class defines fields and methods; a constructor initializes a new object's
fields when `new` is called; `this` distinguishes a field from a same-named parameter; each
object has its own independent copy of the instance fields.

**Common pitfalls:**
- Forgetting `this.` in a constructor when the parameter name matches the field name, causing the
  field to be assigned to itself rather than to the parameter's value (the field silently keeps
  its default value).
- Expecting a method called on one object to somehow see or affect another object's fields — each
  method call operates on exactly one object's data (the one it was called on).
- Writing the constructor's return type as `void` — a constructor must have **no** return type
  at all, not even `void`; adding one turns it into a regular (and likely broken) method.
- Not initializing all fields in every constructor path, leaving some fields at their default
  value (`0`, `null`, `false`) unexpectedly.

**Debugging tip:** if an object's field always seems to hold its default value no matter what
constructor argument was passed, check for a missing `this.` in the constructor first.

**Instructor tip:** show the silent "self-assignment" bug (`name = name;` instead of
`this.name = name;`) live — it compiles with no error or warning in older code styles, making it
an important example of "compiles clean" not meaning "correct."
