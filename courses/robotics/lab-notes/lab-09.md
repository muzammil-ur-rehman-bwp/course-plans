# Lab Notes 9 — LiDAR Scan to Occupancy Grid

**Concept recap:** each LiDAR range+angle reading is a point in the sensor's local frame; it
must be transformed (via Lab 2's functions) into the world/grid frame before being marked
occupied.

**Common pitfalls:**
- Forgetting to filter out `inf`/`NaN` range readings (common for "no obstacle detected within
  range"), which would otherwise produce invalid grid coordinates.
- Grid resolution too coarse or too fine for the provided world size — too coarse loses detail,
  too fine makes the grid array very large/slow for no added benefit at this scale.

**Debugging tip:** plot the raw local-frame points before transforming them — if they already
look wrong (e.g., not forming a sensible scan shape), the bug is in Task A, not the frame
transform.

**Instructor tip:** this is a deliberately light lab given the midterm — use extra time for
individual help on anything from Weeks 1–8 students are still shaky on.
