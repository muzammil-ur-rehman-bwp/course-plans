# Lab Notes 10 — `ArrayList`

**Concept recap:** `ArrayList<T>` grows/shrinks automatically, using `.add`, `.get`, `.set`,
`.remove`, and `.size()` instead of array syntax; generics catch type mistakes at compile time;
autoboxing/unboxing converts transparently between a primitive and its wrapper class.

**Common pitfalls:**
- Writing `list.length` instead of `list.size()` — `ArrayList` has no `.length` field; that is
  array syntax specifically.
- Calling `.remove(index)` when `.remove(Object)` was intended (or vice versa) — `ArrayList` has
  both overloads, and passing an `int` always calls the index version, which can silently remove
  the wrong element if you meant to remove by value.
- Comparing two `Integer` objects from an `ArrayList<Integer>` with `==` instead of `.equals()`
  — small cached `Integer` values (roughly −128 to 127) may coincidentally compare `==` as `true`,
  masking the bug for small test values the same way `String` interning does.
- Forgetting the diamond `<>` or mismatching the type parameter, leading to compile errors that
  can look unrelated to the actual mistake.

**Debugging tip:** if `.remove(...)` removed the wrong element, check whether an `int` argument
was interpreted as an index rather than a value to search for.

**Instructor tip:** demonstrate the `Integer` `==` caching quirk directly (e.g., `Integer.valueOf(100) == Integer.valueOf(100)` vs. `Integer.valueOf(200) == Integer.valueOf(200)`) — it is a
genuinely surprising, memorable example of why `.equals()` is the only safe choice for wrapper
types.
