# Lab Notes 1 — Formal Problem Formulation & Environment Setup

**Concept recap:** a search problem is the tuple ⟨S, s₀, A, T, G, c⟩; a precise formalization
makes clear what counts as a state, an action, and a goal before any algorithm is written.

**Common pitfalls:**
- Confusing the *state* (a full description of the world) with the *action* (what changes it) —
  e.g., describing the 8-puzzle's state as "slide tile left" rather than the resulting board
  configuration.
- Forgetting that `successors` must respect the blank tile's position (corner vs. edge vs.
  center tiles have 2, 3, or 4 legal moves respectively) — an off-by-the-board-edge bug is the
  most common Task D mistake.
- Writing a PEAS description that is too vague to translate into a concrete formal tuple (Task B)
  — graduate rigor means being able to say precisely what S, A, T, G, c are, not just naming
  categories.
- Treating "subfield mapping" (Task C) as guesswork rather than tracing the question back to a
  specific technique/algorithm this course or a sibling course actually covers.

**Debugging tip:** for Task A/D, test `successors` on a known state (e.g., the solved board) by
hand first and compare its output against your Python function's output before trusting it on
arbitrary states.

**Instructor tip:** spend extra time on Task C — many students arrive from the undergraduate
survey still thinking of "AI" as one undifferentiated field; this lab is the first concrete
practice at placing a question precisely within this course's disjoint scope.
