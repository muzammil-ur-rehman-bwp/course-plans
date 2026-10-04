# Week 9 — Lecture Content: Bayesian Machine Learning I

*(Delivered after the Midterm Exam, which covers Weeks 1–8.)*

## 1. The Bayesian Linear Regression Model
Model: $y = Xw + \varepsilon$, $\varepsilon\sim\mathcal{N}(0,\sigma^2 I)$ (known noise variance
$\sigma^2$, for this derivation). Place a Gaussian **prior** over the weights:
$$
w \sim \mathcal{N}(0,\tau^2 I).
$$
Unlike ordinary (frequentist) linear regression, which returns a single point estimate $\hat w$,
Bayesian linear regression computes the full **posterior distribution** $p(w\mid X,y)$ — a
complete description of uncertainty over which $w$ is consistent with the data.

## 2. Deriving the Posterior in Closed Form
By Bayes' rule, $p(w\mid X,y) \propto p(y\mid X,w)\,p(w)$. Taking logs and dropping terms that do
not depend on $w$:
$$
\log p(w\mid X,y) = -\frac{1}{2\sigma^2}\|y-Xw\|^2 - \frac{1}{2\tau^2}\|w\|^2 + \text{const}.
$$
Expand the first term and collect by powers of $w$:
$$
-\frac{1}{2\sigma^2}(y^\top y - 2w^\top X^\top y + w^\top X^\top X w) - \frac{1}{2\tau^2}w^\top w + \text{const}
= -\frac12 w^\top\Big(\frac{X^\top X}{\sigma^2}+\frac{I}{\tau^2}\Big)w + w^\top\frac{X^\top y}{\sigma^2} + \text{const}.
$$
This is a quadratic form in $w$ — i.e., $\log p(w\mid X,y)$ is (up to a constant) the log-density
of a Gaussian. Matching to the general Gaussian log-density $-\frac12(w-\mu)^\top\Lambda(w-\mu)+
\text{const} = -\frac12 w^\top\Lambda w + w^\top\Lambda\mu + \text{const}$ identifies the
**posterior precision** and **posterior mean**:
$$
\boxed{\Lambda_N = \frac{X^\top X}{\sigma^2}+\frac{I}{\tau^2},\qquad \mu_N = \Lambda_N^{-1}\,\frac{X^\top y}{\sigma^2}.}
$$
Equivalently, writing $\lambda=\sigma^2/\tau^2$ and $\Sigma_N=\Lambda_N^{-1}$:
$$
\Sigma_N = \sigma^2\big(X^\top X+\lambda I\big)^{-1},\qquad \mu_N = \big(X^\top X+\lambda I\big)^{-1}X^\top y,\qquad
w\mid X,y \sim \mathcal{N}(\mu_N,\Sigma_N).
$$

## 3. Ridge Regression as MAP Estimation
Because the posterior is Gaussian, its **mode equals its mean**: the maximum a posteriori (MAP)
estimate is $\hat w_{\mathrm{MAP}} = \mu_N = (X^\top X+\lambda I)^{-1}X^\top y$ — which is *exactly*
the ridge-regression solution with penalty $\lambda=\sigma^2/\tau^2$. This gives a precise
probabilistic reading of regularization: **ridge regression is MAP estimation under an isotropic
Gaussian prior on the weights**, where the ridge penalty strength $\lambda$ is the ratio of the
noise variance to the prior variance. A tighter prior (small $\tau^2$, strong belief weights are
near zero) corresponds to a larger $\lambda$ (stronger shrinkage) — exactly matching the intuition
that regularization strength reflects how much we trust the data over our prior belief.

## 4. The Posterior Predictive Distribution
For a new input $x_\star$, since $w$ is Gaussian and $y_\star = x_\star^\top w$ is a linear
function of $w$ (plus independent noise), $y_\star$ is also Gaussian:
$$
y_\star \mid x_\star, X, y \;\sim\; \mathcal{N}\big(x_\star^\top\mu_N,\ x_\star^\top\Sigma_N x_\star + \sigma^2\big).
$$
Notice the predictive variance has **two** sources: $x_\star^\top\Sigma_N x_\star$ (uncertainty
about $w$ itself, which shrinks as more data arrives) and $\sigma^2$ (irreducible observation
noise, which never shrinks) — a first preview of the bias-variance-style decomposition formalized
in Week 14.

## 5. Code: Bayesian Linear Regression From Scratch, Cross-Checked Against Ridge
```python
import numpy as np
from sklearn.linear_model import Ridge

def bayesian_linear_regression(X, y, sigma2, tau2):
    d = X.shape[1]
    lam = sigma2 / tau2
    A = X.T @ X + lam * np.eye(d)
    mu_N = np.linalg.solve(A, X.T @ y)
    Sigma_N = sigma2 * np.linalg.inv(A)
    return mu_N, Sigma_N

rng = np.random.default_rng(0)
n, d = 100, 4
X = rng.normal(size=(n, d))
true_w = np.array([1.5, -2.0, 0.0, 0.5])
sigma2_true = 0.25
y = X @ true_w + rng.normal(scale=np.sqrt(sigma2_true), size=n)

sigma2, tau2 = 0.25, 1.0
mu_N, Sigma_N = bayesian_linear_regression(X, y, sigma2, tau2)

lam = sigma2 / tau2
ridge = Ridge(alpha=lam, fit_intercept=False).fit(X, y)

print("Bayesian posterior mean (MAP):", np.round(mu_N, 4))
print("sklearn Ridge coefficients:   ", np.round(ridge.coef_, 4))
print("max abs difference:", np.max(np.abs(mu_N - ridge.coef_)))
print("posterior std per weight:", np.round(np.sqrt(np.diag(Sigma_N)), 4))
```
Expect the posterior mean and the `Ridge` coefficients to match to numerical precision (both solve
$(X^\top X+\lambda I)^{-1}X^\top y$ with the same $\lambda=\sigma^2/\tau^2$) — `Ridge`'s point
estimate is silently computing a Bayesian MAP estimate, just without reporting the posterior
uncertainty `Sigma_N` gives for free.

## 6. In-Class Exercise
Show, from Section 2's derivation, what happens to $\mu_N$ and $\Sigma_N$ in the two limits
$\tau^2\to\infty$ (a flat, uninformative prior) and $\tau^2\to0$ (an infinitely tight prior pinning
$w=0$), and relate each limit to a familiar estimator.
