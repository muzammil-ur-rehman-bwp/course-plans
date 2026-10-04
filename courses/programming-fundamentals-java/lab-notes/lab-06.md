# Lab Notes 6 — 1D Arrays

**Concept recap:** arrays are objects created with `new`, indexed from `0` to `length - 1`;
Java always checks bounds and throws `ArrayIndexOutOfBoundsException` rather than reading
adjacent memory; a method can mutate an array's contents in place because the array itself is a
shared reference.

**Common pitfalls:**
- Off-by-one indexing: looping `i <= values.length` instead of `i < values.length` reaches one
  index past the end.
- Using `.length` (a field, no parentheses) when a method like `.length()` on `String` was
  intended, or vice versa — this is a frequent and purely syntactic source of confusion.
- Assuming an array parameter needs to be returned from a method to see changes — it doesn't,
  because of shared references; returning it anyway is harmless but unnecessary.
- Calling an aggregate method (like `max`) on an empty array without first deciding what should
  happen — this can throw `ArrayIndexOutOfBoundsException` if the code assumes `values[0]` exists.

**Debugging tip:** when you see `ArrayIndexOutOfBoundsException: Index N out of bounds for length
N`, the index `N` is exactly one past the valid range — check your loop's upper bound first.

**Instructor tip:** have students intentionally cause the exception once (as in Task D) and read
the message out loud before fixing it — recognizing this specific message on sight saves hours
of debugging later in the semester.
