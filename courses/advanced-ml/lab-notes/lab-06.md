# Lab Notes 6 — Truncated Stick-Breaking Dirichlet Process Sampler

**Concept recap:** stick-breaking draws $\beta_k\sim\mathrm{Beta}(1,\alpha)$ and sets
$\pi_k=\beta_k\prod_{j<k}(1-\beta_j)$; weights sum to 1 almost surely as the truncation level
grows, and $\alpha$ controls how concentrated or spread the weights are.

**Common pitfalls:**
- Choosing a truncation level $K$ too small for large $\alpha$ — since large $\alpha$ spreads
  mass thinly across many atoms, a small $K$ leaves substantial unaccounted tail mass; Task B is
  designed specifically to make this visible, so do not skip the small-$K$ residual-mass check.
- Forgetting that `atoms` and `weights` are independent draws — a common bug is accidentally
  correlating atom order with weight order (e.g., sorting one but not the other), which silently
  breaks the sampler's correctness.
- In Task D, miscounting "distinct atoms carrying 90% of weight" by using the *drawn samples'*
  empirical counts rather than the underlying stick-breaking weights directly — both are valid
  but answer slightly different questions; be explicit in your write-up about which you computed.

**Debugging tip:** for a quick sanity check, verify that at $\alpha\to0$ (try $\alpha=0.01$), the
first weight $\pi_1$ should be very close to 1 — if it is not, there is likely a bug in the
cumulative-product computation for `remaining`.

**Instructor tip:** have students compute $\mathbb{E}[\beta_1]=1/(1+\alpha)$ by hand for each
$\alpha$ value used in the lab and compare it to their simulated $\pi_1$ values before moving to
Task C — this directly reinforces the theoretical mean computation underlying the whole exercise.
