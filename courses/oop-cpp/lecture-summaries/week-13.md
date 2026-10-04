# Week 13 Summary — Introduction to the STL

**Key takeaways:**
- `std::vector<T>` is a growable, type-safe replacement for fixed-size arrays; `std::map<K, V>`
  provides fast, sorted key-value lookup.
- An iterator (`begin()`/`end()`, `*it`, `++it`) is a uniform way to traverse any STL container,
  regardless of its internal implementation.
- Range-`for` is iterator traversal with less syntax; use an explicit iterator when you need the
  iterator itself (e.g. to erase or track a position).
- `std::vector` and `std::map` are class templates built on exactly the ideas from Weeks 10–11 —
  prefer them over a hand-written container once the STL already covers the need.

**You should now be able to:** use `std::vector`/`std::map` and iterate them with both explicit
iterators and range-`for`, and explain why iterators generalize across container types.

**Next week:** software design — reading/drawing basic UML class diagrams, composition vs.
inheritance as a design decision, and an undergraduate introduction to two SOLID principles.
