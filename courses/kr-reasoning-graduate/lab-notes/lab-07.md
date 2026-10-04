# Lab Notes 7 — Dalal Belief Revision Operator

**Concept recap:** Dalal revision picks, among φ's models, those at minimum Hamming distance
from K's models — a concrete, AGM-satisfying semantic construction.

**Common pitfalls:**
- **Computing distance to K's models wrong**: `dist_to_K(m)` must take the *minimum* over all
  of K's models, not the distance to an arbitrary single model of K — if K has several models,
  using just one silently changes the answer.
- **Misapplying AGM postulates**: (K∗3)/(K∗4) are specifically about the relationship to plain
  expansion K+φ, not about K∗φ "being small" in some vague sense — Task B requires checking the
  postulates against their precise definitions (Week 7 lecture §2), not an intuitive paraphrase.
- Forgetting that (K∗5) is about φ being *unsatisfiable*, not merely contradicting K — a
  revision by a φ that merely contradicts K (the ordinary, interesting case) should never trigger
  (K∗5)'s absurdity clause; only an unsatisfiable φ (⊨¬φ) does.
- In Task C, conflating "revision" and "update" outcomes on the same example — they are allowed,
  and in general expected, to differ; treating a difference as evidence either operator is
  "wrong" misses the point of the distinction.

**Debugging tip:** reproduce the Tweety/penguin example exactly first — the lecture content's
hand-derived answer (K∗φ = {bird=True, flies=False}) is the one fixed point every later task
should be checked against if something seems off.

**Instructor tip:** have students compute Hamming distances by hand on paper for the two-coin
example in Task C before coding it — seeing which bit flips are "cheapest" under Dalal's metric
makes the minimal-change intuition concrete before the revision/update contrast is discussed.
