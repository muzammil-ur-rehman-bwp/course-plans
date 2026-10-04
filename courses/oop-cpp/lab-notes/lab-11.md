# Lab Notes 11 — Class Templates

**Concept recap:** `template <typename T> class` generalizes an entire class over a type
parameter; each distinct type used (e.g. `Stack<int>` vs. `Stack<std::string>`) is a separately
generated class from the same template; a class template can take multiple independent type
parameters.

**Common pitfalls:**
- Forgetting the `template <typename T>` header before an out-of-class member definition, or
  using `Stack::` instead of `Stack<T>::` as the scope qualifier, if defining methods outside the
  class body.
- Calling `top()` or `pop()` on an empty `Stack<T>` without checking `empty()` first — this is
  undefined behavior (`std::vector::back()`/`pop_back()` on an empty vector), not a clean error;
  next week's exception handling gives a better way to signal this.
- Writing `Pair<T, U>`'s `operator==` without `const` on the method itself or on its parameter,
  breaking its usability in `const` contexts.
- Mixing up `Pair<int, std::string>` and `Pair<std::string, int>` as if they were the same type —
  the order of type parameters matters and produces genuinely different instantiations.

**Debugging tip:** if a template class fails to compile only when instantiated for a *specific*
type, check the implicit constraints (Week 10) — e.g. `Pair<T, U>::operator==` requires both `T`
and `U` to support `==`.

**Instructor tip:** show the generated symbol names (e.g. via `nm` or a compiler explorer, if
available) for `Stack<int>` vs. `Stack<std::string>` to make concrete that these truly are
separate, independently compiled classes.
