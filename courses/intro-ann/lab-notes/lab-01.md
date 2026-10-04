# Lab Notes 1 — Environment Setup & the McCulloch-Pitts Neuron

**Concept recap:** the MP neuron computes $z = \sum_i w_i x_i$ and outputs 1 if $z \ge \theta$,
else 0; weights and threshold are chosen by hand, not learned — that is next week's topic.

**Common pitfalls:**
- Off-by-one threshold errors: confusing "fires when $z \ge \theta$" with "fires when $z > \theta$"
  produces a truth table that is wrong at exactly one row — check boundary cases explicitly.
- Passing a Python list instead of a NumPy array to `np.dot`, which usually still works but hides
  shape bugs that appear later once vectors get larger (Week 2 onward) — get in the habit of using
  `np.array` now.
- For NAND, forgetting that its weights are the *negation* of AND's, not merely AND's weights with
  the output flipped after the fact (both lead to the same truth table here, but only the first
  matches the threshold-unit model being taught).

**Debugging tip:** print the raw weighted sum `z` alongside the final 0/1 output for every row of
the truth table while developing — it is much easier to spot a threshold bug when you can see the
intermediate value, not just the final decision.

**Instructor tip:** some students will want to jump ahead to "making it learn" — use that energy to
motivate Week 2 rather than letting it derail this lab; this week is deliberately about
hand-designed weights only.
