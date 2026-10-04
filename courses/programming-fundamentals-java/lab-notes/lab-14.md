# Lab Notes 14 — Sorting and Searching

**Concept recap:** bubble sort and selection sort both run in roughly n² comparisons in the worst
case but differ in swap count; binary search needs a sorted array and runs in roughly log₂(n)
comparisons, far outperforming linear search on large inputs.

**Common pitfalls:**
- Running binary search on data that was never actually sorted — it does not throw an error, it
  simply returns wrong (or missed) results silently, which is a much harder bug to notice than a
  crash.
- Off-by-one errors in the inner loop bound of bubble/selection sort (e.g., comparing
  `values[i]` and `values[i + 1]` when `i` is already at the last valid index), causing an
  `ArrayIndexOutOfBoundsException`.
- Computing `mid` in binary search without considering integer overflow for very large arrays
  (`(low + high) / 2` can overflow for `low`/`high` near `Integer.MAX_VALUE` — not usually an
  issue at CS1 array sizes, but worth mentioning as a "real-world" caveat).
- Forgetting to update `low`/`high` correctly (`mid + 1`/`mid - 1`, not `mid`), which can produce
  an infinite loop in binary search.

**Debugging tip:** if binary search seems to "miss" a value you know is in the array, the very
first thing to check is whether the array was actually sorted before searching.

**Instructor tip:** have students deliberately binary-search an *unsorted* array once and observe
the silently wrong result — contrasting this with the loud `ArrayIndexOutOfBoundsException` from
other bugs this semester makes the point that not all bugs announce themselves.
