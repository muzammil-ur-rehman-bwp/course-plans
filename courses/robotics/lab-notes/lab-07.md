# Lab Notes 7 — Torque & Actuator Calculations

**Concept recap:** `torque_required = m * g * r` for a horizontal arm link (worst case);
gearing multiplies torque and divides speed by the same ratio `N`.

**Common pitfalls:**
- Forgetting that `m * g * r` is the *worst-case* torque (arm fully horizontal) — torque is
  lower at other arm angles, which matters when discussing feasibility margins.
- Mixing units (e.g., mixing kg and g, or N*m and N*cm) when comparing to datasheet torque
  ratings — always convert to consistent units before comparing.

**Debugging tip:** sanity-check torque calculations against a known reference (e.g., a typical
hobby servo rated ~1-2 N*m) to catch unit errors before trusting a feasibility conclusion.

**Instructor tip:** this is a calculations-only lab (no simulator/code needed) — a good moment
to slow down and make sure every student can do the arithmetic independently, since it directly
supports Assignment 1 and general engineering-judgment skills.
