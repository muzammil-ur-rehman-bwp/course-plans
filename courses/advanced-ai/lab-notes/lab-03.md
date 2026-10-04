# Lab Notes 3 — UCB1 vs. ε-Greedy

**Concept recap:** UCB1's confidence radius √(2 ln t / nᵢ(t)) comes from the Hoeffding bound;
this gives UCB1 expected regret O((K log T)/Δ_min), vs. ε-greedy's regret, which grows linearly
in the long run because it keeps exploring at a constant rate forever.

**Common pitfalls:**
- Forgetting to pull each arm once before computing UCB indices (division by zero when
  nᵢ(t) = 0) — initialize by pulling each arm exactly once in the first K rounds, as in the
  lecture-content implementation.
- Using too few random seeds in Task B/C — bandit simulations are highly stochastic, and 5–10
  seeds is not enough to see a clean regret-curve shape; 50 seeds is the minimum the manual
  specifies for a reason.
- Mistaking a flattening regret curve on a short time horizon for logarithmic growth when it
  could equally be an artifact of too short a horizon — fitting an explicit log(T) curve (Task C)
  and checking the fit quality is more rigorous than eyeballing the plot.
- In the Task D mini-challenge, using a flat (non-informative) Beta(1,1) prior but then forgetting
  to update it with *both* successes and failures (Beta(α + successes, β + failures)) — an
  implementation that only updates on success will not behave like a Bernoulli posterior at all.

**Debugging tip:** before trusting the full comparison, verify UCB1 degenerates sensibly when all
arms have the same true mean — regret should stay near zero regardless of which arm is pulled.

**Instructor tip:** ask students to predict, before running Task C, which algorithm will have
higher regret at T=100 vs. T=5000 — ε-greedy often looks competitive or even better very early on
(before UCB1's confidence bounds have tightened), which is a useful, concrete illustration of why
asymptotic regret bounds do not tell the whole short-horizon story.
