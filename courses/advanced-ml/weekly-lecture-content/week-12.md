# Week 12 — Lecture Content: Robust Statistics

## 1. The Sample Mean's Fragility
The sample mean $\bar X = \frac1n\sum_i X_i$ has **breakdown point** $1/n$: a single
arbitrarily-large corrupted observation can drag $\bar X$ to an arbitrary value (replace $X_1$
with $M\to\infty$; $\bar X\to\infty$ too). Even without adversarial corruption, if the data is
merely **heavy-tailed** (finite variance $\sigma^2$ but not sub-Gaussian — e.g., no higher moments
controlled), Chebyshev's inequality is the *best* concentration guarantee available for
$\bar X$ (sub-Gaussian-style exponential concentration, as in Weeks 2–3, requires sub-Gaussian or
at least sub-exponential tails, which heavy-tailed data does not have), giving only
$\mathbb{P}(|\bar X-\mu|\geq t) \leq \sigma^2/(nt^2)$ — a $1/\delta$ (not $\log(1/\delta)$)
dependence on the failure probability $\delta$, far weaker than the sub-Gaussian-type
$\log(1/\delta)$ dependence enjoyed under lighter tails. Both motivate robust mean estimation.

## 2. The Trimmed Mean
The **trimmed mean** discards the top and bottom $\epsilon$-fraction of the order statistics and
averages what remains:
```
trimmed_mean = (1/(n(1−2ε))) · Σ_{i: X_(i) is among the middle (1−2ε)-fraction} X_(i)
```
Its **breakdown point is $\epsilon$**: up to an $\epsilon$-fraction of arbitrarily corrupted points
can be tolerated without the estimate becoming unbounded, since any corrupted point that lands in
the discarded tails has no effect at all, and a corrupted point that lands in the kept middle is
still bounded by the (uncorrupted) order statistics on either side of it.

## 3. Median-of-Means: Construction
Partition $n$ i.i.d. samples (mean $\mu$, finite variance $\sigma^2$) into $k$ disjoint groups of
size $m=n/k$ each. Compute each group's sample mean $\bar X_1,\dots,\bar X_k$, and return
```
median-of-means = median( X̄_1, …, X̄_k )
```

## 4. Deriving the Median-of-Means Error Bound
**Step 1 — per-group concentration (Chebyshev).** Each group mean $\bar X_j$ has
$\mathrm{Var}(\bar X_j)=\sigma^2/m$, so by Chebyshev's inequality, for any $c>0$,
```
P( |X̄_j − μ| ≥ c·σ/√m ) ≤ 1/c²
```
Choosing $c=2$ (say), $\mathbb{P}(|\bar X_j - \mu| \geq 2\sigma/\sqrt m) \leq 1/4$ — i.e., each
individual group mean is "good" (within $2\sigma/\sqrt m$ of $\mu$) with probability **at least
$3/4$**, a fixed constant, regardless of $n$ or $\delta$ (this constant-probability guarantee is
all Chebyshev ever gives for a single group, however the group is sized).

**Step 2 — boosting via a majority vote across $k$ independent groups.** Let $Z_j =
\mathbf 1\{|\bar X_j - \mu|\geq 2\sigma/\sqrt m\}$ be the indicator that group $j$ is "bad."
$Z_1,\dots,Z_k$ are independent (disjoint, independent groups) with $\mathbb{E}[Z_j]\leq 1/4$. If
the median-of-means estimate is *not* within $2\sigma/\sqrt m$ of $\mu$, then **more than half**
the groups must be bad, i.e. $\sum_j Z_j > k/2$ (this is exactly why the median, not the mean, of
the group means is used: the median is only pulled outside the good groups' range if a majority
of groups are bad). Since each $Z_j$ is a Bernoulli trial with success probability $\leq 1/4 <
1/2$, a Chernoff/Hoeffding bound (directly reusing the graduate course's concentration toolkit,
now applied to this bounded $\{0,1\}$-valued sequence) gives
```
P( Σ_j Z_j > k/2 ) ≤ exp( −k·D(1/2 ‖ 1/4) )  ≤  exp(−ck)
```
for an explicit constant $c>0$ (from a standard Chernoff bound for a sum of independent Bernoullis
with mean $\leq1/4$ exceeding $k/2$; the key point is this probability decays **exponentially** in
the number of groups $k$, exactly the Hoeffding-style behavior assumed from the graduate course,
now bounding the number of "votes," not the quantity of interest directly).

**Step 3 — choosing $k$ and combining.** Setting $k = O(\log(1/\delta))$ makes
$\exp(-ck)\leq\delta$, so with probability $\geq 1-\delta$, the median-of-means estimate is within
$2\sigma/\sqrt m = 2\sigma\sqrt{k/n}$ of $\mu$ (since $m=n/k$). Substituting
$k=O(\log(1/\delta))$:
```
| median-of-means − μ |  =  O( σ · √( log(1/δ) / n ) )     with probability ≥ 1 − δ
```
**This is a sub-Gaussian-type confidence interval — $\sqrt{\log(1/\delta)}$, not
$\sqrt{1/\delta}$, dependence on the failure probability — achieved under only a finite-variance
assumption**, strictly weaker than sub-Gaussianity. This is strictly better than the sample mean's
Chebyshev-only guarantee (which only gives $O(\sigma/\sqrt{n\delta})$, the much worse $1/\delta$
dependence) whenever the underlying data is heavy-tailed rather than sub-Gaussian — the sample
mean cannot match this rate under only a finite-variance assumption, which is exactly why
median-of-means is a genuinely different and better tool in this regime, not merely a cosmetic
variant.

## 5. Behavior Under Adversarial Contamination
If an adversary corrupts (arbitrarily) an $\epsilon$-fraction of the $n$ samples, at most
$\epsilon n = \epsilon k m$ corrupted points are distributed among the $k$ groups; an adversary
acting optimally corrupts at most $\epsilon k$ entire groups to maximize the number of "poisoned"
group means (concentrating corruption in as few groups as possible is actually the adversary's
*worst* lever here if the goal is to corrupt as many groups as possible — spreading corrupted
points across many groups to flip more group means with fewer points per group is typically more
effective, but even in the best case for the adversary, at most $\epsilon k$ groups can have their
mean driven arbitrarily). Provided $\epsilon$ is bounded well below $1/2$, the uncorrupted
majority of groups still concentrates as in §4, and the median remains within
$O(\sigma(\sqrt{\log(1/\delta)/n} + \sqrt\epsilon))$ of $\mu$ — a **bounded-error guarantee that
degrades gracefully in $\epsilon$**, in sharp contrast to the sample mean, whose error under the
same $\epsilon$-contamination model is **unbounded** (a single corrupted point suffices, let alone
an $\epsilon n\geq1$ fraction). The trimmed mean (§2) gives an analogous bounded-error guarantee
directly from its $\epsilon$ breakdown point.

## 6. Conceptual Connection to Adversarial Robustness in Modern ML
Both adversarial examples (small, worst-case perturbations of individual inputs that flip a
model's prediction) and data poisoning (an adversary corrupting a fraction of the *training* set)
are, at a structural level, instances of the same **worst-case contamination** problem robust
statistics studies: a learning procedure that implicitly behaves like an uncorrected sample mean
(or an uncorrected empirical-risk minimizer) over its inputs/training data inherits the sample
mean's unbounded sensitivity to a worst-case perturbation. Robust-statistics-style
estimators — trimmed or winsorized losses, median-of-means gradient estimation during training —
are an active line of research aiming to give learning algorithms the same kind of
bounded-error-under-contamination guarantee this week derives for mean estimation specifically.
This connection is **conceptual and structural**, not a claim that median-of-means mean estimation
*is* adversarial-example defense; a full treatment of adversarial robustness in deep learning is
out of this course's scope (it belongs to *Advanced Deep Learning*).

## 7. Python: Trimmed Mean and Median-of-Means Under Contamination
```python
import numpy as np

rng = np.random.default_rng(0)

def trimmed_mean(x, eps):
    x_sorted = np.sort(x)
    n = len(x)
    lo, hi = int(np.floor(eps * n)), int(np.ceil((1 - eps) * n))
    return x_sorted[lo:hi].mean()

def median_of_means(x, k):
    groups = np.array_split(rng.permutation(x), k)
    return np.median([g.mean() for g in groups])

def contaminate(x, eps, corruption_value, rng):
    x = x.copy()
    n_corrupt = int(eps * len(x))
    idx = rng.choice(len(x), size=n_corrupt, replace=False)
    x[idx] = corruption_value
    return x

mu, sigma = 5.0, 2.0
n = 5000

# Heavy-tailed case: Pareto-adjacent (finite variance, heavy tail) data
x_heavy = mu + (rng.pareto(a=3.0, size=n) - 1.0 / 2.0) * sigma

print("Heavy-tailed data (no contamination):")
print(f"  sample mean      error = {abs(x_heavy.mean() - mu):.4f}")
print(f"  trimmed mean(.1) error = {abs(trimmed_mean(x_heavy, 0.1) - mu):.4f}")
print(f"  median-of-means(k=25) error = {abs(median_of_means(x_heavy, 25) - mu):.4f}")

# Adversarial contamination case: Gaussian data, eps-fraction set to an extreme value
x_gauss = rng.normal(mu, sigma, size=n)
x_contam = contaminate(x_gauss, eps=0.05, corruption_value=1e6, rng=rng)

print("\nGaussian data with 5% adversarial contamination:")
print(f"  sample mean      error = {abs(x_contam.mean() - mu):.4f}")
print(f"  trimmed mean(.1) error = {abs(trimmed_mean(x_contam, 0.1) - mu):.4f}")
print(f"  median-of-means(k=25) error = {abs(median_of_means(x_contam, 25) - mu):.4f}")
```
Under contamination, the sample mean's error should be enormous (dominated by the corrupted
value), while the trimmed mean and median-of-means errors should remain small and comparable to
their uncontaminated-case values — the direct empirical confirmation of §5's bounded-error claim.

## 8. In-Class/Lab Exercise
Sweep the contamination fraction $\epsilon \in \{0.01, 0.05, 0.1, 0.2, 0.4\}$ (with
`corruption_value=1e6`) and plot sample-mean, trimmed-mean, and median-of-means error against
$\epsilon$ on a log scale for the sample-mean error. Identify the $\epsilon$ at which the
trimmed mean (breakdown point exactly $0.1$ in the §7 code) itself starts to fail, and relate this
threshold directly to its defined breakdown point.
