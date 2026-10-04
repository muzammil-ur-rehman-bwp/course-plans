# Week 2 Summary — Minimax Lower Bounds

**Key takeaways:**
- The minimax risk $M_n = \inf_{\hat\theta}\sup_\theta \mathbb{E}_\theta[d(\hat\theta,\theta)]$ is
  the best possible worst-case risk over all estimators; only a matching lower bound certifies an
  estimator's upper bound is rate-optimal.
- Fano's inequality, $I(\theta;X) \geq (1-\delta)\log M - \log 2$, converts estimation-hardness
  into an information-theoretic statement, via the reduction-to-testing + packing-set + KL-bound
  recipe.
- For the Gaussian location family, a 2-point packing set and a KL-divergence bound give a
  $\Omega(1/\sqrt n)$ minimax lower bound, matching the sample mean's $O(1/\sqrt n)$ upper bound.

**You should now be able to:** state the minimax risk framework and Fano's inequality precisely;
construct a packing set and apply the three-step recipe; derive and interpret a matching
upper/lower bound rate.

**Next week:** High-dimensional statistics I — sub-Gaussian and sub-exponential concentration
beyond Hoeffding, and Bernstein's inequality.
