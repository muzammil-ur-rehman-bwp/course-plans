# Lab Notes 13 — Collections Framework

**Concept recap:** `ArrayList<T>` is a growable, type-safe array; `HashMap<K, V>` relies on a
key's `hashCode()`/`equals()` pair internally. The enhanced `for` loop iterates any `List`
directly and a `Map` via `entrySet()`/`keySet()`/`values()`. A lambda is a compact way to
implement `Comparator`'s single abstract method for sorting.

**Common pitfalls:**
- Using a custom class as a `HashMap` key without a correct, matching `equals`/`hashCode` pair
  (Week 2) — lookups can silently fail to find an entry that logically exists.
- Calling `list.get(i)` in a loop by index when an enhanced `for` loop is clearer and avoids
  off-by-one mistakes.
- Writing a `Comparator` lambda that returns the wrong sign convention (positive when it should
  be negative) — resulting in a reverse sort; test with a small, hand-checkable list first.
- Forgetting `.reversed()` or `Collections.reverseOrder()` and instead manually negating a
  `compareTo` result inconsistently.

**Debugging tip:** if a `Map.get(key)` unexpectedly returns `null` for a key that looks present
when printed, suspect a broken `equals`/`hashCode` pair on the key type before anything else.

**Instructor tip:** show the exact same sort written three ways — a hand-written `Comparator`
class, a lambda, and `Comparator.comparing(...)` — to make the lambda's conciseness, not its
novelty, the point.
