# Lab Notes 12 — Recursion

**Concept recap:** every recursive function needs a base case (solved directly) and a recursive
case (reduces toward the base case); each call waits on the call stack for its recursive call to
return before computing its own result.

**Common pitfalls:**
- Missing a base case entirely, or writing one that is never actually reached (e.g., checking
  `n == 0` but the recursive step decreases by 2, skipping past 0 for odd inputs) — causes
  infinite recursion and a stack overflow crash.
- Forgetting to use the recursive call's *result* (e.g., calling `factorial(n - 1)` but not
  multiplying it into the return value) — the recursion runs but produces a meaningless answer.
- For `sumArray`: advancing the pointer/size incorrectly (e.g., not shrinking `size` each call),
  causing either an infinite recursion or reading out of bounds.
- Assuming recursion is always "better" — for simple accumulation (like array sum), the iterative
  version is usually just as clear and uses less stack space; recursion is a tool, not a goal.

**Debugging tip:** add a temporary `std::cout << "factorial(" << n << ")\n";` as the first line
of a recursive function while debugging — watching the sequence of calls print makes an infinite
or wrong-direction recursion immediately visible, then remove the line once fixed.

**Instructor tip:** have students trace Task B (`sumOfDigits`) on paper, writing every nested
call and its return value, before letting them run the code — tracing by hand is what builds the
mental model that typing code alone does not.
