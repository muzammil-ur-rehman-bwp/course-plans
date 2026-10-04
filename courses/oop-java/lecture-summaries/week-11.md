# Week 11 Summary — Generics II: Generic Methods, Wildcards, Bridge to Collections

**Key takeaways:**
- A generic method declares its own type parameter, scoped to just that method, independent of
  any class-level type parameter — useful for a single reusable method whose type is inferred at
  each call site.
- `<? extends T>` accepts a collection of some unknown subtype of `T`; `<? super T>` accepts a
  collection of some unknown supertype — both appear often in standard-library signatures and are
  worth recognizing when reading documentation.
- `List<T>` and `Map<K, V>` are themselves generic classes built from exactly the ideas covered
  this week and last, so the Collections Framework arriving next week should already look
  familiar.

**You should now be able to:** write a generic method with its own type parameter, and read a
wildcard-bounded method signature from the standard library.

**Next week:** exception handling — checked vs. unchecked exceptions, custom exception classes,
and try-with-resources. The capstone proposal is due this week.
