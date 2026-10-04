# Lab Notes 6 — Arrays (1D)

**Concept recap:** arrays index from `0` to `size - 1`; an array passed to a function decays to a
pointer, so always pass the size alongside it; mark an array parameter `const` when the function
only reads it.

**Common pitfalls:**
- Off-by-one: looping `i <= size` instead of `i < size`, reading/writing one past the array's end
  — undefined behavior that may not crash immediately, making it easy to miss.
- Forgetting that C++ does not bounds-check array access at all — a bug can silently corrupt
  unrelated memory instead of raising a clear error.
- Reversing an array by copying into a second array when an in-place swap (two index pointers
  moving toward the middle) is both simpler and the intended skill being practiced.
- Returning `-1` from a search function but then using it directly as an array index elsewhere
  without checking for it first.

**Debugging tip:** print the array's full contents before and after any function that modifies
it — this immediately shows whether the function did what was intended, and at which index
things went wrong if not.

**Instructor tip:** ask students what happens if they access `array[size]` (one past the end) and
run it live — the output is often "fine-looking" garbage, which is the most convincing way to
show why undefined behavior is dangerous precisely because it doesn't always crash.
