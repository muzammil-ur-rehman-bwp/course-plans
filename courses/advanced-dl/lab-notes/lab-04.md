# Lab Notes 4 — Mixture-of-Experts Layer

**Concept recap:** top-$k$ MoE evaluates only $k$ of $N$ experts per token, decoupling total
parameter count ($\propto N$) from per-token compute ($\propto k$); an auxiliary load-balancing
loss counteracts router collapse.

**Common pitfalls:**
- Forgetting to renormalize the top-$k$ gate values after selection — without renormalization,
  the output magnitude is systematically smaller than a correctly-gated layer's, which can look
  like "the MoE model trains worse" for a reason unrelated to routing itself.
- Computing FLOPs in Task B using $N$ (total experts) instead of $k$ (active experts per token) —
  this is the single most common conceptual error this week, and defeats the point of the whole
  exercise if left uncorrected.
- Setting the load-balancing coefficient too high — an overly strong auxiliary loss can force
  near-uniform routing regardless of which expert is actually best suited to a given token,
  trading away useful specialization for balance; the lecture's small coefficient (~0.01) is a
  starting point, not a fixed rule.
- In the naive double-loop `TopKMoE` implementation, forgetting that gradients must still flow
  correctly through the masked assignment — verify with a small toy input that `loss.backward()`
  produces nonzero gradients for every expert that received at least one token.

**Debugging tip:** on a tiny toy batch (e.g., 4 tokens, $N=4$, $k=1$), manually trace which expert
each token is routed to and verify the output equals exactly that expert's output scaled by its
gate value — this isolates routing bugs from training-dynamics issues.

**Instructor tip:** have students predict, before running Task C, what the usage histogram will
look like with no load-balancing loss at all (most will correctly predict skew) — then ask them
to predict *why* skew compounds over training (the rich-get-richer gradient-signal argument from
lecture), not just that it occurs.
