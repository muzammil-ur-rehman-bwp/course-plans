# Lab Notes 7 — Differentiable Soft-Logic Loss (PyTorch)

**Concept recap:** t-norms relax ∧ to [0,1]; the product t-norm keeps a nonzero gradient on every
input, while Gödel's min-based t-norm zeroes the gradient on the non-minimal argument — this is
why product is the standard choice for gradient-based rule learning.

**Common pitfalls:**
- Forgetting `requires_grad=True` on the leaf tensors feeding the loss (or computing scores
  outside the autograd graph, e.g., via `.item()` partway through) — `loss.backward()` will then
  silently do nothing useful, or raise an error with no gradient to propagate.
- Using `torch.min`/`torch.max` directly for Gödel t-norm/t-conorm without checking they are
  differentiable where expected — `torch.min` does have a (sub)gradient PyTorch can use, but it
  routes *all* of the gradient to the minimizing argument and *none* to the other, exactly the
  zero-gradient-on-the-non-minimal-argument behavior Task B is meant to expose; a student who
  expects a "small but nonzero" gradient here instead of an exact zero has misread the lecture
  content's claim.
- In Task D, not resetting the random seed between the two training runs — without matching
  initial conditions, a slower loss curve could be an artifact of a different random
  initialization rather than the t-norm choice; reset `torch.manual_seed(0)` before each run.

**Debugging tip:** print `p_logits.grad` and `q_logits.grad` immediately after `loss.backward()`
on the very first training step, for both the product- and Gödel-based losses, and compare which
entries are exactly zero — this makes Task B's abstract claim concrete and checkable before
trusting the full 50-step training curve.

**Instructor tip:** have students predict, before running Task D, whether the Gödel-based
training will fully stall or merely train more slowly — the correct answer (it trains unevenly,
stalling on whichever instances are not currently the bottleneck term, rather than stalling
entirely) is more subtle than either extreme and is worth discussing explicitly.
