# Week 14 Summary — Sorting & Searching Algorithms

**Key takeaways:**
- Linear search checks every element in order and works on unsorted data; binary search is far
  faster but requires the array to already be sorted.
- Bubble sort repeatedly swaps adjacent out-of-order pairs; selection sort repeatedly places the
  minimum of the unsorted remainder — both are simple but roughly n² in comparisons.
- Binary search eliminates half the remaining candidates each step (roughly log₂(n) comparisons)
  by repeatedly checking the middle element.
- Running binary search on unsorted data gives wrong answers silently — always sort first.

**You should now be able to:** implement and trace bubble sort, selection sort, linear search,
and binary search by hand and in code; explain why binary search needs sorted input.

**Next week:** debugging, testing, and program design/style — the practices that keep programs
like these correct and maintainable.
