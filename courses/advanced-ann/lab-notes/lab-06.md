# Lab Notes 6 — Sharpness Estimation and SAM

**Concept recap:** `top_hessian_eigenvalue` estimates sharpness via power iteration on Hessian-
vector products; SAM's two-step update uses the gradient at a perturbed point to seek flatter
minima directly.

**Common pitfalls:**
- Using `create_graph=False` in the Hessian-vector-product helper — the second backward pass
  needs the computation graph from the first, so `hvp`'s internal `torch.autograd.grad(..., 
  create_graph=True)` on the first call is required; forgetting this raises a runtime error or
  silently returns incorrect gradients depending on the PyTorch version.
- Comparing SAM and SGD runs with different numbers of effective gradient evaluations — SAM uses
  two forward-backward passes per step; compare at equal *wall-clock-equivalent* step budgets
  (e.g., half as many SAM steps as SGD steps) if comparing compute-matched settings is the goal,
  and state explicitly which comparison (equal steps vs. equal compute) is being reported.
- In Task D, forgetting to rescale the *bias* term consistently (or handling a model with no bias
  on the first layer) — if biases are present, only the weight matrices need the compensating
  $\alpha$/$1/\alpha$ scaling for a ReLU network's function to be exactly preserved; verify test
  loss is unchanged before trusting the sharpness comparison.

**Debugging tip:** validate `top_hessian_eigenvalue` on a tiny 2-parameter quadratic loss with a
hand-computable Hessian before trusting it on the full network.

**Instructor tip:** have students state, before running Task D, what they expect to happen to
measured sharpness under the rescaling, and relate the outcome explicitly back to Week 6 §3's
critique — this is the lab's most important conceptual payoff, not just a coding exercise.
