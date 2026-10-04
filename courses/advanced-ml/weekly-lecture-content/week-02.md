# Week 2 — Lecture Content: Minimax Lower Bounds

## 1. The Minimax Risk Framework
Let $\Theta$ be a parameter class and let $X \sim P_\theta$ be data generated from the true
parameter $\theta\in\Theta$. For an estimator $\hat\theta = \hat\theta(X)$ and a loss/distance
$d(\cdot,\cdot)$, the **minimax risk** is
```
M_n = inf_{θ̂}  sup_{θ∈Θ}  E_θ[ d(θ̂(X), θ) ]
```
the best possible worst-case risk, over *all* estimators, against the *worst* parameter in
$\Theta$. This is a fundamentally different question from analyzing one specific estimator's risk
(an **upper bound**, e.g., "the sample mean has risk $O(1/\sqrt n)$"). A **lower bound** on $M_n$
says no estimator — not just the one you analyzed — can do better than some rate, in the worst
case over $\Theta$. Only when a matching upper and lower bound are both established (same rate in
$n$, up to constants) can an estimator be certified **rate-optimal**. Without the lower bound, an
upper bound alone never rules out that some cleverer estimator achieves a faster rate.

## 2. Fano's Inequality
Consider a Markov chain $\theta \to X \to \hat\theta$, where $\theta$ is drawn uniformly from a
finite set of $M$ hypotheses $\{\theta_1,\dots,\theta_M\}$, $X\sim P_{\theta}$, and $\hat\theta$ is
any estimator (a function of $X$ alone) attempting to recover which hypothesis generated the
data. **Fano's inequality** states: if $\mathbb{P}(\hat\theta \neq \theta) \leq \delta$, then
```
I(θ; X) ≥ (1 − δ) log M − log 2
```
Equivalently, rearranging for $\delta$:
```
δ ≥ 1 − ( I(θ; X) + log 2 ) / log M
```
**Intuition.** $I(\theta;X)$ measures how much the data $X$ actually reveals about which
hypothesis $\theta$ was used. If $M$ is large (many hypotheses to distinguish) but $I(\theta;X)$ is
small (the hypotheses produce data that looks similar), then no estimator — however clever — can
reliably identify $\theta$: the error probability $\delta$ is forced to be large. This converts a
*statistical estimation* question (can $\theta$ be recovered?) into an *information-theoretic*
one (how much information does $X$ carry about $\theta$?), which can be bounded using only the
pairwise closeness (in KL divergence) of the hypotheses' distributions — a purely analytic
quantity, no estimator-specific argument required.

## 3. The Three-Step Minimax Lower Bound Recipe
To lower-bound $M_n$ for a continuous parameter class $\Theta$ (not literally finite), the
standard argument proceeds in three steps:
1. **Reduction to testing.** Choose a finite subset $\{\theta_1,\dots,\theta_M\}\subset\Theta$
   that is a **packing set**: every pair is separated by at least $2\epsilon$ in the loss metric
   $d$, i.e. $d(\theta_i,\theta_j) \geq 2\epsilon$ for $i\neq j$. If an estimator $\hat\theta$ has
   small risk, it must (with the triangle inequality) be able to identify which $\theta_i$ is
   closest to $\hat\theta$ with low error probability — so estimation risk $\geq \epsilon$ is
   implied whenever hypothesis *testing* among the $M$ packed hypotheses fails with probability
   $\geq$ some constant.
2. **Packing-set construction.** Exhibit $M$ hypotheses that are pairwise $2\epsilon$-separated
   in $d$, with $M$ as large as possible for the chosen $\epsilon$ (a larger, denser packing makes
   the resulting bound tighter).
3. **Information bound via KL divergence.** Bound $I(\theta;X)$ from above using the pairwise
   Kullback–Leibler divergences $\mathrm{KL}(P_{\theta_i}\|P_{\theta_j})$ between the packed
   hypotheses' data distributions (a standard fact: $I(\theta;X) \leq \frac{1}{M}\sum_{i,j}
   \mathrm{KL}(P_{\theta_i}\|P_{\theta_j})$, or a simpler uniform bound
   $I(\theta;X)\leq \max_{i,j}\mathrm{KL}(P_{\theta_i}\|P_{\theta_j})$). Plugging this into Fano's
   inequality and solving for the smallest $\epsilon$ at which the right-hand side forces
   $\delta \geq 1/2$ (say) gives the minimax lower bound $M_n = \Omega(\epsilon)$.

## 4. Worked Example: The Gaussian Location Family
Let $X_1,\dots,X_n \overset{iid}{\sim} \mathcal N(\theta,1)$ for $\theta\in\mathbb{R}$, and take
$d(\theta,\theta')=|\theta-\theta'|$. We lower-bound the minimax risk of estimating $\theta$ from
$n$ samples.

**Packing set ($M=2$, the simplest case).** Take $\theta_1 = -\epsilon$, $\theta_2=+\epsilon$, so
$d(\theta_1,\theta_2)=2\epsilon$ — a valid 2-point packing at separation $2\epsilon$. The sample
mean $\bar X = \frac1n\sum_i X_i$ is sufficient, and $\bar X \mid \theta \sim \mathcal
N(\theta, 1/n)$, so the data (reduced to the sufficient statistic) is
$\bar X\sim\mathcal N(\theta_1,1/n)$ or $\mathcal N(\theta_2,1/n)$.

**KL divergence.** For two univariate Gaussians with the same variance $1/n$ and means
$\theta_1,\theta_2$:
```
KL(N(θ₁,1/n) ‖ N(θ₂,1/n)) = n (θ₁ − θ₂)² / 2 = n (2ε)² / 2 = 2nε²
```

**Fano, applied to $M=2$.** With $M=2$, $I(\theta;X) \leq \mathrm{KL}(P_{\theta_1}\|P_{\theta_2}) =
2n\epsilon^2$ (the bound is exact for 2 hypotheses up to this standard mutual-information-vs-KL
inequality). Fano's inequality gives
```
δ ≥ 1 − (I(θ;X) + log 2) / log M = 1 − (2nε² + log 2) / log 2
```
We want to choose the *largest* $\epsilon$ for which this forces $\delta \geq 1/2$ (any fixed
constant works; $1/2$ is conventional), i.e. $2n\epsilon^2 + \log 2 \leq \tfrac12 \log 2$, which
holds once $\epsilon^2 \leq \frac{\log 2}{4n}$, i.e.
```
ε = Θ(1/√n)
```
**Conclusion.** Since $d(\theta_1,\theta_2)=2\epsilon=\Theta(1/\sqrt n)$ and any estimator with
risk $<\epsilon$ would, by the triangle inequality, let us test between $\theta_1,\theta_2$ with
error probability $<1/2$ — contradicting the Fano bound just derived — we conclude
```
M_n = inf_θ̂ sup_θ E_θ|θ̂ − θ| = Ω(1/√n)
```
This matches, up to constants, the sample mean's known risk upper bound $\mathbb{E}|\bar X -
\theta| = O(1/\sqrt n)$ (it is sub-Gaussian with parameter $1/\sqrt n$, by the graduate course's
Hoeffding-style concentration — here applied to a Gaussian rather than a bounded variable, a
preview of Week 3's sub-Gaussian framework). **The sample mean is therefore minimax rate-optimal
for the Gaussian location family**: no estimator can achieve a faster-than-$1/\sqrt n$ worst-case
rate, and the sample mean already achieves that rate.

## 5. Python: An Empirical Fano-Bound Sanity Check
This simulation does not "verify" Fano's inequality (it is a theorem), but empirically confirms
that no reasonable estimator beats the derived $\Omega(1/\sqrt n)$ rate on the hardest (packed)
instances, and that the predicted $\sqrt n$ scaling of the achievable testing/estimation error
actually appears in simulation.

```python
import numpy as np

def two_point_test_error(n, eps, n_trials=20000, rng=None):
    """Simulate the 2-point testing problem: theta in {-eps, +eps}, n iid N(theta,1) samples.
    The Bayes-optimal test (by symmetry) is: predict +eps if sample mean > 0, else -eps.
    Returns the empirical error probability."""
    rng = rng or np.random.default_rng(0)
    thetas = rng.choice([-eps, eps], size=n_trials)
    sample_means = thetas + rng.standard_normal((n_trials, n)).mean(axis=1)
    preds = np.where(sample_means > 0, eps, -eps)
    return np.mean(preds != thetas)

# Fix eps at the theoretically critical scale eps = sqrt(log(2)/(4n)) and confirm the
# resulting error probability sits near the Fano-predicted regime (~1/2) at the boundary,
# and drops sharply once eps is scaled up by a constant factor (easier separation).
for n in [50, 200, 800]:
    eps_critical = np.sqrt(np.log(2) / (4 * n))
    err_at_boundary = two_point_test_error(n, eps_critical)
    err_easier = two_point_test_error(n, 3 * eps_critical)
    print(f"n={n:4d}  eps*={eps_critical:.4f}  err(eps*)={err_at_boundary:.3f}  "
          f"err(3*eps*)={err_easier:.3f}")
```
At the critical separation $\epsilon^\star=\sqrt{\log 2/(4n)}$ the empirical error should sit well
above 0 (consistent with Fano's $\delta \geq 1/2$-type prediction at this boundary), while at
$3\epsilon^\star$ it should already be small — demonstrating concretely that $\Theta(1/\sqrt n)$
really is the separation scale at which estimation/testing becomes hard-vs-easy, exactly the rate
the Fano argument derived.

## 6. In-Class/Lab Exercise
Repeat the §4 derivation for a 4-point packing set $\theta_k = \epsilon \cdot k$ for
$k\in\{-1.5,-0.5,0.5,1.5\}$ (equally spaced, pairwise separation $\geq \epsilon$), using the
uniform KL bound $I(\theta;X)\leq \max_{i,j}\mathrm{KL}(P_{\theta_i}\|P_{\theta_j})$, and confirm
the resulting minimax rate is still $\Theta(1/\sqrt n)$ (only the constant changes, not the rate) —
the point of this exercise is that increasing $M$ typically does not change the *rate* for a
fixed-dimensional parameter, but will matter decisively once Week 3–4 generalize to
high-dimensional parameter vectors.
