# Week 9 — Lecture Content: Generalization Theory I (PAC Learning, VC Dimension)

*(Delivered after the Midterm Exam, which covers Weeks 1–8.)*

## 1. The PAC Learning Framework
**Probably Approximately Correct (PAC) learning** formalizes "a hypothesis class $H$ is learnable"
as: there exists a learning algorithm such that, for every target concept, every data
distribution, and every choice of accuracy $\epsilon>0$ and confidence $1-\delta$, if given at
least $m \geq \mathrm{poly}(1/\epsilon, 1/\delta, \text{size of } H)$ independently drawn training
examples, the algorithm outputs a hypothesis $h\in H$ such that
$$
\Pr\big[\,\mathrm{error}_D(h) \leq \epsilon\,\big] \geq 1-\delta
$$
where $\mathrm{error}_D(h)$ is $h$'s true error under distribution $D$. The key quantities are:
*how many examples* ($m$) are needed, as a function of *how good* ($\epsilon$) and *how confident*
($\delta$) a guarantee is wanted, and how these scale with the *complexity* of $H$.

## 2. VC Dimension
The **Vapnik–Chervonenkis (VC) dimension** of a hypothesis class $H$ measures that complexity
without reference to any specific data distribution. A set of $m$ points is **shattered** by $H$
if, for *every one* of the $2^m$ possible binary labelings of those points, some $h\in H$
realizes that exact labeling. The VC dimension of $H$ is the largest $m$ for which *some* set of
$m$ points can be shattered.

**Worked example — linear threshold classifiers in $\mathbb{R}^2$.** $H = \{\mathrm{sign}(w^\top x
+ b)\}$, half-plane classifiers. Take 3 points in *general position* (not collinear): a line can
realize any of the $2^3=8$ labelings of 3 non-collinear points (this can be checked by
enumeration: for each labeling, some separating line exists). So $H$ shatters some set of size 3,
and $\mathrm{VCdim}(H) \geq 3$. For **any** 4 points in the plane, at least one of the $2^4=16$
labelings cannot be realized by a half-plane (a standard case analysis on whether the 4 points are
in "convex position" — if so, labeling opposite corners of the resulting quadrilateral
identically and adjacent corners oppositely cannot be separated by one line; if one point is
inside the triangle of the other three, labeling the inner point oppositely to all three outer
points also cannot be separated by one line). So no set of 4 points is shattered, and
$\mathrm{VCdim}(H) = 3$ exactly. (The general result: linear threshold classifiers in
$\mathbb{R}^d$ have VC dimension $d+1$.)

## 3. The Classical VC Generalization Bound
With probability at least $1-\delta$ over the draw of $m$ training examples, **every** $h\in H$
satisfies
$$
\mathrm{error}_D(h) \;\leq\; \widehat{\mathrm{error}}(h) \;+\; O\!\left(\sqrt{\frac{\mathrm{VCdim}(H)\,\log(m/\mathrm{VCdim}(H)) + \log(1/\delta)}{m}}\right)
$$
The bound is **uniform** over $H$ (it holds simultaneously for every hypothesis the class
contains, including whichever one training happens to select) and depends on $H$ only through its
VC dimension — not at all on the data distribution or the specific learning algorithm.

## 4. Why This Struggles to Explain Deep Learning
For piecewise-linear networks (e.g., ReLU networks), the VC dimension is known to scale at least
linearly, and in some analyses roughly as a low-degree polynomial, in the number of trainable
parameters. A modern network routinely has $p \gg m$ (far more parameters than training examples).
Plugging $\mathrm{VCdim}(H) \gg m$ into the bound above makes the bound's right-hand side exceed
$1$ — the bound becomes **vacuous** (it guarantees nothing, since any error is trivially
$\leq 1$ for a bounded loss). Yet such networks, trained in the ordinary way, are routinely
observed to generalize well in practice. This is the central puzzle Weeks 9–11 build toward: VC
theory is not *wrong*, but it is evidently not capturing whatever mechanism actually controls
generalization for heavily overparameterized networks trained by gradient-based methods — later
weeks (data-dependent Rademacher complexity, margins, and the research directions of Weeks 12–14)
are different attempts to supply the missing piece.

## 5. Code: Empirical Risk vs. True Risk Gap as Sample Size Grows
```python
import numpy as np

def true_concept(x):
    return (x[:, 0] + x[:, 1] > 0).astype(int)        # a linear threshold target

def fit_best_halfplane(X, y, n_candidates=2000, seed=0):
    rng = np.random.default_rng(seed)
    best_w, best_err = None, np.inf
    for _ in range(n_candidates):
        w = rng.normal(size=2)
        pred = (X @ w > 0).astype(int)
        err = np.mean(pred != y)
        if err < best_err:
            best_err, best_w = err, w
    return best_w, best_err

rng = np.random.default_rng(0)
X_test = rng.normal(size=(5000, 2))
y_test = true_concept(X_test)

for m in [5, 10, 20, 50, 200, 1000]:
    X_train = rng.normal(size=(m, 2))
    y_train = true_concept(X_train)
    w_hat, train_err = fit_best_halfplane(X_train, y_train)
    test_err = np.mean((X_test @ w_hat > 0).astype(int) != y_test)
    print(f"m={m:5d}  train err={train_err:.3f}  true err={test_err:.3f}  gap={test_err-train_err:.3f}")
```
Expect the gap between training error and true (test) error to shrink as $m$ grows, consistent
with the VC bound's $O(1/\sqrt{m})$ shape — this small example uses a VC dimension-3 class
(half-planes), where $m \gg \mathrm{VCdim}$ is achievable with the sample sizes shown, unlike the
deep-network regime of Section 4.

## 6. In-Class Exercise
For 4 points placed at the corners of a square, find the one binary labeling (up to symmetry)
that no single straight line can realize, and explain why in one sentence.
