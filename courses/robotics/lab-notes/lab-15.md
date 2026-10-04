# Lab Notes 15 — End-to-End Navigation Pipeline

**Concept recap:** this lab is integration, not new theory — sensing feeds state estimation,
state estimation feeds planning, planning feeds control, control drives the robot, and the loop
repeats/re-plans as needed.

**Common pitfalls:**
- Running planning once at the start and never re-checking it, so a dynamic/new obstacle is
  never avoided after the initial plan.
- Blocking calls (e.g., a slow planning call) inside a single-threaded node's callback, causing
  the entire node to become unresponsive to new sensor data while planning runs.
- Mismatched coordinate frames between the occupancy grid (built in one frame) and the planner/
  controller (expecting another) — re-verify frame consistency end-to-end.

**Debugging tip:** test each piece (sensing, planning, control) independently first — most
integration bugs come from assuming a previously-tested component behaves the same way once
wired into the full pipeline with real timing/data flow.

**Instructor tip:** this is the dress rehearsal for the capstone — budget extra lab time/office
hours this week, since integration bugs are typically the hardest and most time-consuming to
diagnose all semester.
