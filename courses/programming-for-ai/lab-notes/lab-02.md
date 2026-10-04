# Lab Notes 2 — Data Structures & OOP

**Concept recap:** lists are ordered/mutable; tuples are ordered/immutable; sets hold unique
unordered elements; dicts map keys to values. Classes bundle state (attributes) with behavior
(methods); `self` refers to the specific instance a method is called on.

**Common pitfalls:**
- Using a list where a set would avoid duplicate-checking bugs (e.g., "visited" tracking).
- Forgetting `self` as the first parameter of instance methods.
- Mutating a dict/list while iterating over it (causes subtle bugs) — iterate over a copy or
  collect changes separately instead.

**Debugging tip:** if a class behaves unexpectedly, `print(vars(instance))` to inspect all
current attribute values at once.

**Instructor tip:** emphasize that the `Graph` class built here is not a throwaway exercise — it
is the exact data structure reused in Lab 5. Make sure every student leaves with a working copy.
