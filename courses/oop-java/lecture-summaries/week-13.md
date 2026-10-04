# Week 13 Summary — The Collections Framework

**Key takeaways:**
- `ArrayList<T>` is a growable, fully type-safe array; `HashMap<K, V>` is a key-value store that
  relies internally on a key's `hashCode()` and `equals()` — a custom key class must override
  both correctly, exactly per Week 2's contract, or lookups can silently fail.
- The enhanced `for` loop iterates any `List` directly, and any `Map` via `entrySet()`/
  `keySet()`/`values()`.
- A lambda expression is a compact way to implement a single-method functional interface like
  `Comparator`; `list.sort((a, b) -> ...)` and `Comparator.comparing(...).thenComparing(...)` are
  the two forms used to sort a `List` of custom objects by one or more fields.

**You should now be able to:** store and iterate objects with `ArrayList`/`HashMap`, and sort a
`List<CustomObject>` using a lambda-based `Comparator`.

**Next week:** software design — UML class diagrams, composition vs. inheritance, and an
introduction to SOLID.
