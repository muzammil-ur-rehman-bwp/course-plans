# Week 8 — Lecture Content: Rigorous Ensemble Theory; Midterm Review

## 1. AdaBoost's Training-Error Bound — Setup
AdaBoost maintains a distribution $D_t$ over the $m$ training points, fits a weak learner
$h_t$ with weighted error $\epsilon_t = \Pr_{i\sim D_t}[h_t(x_i)\neq y_i] < \tfrac12$, sets
$\alpha_t = \tfrac12\ln\!\frac{1-\epsilon_t}{\epsilon_t}>0$, and updates
$D_{t+1}(i)\propto D_t(i)\exp(-\alpha_t y_i h_t(x_i))$. The final classifier is
$H(x)=\mathrm{sign}\big(\sum_{t=1}^T \alpha_t h_t(x)\big)$.

## 2. Derivation Sketch of the Training-Error Bound
Let $Z_t$ be the normalization constant of the reweighting at round $t$:
$Z_t=\sum_i D_t(i)\exp(-\alpha_ty_ih_t(x_i))$. A short calculation using the definitions of
$\alpha_t$ and $\epsilon_t$ gives
$$
Z_t = \epsilon_t e^{\alpha_t} + (1-\epsilon_t)e^{-\alpha_t} = 2\sqrt{\epsilon_t(1-\epsilon_t)}
$$
(substitute $\alpha_t=\frac12\ln\frac{1-\epsilon_t}{\epsilon_t}$ directly: $e^{\alpha_t} =
\sqrt{(1-\epsilon_t)/\epsilon_t}$, so $\epsilon_t e^{\alpha_t} = \sqrt{\epsilon_t(1-\epsilon_t)}$
and $(1-\epsilon_t)e^{-\alpha_t}=\sqrt{\epsilon_t(1-\epsilon_t)}$ as well; summing gives
$2\sqrt{\epsilon_t(1-\epsilon_t)}$). A standard unraveling of the reweighting recursion shows the
**training (0/1) error** of $H$ is bounded by the product of these normalization constants:
$$
\widehat{L}_S(H) \;\leq\; \prod_{t=1}^T Z_t \;=\; \prod_{t=1}^T 2\sqrt{\epsilon_t(1-\epsilon_t)}.
$$
(Sketch of why: the exponential loss $\frac1m\sum_i\exp(-y_i\sum_t\alpha_th_t(x_i))$ upper-bounds
the 0/1 training loss, since $\exp(-yf(x))\geq\mathbb{1}[yf(x)\leq0]$ for any real $f(x)$; and the
exponential loss equals $\prod_tZ_t$ exactly, by telescoping the per-round reweighting definition.)
Writing the **edge** $\gamma_t=\tfrac12-\epsilon_t>0$ (how much better than chance the weak learner
is), $2\sqrt{\epsilon_t(1-\epsilon_t)}=\sqrt{1-4\gamma_t^2}\leq e^{-2\gamma_t^2}$ (using
$1-u\leq e^{-u}$), so if every $\gamma_t\geq\gamma>0$:
$$
\boxed{\widehat{L}_S(H) \;\leq\; \prod_{t=1}^T e^{-2\gamma_t^2} \;\leq\; e^{-2\gamma^2 T} \;\xrightarrow{T\to\infty}\; 0 \text{ exponentially fast.}}
$$

## 3. Margin Theory — Why Boosting Resists Overfitting
Driving training error to zero (Section 2) does not, by itself, explain why boosting often
*keeps improving* its test error for many additional rounds *after* training error hits zero —
naive VC-based reasoning over more and more rounds ($T$ increasing the effective complexity of the
combined classifier) would predict eventual overfitting. Define the (normalized) **margin** of
training point $i$ as
$$
\mathrm{margin}(i) = \frac{y_i\sum_{t=1}^T \alpha_t h_t(x_i)}{\sum_{t=1}^T\alpha_t} \in[-1,1].
$$
A point with $\mathrm{margin}(i)>0$ is correctly classified; a *larger* margin means the combined
vote is more lopsided, hence more robust to small perturbations. The Schapire–Freund–Bartlett–Lee
margin bound states, informally: for any $\theta>0$, with high probability,
$$
L_D(H) \;\leq\; \widehat{\Pr}_S\big[\mathrm{margin}(i)\leq\theta\big] \;+\; O\!\left(\sqrt{\frac{\mathrm{VCdim}(\mathcal{H}_{\text{weak}})}{m\,\theta^2}}\right),
$$
where $\mathcal{H}_{\text{weak}}$ is the weak-learner class — **crucially, with no explicit
dependence on the number of rounds $T$.** Empirically, AdaBoost keeps increasing the *margin
distribution* over training points for many rounds after training error reaches zero (points
already correctly classified get pushed to ever-larger margins), which this bound shows keeps
improving the generalization guarantee even while $\widehat{L}_S(H)$ is flat at zero — resolving
the apparent paradox.

## 4. Bagging's Variance-Reduction Argument
Let $\hat h_1,\dots,\hat h_n$ be estimators (e.g., trees fit on bootstrap resamples), each with
variance $\sigma^2$ and pairwise correlation $\rho$ (identical marginal variance, exchangeable).
The bagged prediction is $\bar h = \frac1n\sum_k\hat h_k$. Its variance:
$$
\mathrm{Var}(\bar h) = \frac{1}{n^2}\mathrm{Var}\Big(\sum_k\hat h_k\Big)
= \frac{1}{n^2}\Big[\sum_k\mathrm{Var}(\hat h_k) + \sum_{k\neq l}\mathrm{Cov}(\hat h_k,\hat h_l)\Big]
= \frac{1}{n^2}\big[n\sigma^2 + n(n-1)\rho\sigma^2\big]
$$
$$
\boxed{\mathrm{Var}(\bar h) \;=\; \frac{\sigma^2}{n} \;+\; \frac{n-1}{n}\,\rho\sigma^2 \;\xrightarrow{n\to\infty}\; \rho\sigma^2.}
$$
**Reading this formula:** bagging shrinks the *first* term (averaging $n$ things shrinks their
independent-noise component by $1/n$, exactly as for any sample mean), but it can never remove the
*second* term — a floor of $\rho\sigma^2$ set entirely by how correlated the base estimators are.
This is precisely why Random Forests add **random feature subsets** on top of bootstrap
resampling: bootstrap resampling alone leaves individual trees still fairly correlated (they are
all fit on overlapping data and tend to split on the same strong predictors first), but forcing
each split to consider only a random feature subset decorrelates the trees further, lowering
$\rho$ and hence lowering the variance floor beyond what bagging alone achieves.

## 5. Midterm Review
The midterm (end of this week) covers Weeks 1–8: the ERM framework; the finite-class PAC bound
(Hoeffding + union bound); VC dimension (shattering, worked examples, the VC bound, Sauer–Shelah);
Rademacher complexity (definition, bound, Massart's lemma link to VC); concentration inequalities
(Markov, Chebyshev, Hoeffding's full derivation, McDiarmid); convex optimization (GD convergence
rates, KKT); RKHS theory and the representer theorem; and this week's ensemble-theory bounds.

## 6. Code: AdaBoost From Scratch — Training Error and Margin Curves
```python
import numpy as np

def decision_stump(X, y, weights):
    """Best axis-aligned threshold stump under sample weights `weights`."""
    m, d = X.shape
    best = (np.inf, None, None, None)
    for j in range(d):
        order = np.argsort(X[:, j])
        xs, ys, ws = X[order, j], y[order], weights[order]
        for thresh in (xs[:-1] + xs[1:]) / 2:
            for sign in (1, -1):
                pred = np.where(xs <= thresh, sign, -sign)
                err = np.sum(ws * (pred != ys))
                if err < best[0]:
                    best = (err, j, thresh, sign)
    err, j, thresh, sign = best
    def h(Xq):
        return np.where(Xq[:, j] <= thresh, sign, -sign)
    return h, err

rng = np.random.default_rng(0)
m = 200
X = rng.normal(size=(m, 2))
y = np.where(X[:, 0] + 0.5 * X[:, 1] > 0, 1, -1)

T = 40
D = np.ones(m) / m
alphas, stumps = [], []
train_errs, margins_over_time = [], []

for t in range(T):
    h, eps = decision_stump(X, y, D)
    eps = np.clip(eps, 1e-6, 1 - 1e-6)
    alpha = 0.5 * np.log((1 - eps) / eps)
    pred_t = h(X)
    D = D * np.exp(-alpha * y * pred_t)
    D /= D.sum()
    alphas.append(alpha); stumps.append(h)

    combined = sum(a * s(X) for a, s in zip(alphas, stumps))
    train_err = np.mean(np.sign(combined) != y)
    margins = y * combined / sum(alphas)
    train_errs.append(train_err)
    margins_over_time.append(np.mean(margins))

print("round  train_err  mean_margin")
for t in [0, 4, 9, 19, 39]:
    print(f"{t+1:5d}  {train_errs[t]:.3f}      {margins_over_time[t]:.3f}")
```
Expect training error to reach `0.000` within a modest number of rounds, while `mean_margin`
keeps *increasing* for many further rounds — the empirical signature Section 3 explains.

## 7. In-Class Exercise
Using Section 4's formula, compute $\mathrm{Var}(\bar h)$ for $\sigma^2=1$, $\rho=0.3$, at
$n=1$, $10$, $100$, $\infty$, and state in one sentence what prevents the variance from reaching
zero even as $n\to\infty$.
