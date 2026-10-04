# Lab Notes 4 — Inverse Kinematics

**Concept recap:** 2-link IK uses the law of cosines; reachability requires
`|l1-l2| <= d <= l1+l2`; feeding an IK solution back through FK and recovering the original
target is the standard correctness check for any IK implementation.

**Common pitfalls:**
- Not clipping the `cos_theta2` argument to `arccos` into `[-1, 1]` — floating-point rounding
  can push a value just outside this range for targets near the workspace boundary, causing a
  `NaN` result.
- Reporting only one IK solution when a target has two (elbow up/down) — both are valid unless a
  specific configuration preference is specified.

**Debugging tip:** the FK-then-IK-then-FK round trip (Task A) is the single most useful sanity
check in this lab — if the recovered position doesn't match the original target, the bug is in
the IK implementation, not elsewhere.

**Instructor tip:** deliberately include one unreachable target in the provided test cases so
Task C has a genuine case to analyze, not a hypothetical one.
