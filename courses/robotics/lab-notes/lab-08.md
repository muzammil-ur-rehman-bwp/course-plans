# Lab Notes 8 — Odometry from Encoder Data

**Concept recap:** odometry integrates small per-step motion estimates into a cumulative pose
estimate; errors in each step (quantization, slip) accumulate ("drift") over the trajectory.

**Common pitfalls:**
- Using the wrong `dt` (time step) between tick readings, causing systematic over/under-
  estimation of distance traveled.
- Comparing estimated vs. ground truth trajectories without aligning their starting
  pose/orientation, making the error look larger (or smaller) than it actually is.

**Debugging tip:** verify the `ticks_to_distance` conversion against a simple known case (e.g.,
one full wheel revolution) before using it in the full integration.

**Instructor tip:** Task D's reflection is the pedagogical payoff of this lab — make sure
students connect the observed drift pattern (likely worse during turns) back to the lecture's
explanation of why errors accumulate differently depending on motion type.
