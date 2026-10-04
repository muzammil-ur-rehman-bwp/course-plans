# Week 14 Summary — Sorting & Searching Algorithms

**Key takeaways:**
- Bubble sort repeatedly swaps adjacent out-of-order elements, bubbling the largest unsorted
  value to the end each pass; selection sort instead finds the minimum of the unsorted remainder
  and swaps it into place, one swap per pass.
- Linear search works on any array but may examine every element; binary search is far faster
  but strictly requires a sorted array, halving the remaining range at each step.
- Running binary search on unsorted data produces silently wrong answers, not an obvious error —
  sortedness is a precondition the caller must guarantee.
- Comparing comparison counts informally (n²-ish for the sorts, log₂(n) for binary search) builds
  intuition for algorithmic efficiency ahead of formal Big-O analysis in later courses.

**You should now be able to:** implement bubble sort, selection sort, linear search, and binary
search on `int[]` arrays from scratch; trace each algorithm's execution by hand on a small array.

**This week:** Assignment 4 was assigned (encapsulation, sorting/searching) — see
`assignments/assignment-04.md`.

**Next week:** debugging, testing, and program design/style.
