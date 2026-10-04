# Week 4 Summary — Loops: `for`, `while`, `do-while`

**Key takeaways:**
- `for` suits counter-controlled iteration; `while` suits condition-controlled iteration of
  unknown length; `do-while` guarantees at least one execution, useful for menu loops.
- `break` exits a loop entirely; `continue` skips to the next iteration.
- Sentinel-controlled loops read values until a special "stop" value is seen, and must guard
  against dividing by a zero count when no real data was entered.
- Off-by-one errors (`<` vs. `<=`, starting at `0` vs. `1`) and forgetting to update the loop
  variable (infinite loops) are the two most common loop bugs.

**You should now be able to:** choose the right loop construct for a task; write sentinel- and
counter-controlled loops correctly; spot and fix off-by-one and infinite-loop bugs.

**Next week:** methods — decomposing programs, and pass-by-value semantics for primitives vs.
object references.

**This week:** Assignment 1 was assigned (basics, operators, control flow, loops) — see
`assignments/assignment-01.md`.
