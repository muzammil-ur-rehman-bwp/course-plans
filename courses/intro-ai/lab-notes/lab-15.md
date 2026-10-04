# Lab Notes 15 — Vision & Robotics Survey + Ethics Reflection

**Concept recap:** a simple edge detector flags large local intensity differences as likely
object boundaries; a reactive grid agent picks the neighboring cell that most reduces Manhattan
distance to the goal, with no memory or planning — fast but easily stuck behind concave
obstacles; AI ethics concerns (bias, safety, privacy, societal impact) apply to both classical
and learned systems alike.

**Common pitfalls:**
- Expecting the reactive grid agent to navigate around a U-shaped obstacle — by design it has
  no planning or memory, so it can get stuck oscillating; this is a feature of the exercise
  (motivating Weeks 3–4/9's planning and search), not a bug to "fix" with more cleverness.
- Running the 1-D edge detector on a row with gradual (not abrupt) intensity change and
  expecting a sharp spike — gradual gradients produce small, spread-out differences, which is
  itself worth discussing as a real limitation of simple difference-based edge detection.
- Treating the Task D ethics reflection as a formality — a vague, generic answer ("AI should be
  fair") does not engage with the specific case study and will not satisfy the rubric.

**Debugging tip:** if the reactive grid agent oscillates between two cells forever, print its
position at each step and the Manhattan distance to goal from each candidate move — this
reveals when two moves are tied in greedy value, which is exactly when the greedy strategy fails.

**Instructor tip:** have each small group present their Task D mitigation idea briefly to the
whole class — hearing 4–5 *different* concrete mitigations for the same case study is far more
useful than any one group's answer alone.
