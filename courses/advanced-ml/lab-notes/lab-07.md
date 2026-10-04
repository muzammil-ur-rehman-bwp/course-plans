# Lab Notes 7 — Chinese Restaurant Process Sampler and Infinite Mixture Model

**Concept recap:** the CRP seats customer $n+1$ at table $k$ with probability $n_k/(n+\alpha)$ or
a new table with probability $\alpha/(n+\alpha)$; the expected number of tables grows as
$O(\alpha\log n)$, and the resulting partition is exchangeable.

**Common pitfalls:**
- Off-by-one errors in the seating-probability normalization — at the moment customer $i$ (0-
  indexed) is being seated, exactly $i$ customers are already seated, so the denominator must be
  $i+\alpha$, not $i+1+\alpha$ or $n+\alpha$.
- In Task B, comparing a *single* run's $K_n$ to the predicted $\alpha\ln n$ and concluding the
  theory is wrong when they do not match exactly — $\mathbb{E}[K_n]=\alpha\ln n + O(\alpha)$ is an
  expectation; average over several independent runs before judging agreement.
- In Task C, forgetting that the *specific* occupancy pattern at $n=10$ varies run to run, but
  the new-table probability $\alpha/(n+\alpha)$ at step 11 does not depend on that pattern — do
  not condition on a specific occupancy pattern when aggregating across the 2000 runs.

**Debugging tip:** for a tiny sanity check, run `crp_sample(n=3, alpha=1e6, rng)` (huge $\alpha$)
and confirm nearly every customer starts a new table (3 tables, each with 1 customer) — this
isolates a basic correctness check of the new-table branch independent of the join-existing-table
branch.

**Instructor tip:** have students derive $\mathbb{E}[K_{10}]$ by hand by summing
$\sum_{j=0}^{9}\alpha/(j+\alpha)$ for $\alpha=2$ and compare it to their Task B simulation at
$n=10$ specifically (a small enough $n$ to compute the exact sum by hand) before trusting the
$n=2000$, $\alpha\ln n$ asymptotic approximation.
