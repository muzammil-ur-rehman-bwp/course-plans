# Lab Notes 6 — Semantic Networks and Frames with Inheritance

**Concept recap:** strict inheritance walks an IS-A chain and returns the first property value
found, with no way to distinguish "the general case" from "an exception"; frames fix this by
separating a slot's own value/default from inherited ones, checking the most specific frame
first.

**Common pitfalls:**
- Confusing default/non-monotonic inheritance with strict inheritance — in `Frame.get_slot`, a
  frame's own `defaults` must still be checked *before* moving to the parent, not after; checking
  the parent first would let a general ancestor's fact override a more specific frame's own
  default, which is backwards.
- Implementing `get_slot` so that a frame's default silently overrides its *own* explicit slot
  value (checking `defaults` before `slots` at the same frame) — within one frame, an explicit
  slot value must always win over that same frame's default.
- Building the Task A semantic network so that the "exception" case is never actually exercised
  (e.g., setting `can_fly = False` on `Bird` directly instead of `Penguin`) — the point of Task A
  is to *reproduce the wrong answer first*, so the Task C fix is a visible, meaningful contrast.
- Forgetting that `get_property`/`get_slot` must handle a node/frame with no parent (`None`)
  gracefully, returning `None` rather than raising an error when no value or default is found
  anywhere in the chain.

**Debugging tip:** print the full ancestor chain walked (each frame name visited, in order) for
a `get_slot` call that returns the wrong value — the bug is almost always that the chain stops
too early (at a default instead of continuing to check for a more specific override) or too late.

**Instructor tip:** require Task A to be completed and run (showing the wrong answer) *before*
students start Task C — skipping straight to the frame-based fix, without first reproducing the
problem it solves, leaves students unable to explain why frames handle this case better.
