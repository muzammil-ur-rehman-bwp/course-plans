# Lab Notes 14 — Sorting & Searching Algorithms

**Concept recap:** bubble sort swaps adjacent out-of-order pairs repeatedly; selection sort
repeatedly places the minimum of the unsorted remainder; linear search checks elements in order
(works unsorted); binary search repeatedly halves the search range but requires sorted input.

**Common pitfalls:**
- Running binary search on an array that was never actually sorted (e.g., sorted a *copy* but
  searched the original) — gives wrong answers with no error at all.
- Off-by-one in the sort's inner loop bound (e.g., `size - pass` vs. `size - 1 - pass`), leaving
  the last element unsorted or reading one past the end.
- In binary search, computing `mid = (low + high) / 2` instead of `low + (high - low) / 2` —
  functionally equivalent for the small arrays used here, but worth knowing the safer form exists
  for larger values elsewhere.
- Updating `low`/`high` incorrectly (e.g., `low = mid` instead of `low = mid + 1`), which can
  cause binary search to loop forever on certain inputs.

**Debugging tip:** print the array after every pass of a sort (not just at the end) — this
quickly shows whether the algorithm is making progress correctly or stuck swapping the same
elements.

**Instructor tip:** have students count actual comparisons made by linear vs. binary search on
the same 20–30 element sorted array (add a counter variable) — seeing the concrete numbers makes
the "halves the problem" intuition tangible rather than abstract.
