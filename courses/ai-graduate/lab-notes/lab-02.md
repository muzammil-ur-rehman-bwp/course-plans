# Lab Notes 2 — Advanced Search: IDA*

**Concept recap:** IDA* trades A*'s O(b^d) memory for O(d) memory by running repeated
depth-first contours bounded by an increasing f-cost threshold; it preserves optimality and
completeness.

**Common pitfalls:**
- Forgetting to update the threshold to the *minimum* f-value that exceeded the previous
  threshold — using the wrong next-threshold update can cause IDA* to skip states or loop
  without making progress.
- Not checking for already-visited states on the current path, which can cause infinite loops on
  graphs with cycles (the 8-puzzle's state graph has cycles — sliding a tile and sliding it back
  returns to the same state).
- Comparing A* and IDA* "time" without accounting for IDA*'s node re-expansion overhead across
  contours — IDA* can be slower in wall-clock time even though it uses far less memory; both
  facts should be reported, not just one.
- In the bidirectional search mini-challenge, forgetting that the backward search must use the
  *reverse* of the maze's transition model — on an undirected grid this happens to be the same
  model, which can mask a bug that would show up on a directed graph.

**Debugging tip:** verify IDA* against known-small instances first (e.g., an 8-puzzle 2 moves
from the goal) where you can enumerate the correct solution by hand before trusting it on harder
instances.

**Instructor tip:** have students explicitly write down, before running Task C, which algorithm
they predict will use less memory and which will run faster — then compare predictions to
results. This reinforces the Week 2 complexity analysis as a predictive tool, not just a
post-hoc description.
