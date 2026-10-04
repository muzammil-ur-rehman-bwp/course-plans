# Lab Notes 13 — Introduction to the STL

**Concept recap:** `std::vector<T>` is a growable, type-safe array; `std::map<K, V>` stores
sorted key-value pairs with fast lookup; an iterator (`begin()`/`end()`, `*it`, `++it`) traverses
any STL container uniformly; range-`for` is iterator traversal with less syntax.

**Common pitfalls:**
- Using `map["key"]` to *check* whether a key exists — this inserts a default-valued entry as a
  side effect if the key is missing; use `find()` or `count()` for a true existence check.
- Forgetting that `std::map` iterates in **key** order, not insertion order — don't assume the
  tally in Task B prints in the order words first appeared.
- Writing `it->first`/`it->second` incorrectly as `(*it).first`'s less-idiomatic equivalent, or
  confusing which is the key vs. the value.
- In Task D, using `[]` indexing instead of an actual iterator-based loop, missing the point of
  practicing the iterator pattern itself (even though `[]` would also work for `std::vector`).

**Debugging tip:** if `std::map` output looks "out of order" compared to your expectations, check
whether you expected insertion order — `std::map` always iterates by ascending key, which is a
feature, not a bug, once you expect it.

**Instructor tip:** contrast `std::map`'s automatic sorted iteration with the Week 1–16
prerequisite course's hand-rolled sorting algorithms (bubble sort, etc.) — a good moment to note
how much the STL already provides versus writing such logic by hand.
