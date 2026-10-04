# Week 8 — Lecture Content: Scaling Laws; Midterm Review

## 1. The Empirical Power-Law Form
A large body of empirical work, broadly associated with Kaplan et al.'s large-scale study, finds
that a trained model's test loss $L$ follows an approximate power law in each of model size $N$
(parameter count), dataset size $D$, and training compute $C$, when the other two are not the
limiting factor:
$$
L(N) \approx \left(\frac{N_c}{N}\right)^{\alpha_N}, \qquad
L(D) \approx \left(\frac{D_c}{D}\right)^{\alpha_D}, \qquad
L(C) \approx \left(\frac{C_c}{C}\right)^{\alpha_C},
$$
for fitted constants $N_c, D_c, C_c$ and fitted exponents $\alpha_N,\alpha_D,\alpha_C$ (typically
small positive numbers, often well under $1$), remarkably stable across many orders of magnitude
of $N$, $D$, and $C$ within a given model family and data domain. Equivalently, on a log-log plot
of loss against size, the relationship is close to a straight line with slope $-\alpha$ — a
striking degree of empirical regularity for a quantity (test loss of a highly non-convex, deeply
nonlinear training procedure) with no a priori reason to be so simply predictable.

## 2. Compute-Optimal Scaling
An early, natural reading of the $N$-scaling result alone might suggest "scale up model size as
much as possible for a fixed compute budget." The refinement broadly attributed to Hoffmann et
al. complicates this: for a **fixed compute budget** $C$, loss is not minimized by maximizing $N$
alone — because larger models trained on relatively too little data saturate due to the data-size
bottleneck in the formulas above — but by scaling model size $N$ and dataset size $D$ **together**
according to a fitted joint relationship, with both growing at comparable rates as the compute
budget increases. This refinement was influential in shifting large-scale training practice toward
jointly scaling data and model size rather than model size alone, and is a clean example of a
purely empirical, curve-fitting result nonetheless changing how practitioners allocate a genuinely
scarce resource (compute).

## 3. Theoretical Attempts to Explain Why Scaling Laws Hold
That loss decreases is unsurprising; that it decreases as a *clean power law*, stable across many
orders of magnitude, is the fact requiring explanation, and this remains only partially understood:
- **Data-manifold / intrinsic-dimension arguments.** If the data effectively lies on (or near) a
  lower-dimensional manifold of intrinsic dimension $d$, classical nonparametric-regression-style
  arguments predict error decaying as a power of $D$ with an exponent tied to $d$ — offering a
  plausible, partially explanatory account of the data-scaling exponent $\alpha_D$, though
  real-world intrinsic dimensionality is hard to measure directly and the fit to observed exponents
  is only approximate.
- **Random-feature / kernel-theoretic connections.** Treating a wide network in its NTK/kernel
  regime (this course's own Week 2 material) as performing kernel regression against a fixed
  kernel lets one import classical kernel-regression error-decay theory, which under certain
  (strong, not fully realistic) assumptions on the kernel's eigenvalue spectrum also predicts
  power-law error decay in both model size (number of random features) and data size — connecting
  the empirical scaling-law phenomenon back to this course's own NTK and mean-field material,
  while still falling short of a complete, assumption-free first-principles derivation of the
  specific exponents observed in practice for real, finite-width, feature-learning networks.
- **Honest status.** No currently available theory derives the specific observed exponents
  $\alpha_N,\alpha_D,\alpha_C$ from first principles for realistic architectures and data
  distributions; the power-law form itself is far better empirically established than any single
  proposed explanation for why that specific functional form should hold.

## 4. Code: Fitting a Power Law to Loss-vs-Size Data
```python
import numpy as np

def fit_power_law(sizes, losses):
    """loss ~ (N_c / N)^alpha  <=>  log(loss) = alpha*log(N_c) - alpha*log(N)
    A simple linear fit in log-log space recovers alpha (the slope) and log(N_c)."""
    log_n, log_l = np.log(sizes), np.log(losses)
    A = np.vstack([log_n, np.ones_like(log_n)]).T
    slope, intercept = np.linalg.lstsq(A, log_l, rcond=None)[0]
    alpha = -slope
    n_c = np.exp(intercept / alpha)
    return alpha, n_c

rng = np.random.default_rng(0)
sizes = np.array([1e5, 3e5, 1e6, 3e6, 1e7, 3e7, 1e8])
true_alpha, true_nc = 0.08, 5e9
noise = rng.normal(0, 0.01, size=sizes.shape)
losses = (true_nc / sizes) ** true_alpha * np.exp(noise)

fitted_alpha, fitted_nc = fit_power_law(sizes, losses)
print(f"True alpha={true_alpha:.3f}  Fitted alpha={fitted_alpha:.3f}")
print(f"True N_c={true_nc:.3e}      Fitted N_c={fitted_nc:.3e}")

for n in sizes:
    pred = (fitted_nc / n) ** fitted_alpha
    print(f"N={n:.0e}  predicted loss={pred:.4f}")
```
The fitted exponent should recover `true_alpha` closely despite the added noise, illustrating how
a scaling-law exponent is read off real data in practice (fit a line in log-log space).

## 5. Midterm Review (Weeks 1–8)
Review, at the level of "explain and critique," not just "state": the Week 2 NTK linearization
argument and lazy-training condition; the Week 3 mean-field limit and its contrast with NTK,
including the variance/correlation signal-propagation recursion; the Week 4 max-margin implicit-
bias derivation and its loss-tail dependence; the Week 5 mechanisms and signatures of feature
learning; the Week 6 sharpness measures, the reparameterization critique, and the SAM derivation;
the Week 7 grokking phenomenon and its two competing hypotheses; this week's scaling-law form and
theoretical attempts to explain it. The midterm (Week 9) is qualifying-exam style: expect to be
asked not just "what is the result" but "what does it establish, and what does it not."

## 6. In-Class Exercise
A paper reports a scaling-law fit with $\alpha_N = 0.34$ on one task and $\alpha_N = 0.09$ on
another. Without additional information, state one plausible (not necessarily correct) hypothesis
for why the two tasks might have such different exponents, drawing on §3's data-manifold argument.
