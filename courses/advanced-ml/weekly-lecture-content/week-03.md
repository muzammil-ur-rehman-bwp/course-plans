# Week 3 — Lecture Content: High-Dimensional Statistics I — Concentration Beyond Hoeffding

## 1. Why Hoeffding Is Not Enough
The graduate course's Hoeffding's inequality applies only to **bounded** independent random
variables. High-dimensional statistics routinely needs concentration for variables that are
unbounded but still well-behaved — Gaussian noise, linear/quadratic functions of Gaussian vectors,
and sample covariances. These need a strictly more general toolkit: sub-Gaussian and
sub-exponential random variables.

## 2. Sub-Gaussian Random Variables
A (centered, i.e. mean-zero) random variable $X$ is **sub-Gaussian with parameter $\sigma$** if
its moment-generating function (MGF) is dominated by a Gaussian's:
```
E[exp(λX)] ≤ exp(λ²σ²/2)      for all λ ∈ ℝ
```
This is exactly the MGF bound a $\mathcal N(0,\sigma^2)$ variable satisfies with equality, so
"sub-Gaussian" means "MGF at most as large as a Gaussian's at every $\lambda$." A standard
Chernoff-bound argument converts this MGF bound into a tail bound: for $t>0$,
```
P(X ≥ t) = P(exp(λX) ≥ exp(λt)) ≤ E[exp(λX)] / exp(λt) ≤ exp(λ²σ²/2 − λt)
```
Minimizing the right-hand side over $\lambda$ (set $\lambda = t/\sigma^2$) gives
```
P(X ≥ t) ≤ exp(−t²/(2σ²)),      and by symmetry  P(|X| ≥ t) ≤ 2 exp(−t²/(2σ²))
```
— genuinely Gaussian-type tail decay, for a class of variables much larger than Gaussians
themselves.

**Bounded ⟹ sub-Gaussian (Hoeffding's lemma).** If $X\in[a,b]$ almost surely with mean 0, then $X$
is sub-Gaussian with parameter $\sigma = (b-a)/2$. This is precisely the lemma underlying the
graduate course's Hoeffding's inequality — it shows Hoeffding's inequality is the *special case*
of the sub-Gaussian tail bound applied to bounded variables, not a separate phenomenon. The
sub-Gaussian framework strictly generalizes it: Gaussian variables themselves, bounded variables,
and Rademacher ($\pm1$) variables are all sub-Gaussian, and sums of independent sub-Gaussian
variables are again sub-Gaussian with parameter $\sqrt{\sum_i \sigma_i^2}$ (via MGF
multiplicativity across independent variables), so the same tail bound applies directly to
averages $\frac1n\sum_i X_i$, exactly as Hoeffding's inequality does for bounded variables, but
now for any sub-Gaussian population.

## 3. Sub-Exponential Random Variables
Many important variables are *not* sub-Gaussian: e.g., $X = Z^2 - 1$ for $Z\sim\mathcal N(0,1)$
(a centered $\chi^2_1$ variable). Its MGF, $\mathbb{E}[e^{\lambda X}] = \frac{e^{-\lambda}}
{\sqrt{1-2\lambda}}$, is only finite for $\lambda < 1/2$ and blows up as $\lambda \to 1/2$ — no
Gaussian-type bound $e^{\lambda^2\sigma^2/2}$ (finite for *every* $\lambda$) can dominate it. This
motivates the **sub-exponential** class: $X$ is sub-exponential with parameters $(\nu,\alpha)$ if
```
E[exp(λX)] ≤ exp(λ²ν²/2)      for all |λ| ≤ 1/α
```
— the same Gaussian-type MGF bound as sub-Gaussian, but now only required to hold in a *bounded
neighborhood* of $\lambda=0$ (width $1/\alpha$), reflecting that the tail is allowed to be heavier
than Gaussian far out. Every sub-Gaussian variable is sub-exponential (with $\alpha\to 0$, i.e. the
MGF bound holding for all $\lambda$), but not conversely — sub-exponential is the strictly larger
class, exactly what is needed for $\chi^2$-type and other squared/quadratic quantities that arise
constantly in high-dimensional statistics (sample variances, quadratic forms, products of
Gaussians).

## 4. Bernstein's Inequality
For a centered sub-exponential variable with parameters $(\nu,\alpha)$, the Chernoff-bound
argument (optimizing $\lambda$ subject to the constraint $|\lambda|\leq 1/\alpha$) gives a tail
bound with **two regimes**:
```
P(X ≥ t) ≤ exp( −t²/(2ν²) )              if 0 ≤ t ≤ ν²/α     (sub-Gaussian-like regime)
P(X ≥ t) ≤ exp( −t/(2α) )                if t > ν²/α          (purely exponential regime)
```
compactly written as the single bound
```
P(X ≥ t) ≤ exp( − (1/2) · min( t²/ν², t/α ) )
```
For a sum $S_n=\sum_{i=1}^n X_i$ of independent, centered sub-exponential variables with common
parameters $(\nu,\alpha)$, the same argument (MGF multiplicativity) gives **Bernstein's
inequality**:
```
P(S_n ≥ t) ≤ exp( − (1/2) · min( t²/(nν²), t/α ) )
```
**Derivation sketch (Chernoff + MGF bound).** As in §2, $\mathbb{P}(S_n\geq t) \leq
e^{-\lambda t}\prod_i \mathbb{E}[e^{\lambda X_i}] \leq e^{-\lambda t + n\lambda^2\nu^2/2}$ for
$|\lambda|\leq 1/\alpha$. Minimizing $-\lambda t + n\lambda^2\nu^2/2$ over unconstrained $\lambda$
gives $\lambda^\star = t/(n\nu^2)$; if $\lambda^\star \leq 1/\alpha$ (i.e. $t \leq n\nu^2/\alpha$)
this is feasible and gives the sub-Gaussian-like bound $e^{-t^2/(2n\nu^2)}$; otherwise the optimal
feasible choice is $\lambda=1/\alpha$ (the constraint boundary), giving the purely exponential
bound $e^{-t/(2\alpha)}$ for large $t$ — exactly the two-regime structure above. **Why two
regimes, intuitively:** near the mean, a sub-exponential variable behaves like a Gaussian
(Bernstein matches Hoeffding/sub-Gaussian-style decay); far in the tail, the heavier true tail
dominates and decay slows to purely exponential — Bernstein's inequality is the honest statement
of exactly where that transition happens (at $t\approx n\nu^2/\alpha$).

## 5. Python: Simulating Sub-Gaussian and Sub-Exponential Concentration
```python
import numpy as np

rng = np.random.default_rng(0)

def empirical_tail(samples_matrix, t):
    """samples_matrix: shape (n_trials, n). Returns P(mean(row) >= t) empirically."""
    means = samples_matrix.mean(axis=1)
    return np.mean(means >= t)

n, n_trials = 200, 50000

# Sub-Gaussian case: Rademacher (+-1) variables, sigma = 1.
rad = rng.choice([-1.0, 1.0], size=(n_trials, n))
# Sub-exponential case: centered chi-square_1 variables (Z^2 - 1), heavier tail.
chi2 = rng.standard_normal((n_trials, n)) ** 2 - 1.0

for t in [0.1, 0.3, 0.5, 0.8]:
    emp_rad = empirical_tail(rad, t)
    bound_rad = np.exp(-n * t**2 / 2)                 # sub-Gaussian bound, sigma=1
    emp_chi2 = empirical_tail(chi2, t)
    # Bernstein bound for centered chi^2_1: nu^2=2, alpha=4 (standard chi^2_1 sub-exponential
    # parameters), two-regime bound:
    nu2, alpha = 2.0, 4.0
    bound_chi2 = np.exp(-n * min(t**2 / (2 * nu2), t / (2 * alpha)))
    print(f"t={t:.2f}  Rademacher: emp={emp_rad:.4f} bound={bound_rad:.4f}  |  "
          f"chi2-1: emp={emp_chi2:.4f} bound={bound_chi2:.4f}")
```
In every row, the empirical tail probability should sit at or below its corresponding theoretical
bound — confirming both the sub-Gaussian bound (Rademacher case) and the two-regime Bernstein
bound (sub-exponential $\chi^2_1$ case) are valid, and that the $\chi^2_1$ case needs the heavier,
Bernstein-style bound rather than a sub-Gaussian one (a sub-Gaussian bound applied to $\chi^2_1$
would be violated at large $t$, which the exercise below asks you to confirm directly).

## 6. In-Class/Lab Exercise
Apply the *sub-Gaussian* bound $e^{-nt^2/2}$ (incorrectly) to the centered $\chi^2_1$ data from
§5's simulation at $t=1.0$ and $t=1.5$, compare it against the empirical tail probability, and
confirm the sub-Gaussian bound is violated (too optimistic) at these larger $t$ values — then
confirm Bernstein's two-regime bound is not violated at the same $t$ values. This is the concrete
demonstration of why sub-exponential variables genuinely need Bernstein's inequality rather than a
sub-Gaussian-style bound.
