# Lab Notes 4 — Loops

**Concept recap:** `for` suits known-count iteration; `while` checks its condition before each
iteration; `do-while` checks after, guaranteeing at least one execution; always confirm a loop's
variable is actually updated and its boundary condition matches the intended range.

**Common pitfalls:**
- Off-by-one errors: using `<=` where `<` was intended (or vice versa), producing one extra or
  one missing iteration.
- Forgetting to increment/update the loop control variable inside a `while` loop, causing an
  infinite loop.
- Dividing by a count of `0` when no sentinel-terminated input was actually entered — always
  guard this case explicitly.
- Confusing `break` (exits the loop entirely) with `continue` (skips to the next iteration) when
  reading or writing loop control logic.

**Debugging tip:** if a program seems to hang, it's very likely an infinite loop — interrupt it
(Ctrl+C or the IDE's stop button) and check that every loop variable referenced in the condition
is actually modified inside the loop body.

**Instructor tip:** deliberately run an infinite loop once in front of the class (with a visible
stop button ready) so students recognize the symptom and know how to recover, rather than
panicking the first time it happens to them.
