# Lab Notes 12 — Muddy Children Simulator

**Concept recap:** worlds are subsets of "who is muddy"; child i's accessibility relates worlds
agreeing on everyone except possibly i; each round's public "no" removes worlds where someone
would actually have known; muddy children know simultaneously at round k.

**Common pitfalls:**
- **Checking knowledge against the wrong world set**: at round t, whether a child knows must be
  checked against the world set *as of the end of round t−1's update* (before that round's own
  "no" is incorporated) — computing the round-t update first and then checking knowledge against
  the *updated* set silently shifts every result by one round.
- **Excluding the empty world incorrectly**: the public announcement "at least one is muddy"
  removes exactly the empty-set world — forgetting this step (or removing more than that) breaks
  the base case (k=1) entirely.
- **Treating "no one else is in W" as "trivially knows"**: `knows_own_status` correctly treats a
  world with no other indistinguishable worlds in the current set as "known" (vacuously) — this
  is mathematically correct, not a bug, and is exactly what makes a single muddy child deduce
  their status immediately at round 1.
- In Task D, confusing distributed knowledge's intersection construction with common knowledge's
  union/transitive-closure construction — using the wrong one silently computes the other
  notion.

**Debugging tip:** reproduce the n=3, k=2 trace from the lecture content exactly first (round 2,
worlds {{0,1},{0,2},{1,2},{0,1,2}} surviving after round 1) — if your intermediate world sets
differ from this hand-derived trace at any step, debug there before running the Task B sweep.

**Instructor tip:** have students act out a 3-person, 2-muddy version of the puzzle live (using
stickers or cards) before coding it — physically experiencing "I see two muddy foreheads, so if I
were clean, those two would have figured it out by round 1, but they haven't" makes the induction
argument intuitive before it becomes code.
