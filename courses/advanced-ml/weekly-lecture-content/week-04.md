# Week 4 — Lecture Content: High-Dimensional Statistics II — Random Matrix Theory Basics

## 1. Why Classical Covariance Theory Breaks Down When $p \approx n$
The graduate course's PCA theory implicitly assumes the classical asymptotic regime: feature
dimension $p$ fixed, sample size $n\to\infty$, under which the sample covariance matrix
$\hat\Sigma = \frac1n X X^\top$ (for a $p\times n$ data matrix $X$) converges entrywise to the
true covariance $\Sigma$, and so do its eigenvalues/eigenvectors. Modern high-dimensional data
(genomics, text embeddings, wide feature sets) routinely has $p$ comparable to $n$, where this
convergence simply fails — not due to insufficient data cleverness, but as a structural fact about
random matrices. Random matrix theory characterizes exactly how it fails.

## 2. The Marchenko–Pastur Law
Let $X$ be a $p\times n$ matrix of i.i.d. mean-zero, unit-variance entries, and suppose
$p,n\to\infty$ with the **aspect ratio** $\gamma = p/n \to \gamma \in (0,\infty)$. Consider the
sample covariance $\hat\Sigma = \frac1n XX^\top$ (a $p\times p$ matrix). If the true covariance
were the identity, one might hope every eigenvalue of $\hat\Sigma$ concentrates near 1. The
**Marchenko–Pastur law** states instead that the **empirical spectral distribution** of
$\hat\Sigma$ (the distribution that puts mass $1/p$ on each eigenvalue) converges almost surely to
a deterministic limiting distribution with density, for $\gamma \leq 1$,
```
f_MP(x) = { 1/(2π γ x) · √((b − x)(x − a))   for a ≤ x ≤ b
          { 0                                  otherwise
```
where $a = (1-\sqrt\gamma)^2$ and $b=(1+\sqrt\gamma)^2$ are the edges of the support (for
$\gamma>1$, an additional point mass of weight $1-1/\gamma$ sits at $x=0$, reflecting that
$\hat\Sigma$ is then rank-deficient — more features than samples).

**Conceptual derivation sketch.** The result follows from a moment-method argument: the
$k$-th moment of the empirical spectral distribution,
$\frac1p\mathrm{Tr}(\hat\Sigma^k) = \frac{1}{p}\sum_i \lambda_i^k$, can be computed (in
expectation, then shown to concentrate) by a combinatorial count of closed walks in a
random-graph-like structure induced by the matrix product $\hat\Sigma^k$; as $p,n\to\infty$ with
$p/n\to\gamma$, these moments converge to the moments of exactly the Marchenko–Pastur
distribution (identified by matching them to the moments of the explicit density above), by the
method of moments for distributional convergence. This is a genuinely different and more delicate
argument than the entrywise LLN-based convergence that works in the classical fixed-$p$ regime;
the full combinatorial proof is beyond this course's scope, but the moment-matching *logic* — why
a deterministic limiting spectral law should exist at all — is the key conceptual takeaway.

## 3. Why This Means Sample Eigenvalues Are Systematically Distorted
Even when the *true* covariance is the identity (every true eigenvalue exactly 1), the
Marchenko–Pastur law says the *sample* covariance's eigenvalues spread out over the entire
interval $[(1-\sqrt\gamma)^2,(1+\sqrt\gamma)^2]$ — not concentrated at 1 at all, for any fixed
$\gamma>0$. For example, at $\gamma=0.5$: support is $[(1-0.707)^2,(1+0.707)^2]\approx[0.086,
2.914]$ — sample eigenvalues can be nearly 3 times the true value, or less than a tenth of it,
purely from sampling noise in the high-dimensional regime, with **no actual signal** at all. This
is the single most important cautionary fact for any method that trusts a high-dimensional sample
covariance matrix's eigenstructure at face value: a graduate-course PCA analysis run naively at
$p\approx n$ will report "large" top principal components and "near-zero" trailing ones even on
pure noise, because that is exactly what the Marchenko–Pastur spectrum looks like — a smooth
spread from near $0$ to a value well above $1$, not a flat spectrum at $1$. Any claim that a
particular sample eigenvalue reflects "real" structure requires checking it against the
Marchenko–Pastur null spectrum's upper edge $b=(1+\sqrt\gamma)^2$ first.

## 4. Boundary Cases
- **$\gamma \to 0$** (classical regime: $p$ fixed, $n\to\infty$): the support interval
  $[(1-\sqrt\gamma)^2,(1+\sqrt\gamma)^2]$ collapses to the single point $\{1\}$, recovering the
  classical result that $\hat\Sigma \to \Sigma = I$ entrywise and hence eigenvalue-wise. The
  Marchenko–Pastur law strictly generalizes, and reduces correctly to, classical covariance theory.
- **$\gamma \to 1$** (as many features as samples): the lower edge $a=(1-\sqrt\gamma)^2 \to 0$ —
  the smallest sample eigenvalue approaches zero, meaning $\hat\Sigma$ becomes nearly singular (its
  condition number blows up), even though the true covariance $\Sigma=I$ is perfectly
  well-conditioned. This is the formal statement of a widely-encountered practical pathology:
  covariance-matrix inversion (e.g., in LDA, whitening, or Mahalanobis-distance computation)
  becomes numerically unstable not from a coding bug but from a structural random-matrix fact once
  $p$ approaches $n$.

## 5. Python: Simulating the Empirical Spectral Distribution
```python
import numpy as np
import matplotlib.pyplot as plt

def mp_density(x, gamma):
    a, b = (1 - np.sqrt(gamma))**2, (1 + np.sqrt(gamma))**2
    x = np.asarray(x, dtype=float)
    out = np.zeros_like(x)
    mask = (x >= a) & (x <= b) & (x > 0)
    out[mask] = np.sqrt(np.clip((b - x[mask]) * (x[mask] - a), 0, None)) / (2 * np.pi * gamma * x[mask])
    return out

rng = np.random.default_rng(0)
n = 2000

fig, axes = plt.subplots(1, 3, figsize=(15, 4))
for ax, gamma in zip(axes, [0.2, 0.5, 0.9]):
    p = int(gamma * n)
    X = rng.standard_normal((p, n))              # true covariance = identity
    Sigma_hat = (X @ X.T) / n
    eigs = np.linalg.eigvalsh(Sigma_hat)
    ax.hist(eigs, bins=60, density=True, alpha=0.6, label="empirical spectrum")
    xs = np.linspace(max(1e-3, (1 - np.sqrt(gamma))**2 - 0.1), (1 + np.sqrt(gamma))**2 + 0.1, 400)
    ax.plot(xs, mp_density(xs, gamma), 'r-', lw=2, label="Marchenko–Pastur density")
    ax.set_title(f"gamma = p/n = {gamma}")
    ax.legend(fontsize=8)
plt.tight_layout()
plt.savefig("marchenko_pastur_fit.png", dpi=120)
```
The empirical histogram should hug the Marchenko–Pastur density closely at each $\gamma$,
including the correctly predicted support edges $a,b$ — and the spread should visibly widen as
$\gamma$ increases toward 1, with the left edge approaching 0.

## 6. In-Class/Lab Exercise
For $\gamma=0.5$ (as in §3), compute the predicted upper support edge $b=(1+\sqrt{0.5})^2$
analytically, then empirically report, from a single simulated $\hat\Sigma$ at $p=250,n=500$ with
true covariance $I$, what fraction of the 250 sample eigenvalues exceed $b$ (should be small/zero,
confirming $b$ is genuinely an edge, not merely a typical value) and confirm the single largest
sample eigenvalue sits close to, but not exceeding, $b$ by more than finite-size fluctuation.
