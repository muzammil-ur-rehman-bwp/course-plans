# Week 6 — Lecture Content: Nonparametric Bayesian Methods I — The Dirichlet Process

## 1. Motivation: Beyond Fixed-$K$ Mixtures
A finite Gaussian mixture model (assumed background from model-selection theory) requires fixing
the number of components $K$ in advance, then uses AIC/BIC or cross-validation to choose among a
finite menu of $K$ values. A **nonparametric** Bayesian alternative instead places a prior
directly over the space of *all* possible discrete probability distributions (equivalently, over
mixtures with an unbounded number of components), letting the effective number of components used
by any finite dataset be inferred rather than fixed a priori.

## 2. The Dirichlet Process: Defining Property
A **Dirichlet process** $\mathrm{DP}(\alpha, H)$ is a distribution over probability distributions
on a sample space $\mathcal X$, parameterized by a **base distribution** $H$ (a probability
distribution on $\mathcal X$, playing the role of a "prior mean" distribution) and a
**concentration parameter** $\alpha>0$. A random distribution $G \sim \mathrm{DP}(\alpha,H)$ is
defined by: for *every* finite measurable partition $A_1,\dots,A_k$ of $\mathcal X$,
```
( G(A_1), …, G(A_k) )  ~  Dirichlet( α H(A_1), …, α H(A_k) )
```
This is a direct infinite-dimensional generalization of the ordinary (graduate-course-adjacent)
Dirichlet distribution: instead of a fixed-length probability vector drawn from a Dirichlet
distribution, $G$ is an entire random *distribution*, consistent (in the above sense) across every
possible finite partition simultaneously. Two structural facts follow immediately from this
definition: $\mathbb{E}[G(A)] = H(A)$ for every $A$ (so $H$ is indeed the "mean" distribution), and
$G$ is almost surely a discrete distribution (even when $H$ is continuous) — a non-obvious
consequence that the stick-breaking construction makes completely explicit.

## 3. The Stick-Breaking Construction
Sethuraman's stick-breaking representation gives an explicit, constructive way to draw
$G\sim\mathrm{DP}(\alpha,H)$:
1. Draw $\beta_k \overset{iid}{\sim} \mathrm{Beta}(1,\alpha)$ for $k=1,2,3,\dots$ (independently).
2. Define weights by **breaking a unit-length "stick"** repeatedly: the first weight takes a
   $\beta_1$-fraction of the whole stick, $\pi_1 = \beta_1$; the second weight takes a
   $\beta_2$-fraction of what remains, $\pi_2 = \beta_2(1-\beta_1)$; in general,
   ```
   π_k = β_k · Π_{j=1}^{k-1} (1 − β_j)
   ```
3. Draw atom locations $\theta_k \overset{iid}{\sim} H$, independently of the $\beta_k$'s.
4. Set $G = \sum_{k=1}^\infty \pi_k \,\delta_{\theta_k}$ (a weighted sum of point masses, i.e. an
   almost-surely-discrete distribution, regardless of whether $H$ itself is continuous).

**Why the weights sum to 1.** Let $R_k = \prod_{j=1}^{k}(1-\beta_j)$ be the fraction of stick
remaining after $k$ breaks (with $R_0=1$). Then $\pi_k = R_{k-1} - R_k$ directly (since
$R_{k-1}-R_k = R_{k-1}(1-(1-\beta_k)) = R_{k-1}\beta_k = \pi_k$), so
$\sum_{k=1}^{K}\pi_k = R_0 - R_K = 1 - R_K$, and $R_K = \prod_{j=1}^K(1-\beta_j) \to 0$ almost
surely as $K\to\infty$ (since each factor $\mathbb{E}[1-\beta_j] = \alpha/(1+\alpha) < 1$, so
$R_K\to0$ a.s. by a standard product-of-i.i.d.-sub-unity-factors argument — formally, $\log R_K =
\sum_j \log(1-\beta_j)$ is a sum of i.i.d. strictly negative-mean terms, so $\log R_K\to-\infty$
a.s. by the strong law of large numbers). Hence $\sum_{k=1}^\infty \pi_k = 1 - \lim_K R_K = 1$
**almost surely** — a clean telescoping argument showing the construction genuinely produces a
valid (normalized) random probability distribution. (That this $G$ in fact satisfies the §2
finite-partition defining property is Sethuraman's theorem; the proof is beyond this course's
scope, but the construction above is fully explicit and directly usable.)

## 4. The Role of the Concentration Parameter $\alpha$
Since $\beta_k \sim \mathrm{Beta}(1,\alpha)$ has mean $\mathbb{E}[\beta_k] = 1/(1+\alpha)$:
- **Small $\alpha$** (e.g., $\alpha=0.1$): $\mathbb{E}[\beta_1]$ is close to 1 — the *first* break
  typically claims almost the entire stick, so a small number of atoms dominate the mass of $G$
  (few effective clusters).
- **Large $\alpha$** (e.g., $\alpha=10$): $\mathbb{E}[\beta_k]$ is close to 0 — each break claims
  only a small fraction, so mass is spread thinly across many atoms before it is exhausted, and
  $G$ approaches $H$ itself (many effective clusters, each contributing little weight).
$\alpha$ is therefore the single hyperparameter directly controlling how many clusters a DP
mixture model is likely to actually use for a given amount of data — this is made fully precise
by the Chinese Restaurant Process view in Week 7.

## 5. Python: A Truncated Stick-Breaking Sampler
A DP has infinitely many atoms, but any finite truncation captures all but a vanishingly small
tail of the mass (since $R_K\to0$), so simulation uses a large truncation level $K$.

```python
import numpy as np

def stick_breaking(alpha, K, base_sampler, rng):
    """Returns K weights (summing to <= 1, with the remainder as negligible tail mass for
    large K) and K atom locations drawn from base_sampler(K, rng)."""
    betas = rng.beta(1, alpha, size=K)
    remaining = np.cumprod(np.concatenate([[1.0], 1 - betas[:-1]]))
    weights = betas * remaining
    atoms = base_sampler(K, rng)
    return weights, atoms

rng = np.random.default_rng(0)
base_sampler = lambda K, rng: rng.normal(loc=0.0, scale=3.0, size=K)   # H = N(0, 9)

for alpha in [0.5, 2.0, 10.0]:
    weights, atoms = stick_breaking(alpha, K=1000, base_sampler=base_sampler, rng=rng)
    print(f"alpha={alpha:5.1f}  sum(weights)={weights.sum():.5f}  "
          f"top-5 weights={np.sort(weights)[::-1][:5].round(3)}  "
          f"effective #atoms (weight>0.01)={np.sum(weights > 0.01)}")
```
At small $\alpha$, expect a handful of large weights dominating; at large $\alpha$, expect many
comparably small weights and a visibly larger count of atoms exceeding the 0.01 threshold —
directly confirming §4's qualitative claim.

## 6. In-Class/Lab Exercise
Draw $G\sim\mathrm{DP}(\alpha,H)$ via truncated stick-breaking for $\alpha\in\{0.5,2,10\}$ with
$H=\mathcal N(0,9)$, generate $n=500$ samples from each $G$ (sample an atom index according to the
weights, then return that atom's location), and plot a histogram of the samples for each $\alpha$.
Confirm visually that small $\alpha$ produces a few sharp spikes (few effective cluster locations
repeated often) while large $\alpha$ produces a histogram shape closer to a smooth approximation
of $H$ itself.
