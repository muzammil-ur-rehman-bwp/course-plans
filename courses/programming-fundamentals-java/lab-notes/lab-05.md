# Lab Notes 5 — Methods

**Concept recap:** a method's parameter list receives copies of the caller's arguments; for a
primitive, reassigning the parameter never affects the caller; for an array/object parameter,
mutating the referenced object's contents *is* visible to the caller, but reassigning the
parameter itself is not.

**Common pitfalls:**
- Expecting a method like `tryToDouble(int n)` to change the caller's variable — it cannot, since
  `n` is a copy of the primitive value.
- Conflating "the method changed the array" with "the method changed the caller's reference" —
  only the former is possible for a reference-type parameter.
- Writing two overloads that differ only in return type (not parameter list) — this does not
  compile; overload resolution is based entirely on parameter types.
- Shadowing a field or outer variable with a same-named parameter and forgetting `this.` when a
  constructor/method needs to distinguish them (relevant again from Week 9 onward).

**Debugging tip:** when a method "isn't working," first check whether it's a primitive parameter
you expected to mutate — that's almost always the actual issue, not a bug in the method's logic.

**Instructor tip:** live-code the `zeroOutFirst(int[] arr)` vs. `tryToDouble(int n)` comparison
side by side on the projector — seeing both outcomes in the same run makes the distinction
concrete rather than abstract.
