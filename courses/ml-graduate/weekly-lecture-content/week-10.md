# Week 10 — Lecture Content: Bayesian Machine Learning II — Gaussian Processes

## 1. A Gaussian Process as a Prior Over Functions
Week 9 placed a prior over a finite weight vector $w$. A **Gaussian Process (GP)** generalizes
this to a prior directly over *functions* $f:\mathcal{X}\to\mathbb{R}$, written $f\sim
\mathrm{GP}(m,k)$ for a mean function $m(x)$ (typically $m\equiv0$) and a covariance (kernel)
function $k(x,x')$. The defining property: for **any** finite set of inputs $x_1,\dots,x_n$, the
vector of function values $(f(x_1),\dots,f(x_n))$ is jointly Gaussian,
$$
\big(f(x_1),\dots,f(x_n)\big) \sim \mathcal{N}\big(0,\,K\big),\qquad K_{ij}=k(x_i,x_j).
$$
$k$ must be a positive-definite kernel (Week 7) precisely so that every such $K$ is a valid
covariance matrix (positive semi-definite). The kernel controls the prior's assumptions about
function smoothness: e.g., for the RBF kernel $k(x,x')=\exp(-\|x-x'\|^2/2\ell^2)$, the length
scale $\ell$ controls how quickly the function can vary.

## 2. GP Regression — Setup
Observe noisy training data $y_i = f(x_i)+\varepsilon_i$, $\varepsilon_i\sim\mathcal{N}(0,
\sigma_n^2)$ i.i.d., at inputs $X=(x_1,\dots,x_m)$. We want the posterior distribution over $f$ at
new test inputs $X_\star$. Because the GP prior makes any finite set of function values jointly
Gaussian, and the observation noise is Gaussian, the joint distribution of observed outputs $y$
and test function values $f_\star$ is also jointly Gaussian:
$$
\begin{bmatrix} y \\ f_\star \end{bmatrix} \sim \mathcal{N}\left(0,\ \begin{bmatrix} K+\sigma_n^2 I & K_\star \\ K_\star^\top & K_{\star\star}\end{bmatrix}\right),
$$
where $K=k(X,X)$ ($m\times m$), $K_\star=k(X,X_\star)$ ($m\times n_\star$), $K_{\star\star}=
k(X_\star,X_\star)$ ($n_\star\times n_\star$).

## 3. Deriving the GP Predictive Equations
For a jointly Gaussian $\begin{bmatrix}a\\b\end{bmatrix}\sim\mathcal{N}\left(0,
\begin{bmatrix}A&C\\C^\top&B\end{bmatrix}\right)$, the conditional distribution $b\mid a$ is
Gaussian with mean $C^\top A^{-1}a$ and covariance $B-C^\top A^{-1}C$ (the standard Gaussian
conditioning identity, obtained by completing the square in the joint density or via the Schur
complement of the joint precision matrix). Applying this with $a=y$, $b=f_\star$, $A=K+\sigma_n^2I$,
$B=K_{\star\star}$, $C=K_\star$:
$$
\boxed{
f_\star \mid X,y,X_\star \;\sim\; \mathcal{N}\Big(\ \underbrace{K_\star^\top(K+\sigma_n^2I)^{-1}y}_{\text{predictive mean}}\ ,\ \ \underbrace{K_{\star\star} - K_\star^\top(K+\sigma_n^2I)^{-1}K_\star}_{\text{predictive covariance}}\ \Big).
}
$$
**Reading the predictive mean:** it is a kernel-weighted combination of training outputs,
$\bar f_\star = \sum_i \beta_i\, k(x_i,x_\star)$ with $\beta=(K+\sigma_n^2I)^{-1}y$ — the exact same
finite, kernel-expansion form the representer theorem (Week 7) guarantees for a regularized RKHS
problem; GP regression's posterior mean coincides with kernel ridge regression's prediction for
$\lambda=\sigma_n^2$. **Reading the predictive covariance:** it starts at the prior covariance
$K_{\star\star}$ and is *reduced* by an amount that grows with how strongly $x_\star$ correlates
(via $k$) with the training inputs — uncertainty shrinks near observed data and remains close to
the prior far from it.

## 4. The Role of the Kernel and Noise Hyperparameters
- The kernel's **length scale** controls how far "influence" from a training point extends in
  input space — a short length scale gives a wiggly posterior mean that mostly reverts to the prior
  between data points; a long length scale gives a smoother, more constrained posterior mean.
- The kernel's **signal variance** scales the overall amplitude of plausible functions.
- The **noise variance** $\sigma_n^2$ controls how tightly the posterior mean is pulled through the
  observed $y_i$ versus how much it is allowed to smooth over them.
These are typically set by maximizing the **marginal likelihood** $p(y\mid X)$ (a Gaussian
density in closed form here too), which automatically penalizes overly complex kernel settings —
a Bayesian form of model selection, previewing Week 14.

## 5. Numerical Note: Why Cholesky, Not Direct Inversion
$(K+\sigma_n^2I)^{-1}$ should never be computed by direct matrix inversion in practice: $K$ can be
near-singular (nearby training points give nearly identical kernel rows), making direct inversion
numerically unstable. The standard approach instead Cholesky-factorizes $K+\sigma_n^2I = LL^\top$
and solves two triangular systems, which is both faster and numerically far more stable; this is
exactly why `scipy.linalg.cho_factor`/`cho_solve` is used in the code below rather than
`np.linalg.inv`.

## 6. Code: Gaussian Process Regression From Scratch
```python
import numpy as np
from scipy.linalg import cho_factor, cho_solve

def rbf_kernel(X1, X2, length_scale, signal_var=1.0):
    sq = np.sum(X1**2, 1)[:, None] + np.sum(X2**2, 1)[None, :] - 2 * X1 @ X2.T
    return signal_var * np.exp(-0.5 * sq / length_scale**2)

def gp_predict(X_train, y_train, X_test, length_scale, signal_var, noise_var):
    K = rbf_kernel(X_train, X_train, length_scale, signal_var) + noise_var * np.eye(len(X_train))
    K_star = rbf_kernel(X_train, X_test, length_scale, signal_var)
    K_star_star = rbf_kernel(X_test, X_test, length_scale, signal_var)

    c, low = cho_factor(K)
    alpha = cho_solve((c, low), y_train)                 # (K + noise*I)^-1 y, via Cholesky
    mean = K_star.T @ alpha

    v = cho_solve((c, low), K_star)                      # (K + noise*I)^-1 K_star
    cov = K_star_star - K_star.T @ v
    return mean, cov

rng = np.random.default_rng(0)
X_train = rng.uniform(-5, 5, size=(15, 1))
y_train = np.sin(X_train[:, 0]) + 0.05 * rng.normal(size=15)
X_test = np.linspace(-5, 5, 100).reshape(-1, 1)

mean, cov = gp_predict(X_train, y_train, X_test, length_scale=1.0, signal_var=1.0, noise_var=0.05**2)
std = np.sqrt(np.clip(np.diag(cov), 0, None))

print("predictive mean at x=0:", round(np.interp(0, X_test[:, 0], mean), 4),
      "  true f(0)=sin(0)=0")
print("predictive std at x=0 (near data):", round(np.interp(0, X_test[:, 0], std), 4))
print("predictive std at x=4.9 (edge, sparser data):", round(np.interp(4.9, X_test[:, 0], std), 4))
```
Expect the predictive mean near $x=0$ to be close to $\sin(0)=0$ with small predictive std (dense
nearby training data), and the predictive std to grow near the domain's edges where training
points are sparser — the credible band should visibly widen away from data, as Section 3 predicts.

## 7. In-Class Exercise
Explain why the GP predictive mean formula in Section 3 has exactly the same algebraic form as the
kernel ridge regression solution from Week 7, and identify which GP hyperparameter plays the role
of the ridge penalty $\lambda$.
