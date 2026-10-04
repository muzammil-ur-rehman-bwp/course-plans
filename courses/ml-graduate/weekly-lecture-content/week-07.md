# Week 7 — Lecture Content: Kernel Methods and RKHS Theory

## 1. Positive-Definite Kernels
A symmetric function $k:\mathcal{X}\times\mathcal{X}\to\mathbb{R}$ is a **positive-definite
(Mercer) kernel** if, for every finite set $x_1,\dots,x_m\in\mathcal{X}$ and every $c\in
\mathbb{R}^m$,
$$
\sum_{i=1}^m\sum_{j=1}^m c_ic_j\,k(x_i,x_j) \;\geq\; 0,
$$
equivalently, every **Gram matrix** $K_{ij}=k(x_i,x_j)$ is positive semi-definite. This is the
abstract condition that replaces "$k(x,x')=\phi(x)^\top\phi(x')$ for some explicit feature map
$\phi$" — Mercer's theorem (next) says the two are in fact equivalent.

## 2. Mercer's Theorem (Conceptual)
For a continuous, symmetric, positive-definite kernel $k$ on a compact domain, Mercer's theorem
guarantees an eigen-expansion
$$
k(x,x') = \sum_{i=1}^\infty \lambda_i\,\phi_i(x)\,\phi_i(x'),\qquad \lambda_i\geq0,
$$
where $\{\phi_i\}$ are orthonormal eigenfunctions of the integral operator induced by $k$. This
means $k$ *is* an inner product, $k(x,x')=\Phi(x)^\top\Phi(x')$, for the (possibly
infinite-dimensional) feature map $\Phi(x)=(\sqrt{\lambda_1}\phi_1(x),\sqrt{\lambda_2}\phi_2(x),
\dots)$ — a positive-definite kernel is always *some* inner product in *some* feature space, even
when no explicit, finite-dimensional $\phi$ is ever written down (e.g., the RBF kernel's implicit
feature space is infinite-dimensional).

## 3. Constructing the Reproducing Kernel Hilbert Space (RKHS)
Given $k$, build a Hilbert space $\mathcal{H}_k$ of functions as follows. Start with finite
linear combinations $f(\cdot)=\sum_i\alpha_i k(x_i,\cdot)$, define an inner product
$\langle k(x,\cdot), k(x',\cdot)\rangle_{\mathcal{H}_k} := k(x,x')$ (extended bilinearly), and take
the completion of this space under the resulting norm. The defining **reproducing property**
follows directly:
$$
\langle f, k(x,\cdot)\rangle_{\mathcal{H}_k} = f(x) \quad\text{for all } f\in\mathcal{H}_k, x\in\mathcal{X}
$$
(check it for $f=k(x',\cdot)$: $\langle k(x',\cdot),k(x,\cdot)\rangle=k(x,x')=f(x)$ by the
definition of $k(x',\cdot)$ evaluated at $x$ — linearity extends this to all finite combinations,
and the completion argument extends it to all of $\mathcal{H}_k$). This property is what lets
"evaluating $f$ at a point" and "taking an inner product in $\mathcal{H}_k$" be treated as the same
operation — the entire basis for the representer theorem below.

## 4. The Representer Theorem
**Setting:** minimize a regularized empirical risk over the RKHS,
$$
\hat f \;=\; \arg\min_{f\in\mathcal{H}_k}\ \frac{1}{m}\sum_{i=1}^m \ell\big(f(x_i),y_i\big) \;+\; \lambda\,\|f\|_{\mathcal{H}_k}^2,\qquad \lambda>0.
$$
**Theorem (statement):** the minimizer always has the finite-dimensional form
$$
\hat f(\cdot) \;=\; \sum_{i=1}^m \alpha_i\, k(x_i,\cdot)
$$
for some $\alpha\in\mathbb{R}^m$ — **regardless of how high- or infinite-dimensional
$\mathcal{H}_k$ is.**

**Proof sketch:** decompose any candidate $f\in\mathcal{H}_k$ as $f = f_\parallel + f_\perp$, where
$f_\parallel\in\mathrm{span}\{k(x_1,\cdot),\dots,k(x_m,\cdot)\}$ and $f_\perp$ is orthogonal to that
span. By the reproducing property, $f(x_i) = \langle f,k(x_i,\cdot)\rangle = \langle
f_\parallel,k(x_i,\cdot)\rangle + \langle f_\perp,k(x_i,\cdot)\rangle = f_\parallel(x_i) + 0$ (the
second term vanishes by orthogonality to the span, since $k(x_i,\cdot)$ is in that span). So the
data-fit term $\ell(f(x_i),y_i)$ depends on $f$ *only through* $f_\parallel$ — $f_\perp$ is
completely invisible to the loss. But by Pythagoras, $\|f\|^2_{\mathcal{H}_k} =
\|f_\parallel\|^2+\|f_\perp\|^2 \geq \|f_\parallel\|^2$, with equality iff $f_\perp=0$. So for *any*
$f$ with $f_\perp\neq0$, replacing it with $f_\parallel$ strictly decreases the regularization term
while leaving the data-fit term unchanged — strictly improving the objective. Hence an optimal
$\hat f$ must have $f_\perp=0$, i.e., $\hat f\in\mathrm{span}\{k(x_1,\cdot),\dots,k(x_m,\cdot)\}$,
proving the theorem.

**Why it matters:** it converts an optimization over an infinite-dimensional function space into
an optimization over a finite, $m$-dimensional vector $\alpha$ — this is *the* reason kernel
methods are computationally tractable at all. Plugging $\hat f(\cdot)=\sum_i\alpha_ik(x_i,\cdot)$
back in, $\|\hat f\|^2_{\mathcal{H}_k} = \alpha^\top K\alpha$ (where $K_{ij}=k(x_i,x_j)$) and
$\hat f(x_j)=\sum_i\alpha_iK_{ij} = (K\alpha)_j$, so the whole regularized-risk minimization becomes
a finite-dimensional problem purely in terms of the Gram matrix $K$.

## 5. Connection to Kernel SVMs
Week 6 derived the SVM dual purely in terms of inner products $x_i^\top x_j$. Replacing
$x_i^\top x_j$ with $k(x_i,x_j)$ (the "kernel trick") is now justified rigorously, not just as a
notational substitution: it is equivalent to solving the margin-maximization problem over the RKHS
$\mathcal{H}_k$ instead of over linear functions of $x$, and the representer theorem guarantees the
resulting $\hat f(x)=\sum_i\alpha_iy_ik(x_i,x)+b$ (the familiar kernel-SVM decision function) is
exactly the correct finite-dimensional form the RKHS-optimal solution must take — no information is
lost by restricting the search to this form, because Section 4 proved the true optimum is always
of this form regardless.

## 6. Code: Kernel Ridge Regression via the Representer Theorem
```python
import numpy as np
from sklearn.kernel_ridge import KernelRidge

def rbf_kernel(X1, X2, gamma):
    sq = np.sum(X1**2, axis=1)[:, None] + np.sum(X2**2, axis=1)[None, :] - 2 * X1 @ X2.T
    return np.exp(-gamma * sq)

def kernel_ridge_fit(X, y, lam, gamma):
    K = rbf_kernel(X, X, gamma)
    m = X.shape[0]
    alpha = np.linalg.solve(K + lam * m * np.eye(m), y)   # representer-theorem finite-dim solve
    return alpha

def kernel_ridge_predict(X_train, alpha, X_new, gamma):
    K_new = rbf_kernel(X_new, X_train, gamma)
    return K_new @ alpha

rng = np.random.default_rng(0)
X = rng.uniform(-3, 3, size=(60, 1))
y = np.sin(X[:, 0]) + 0.1 * rng.normal(size=60)
gamma, lam = 0.5, 0.1

alpha = kernel_ridge_fit(X, y, lam, gamma)
X_test = np.linspace(-3, 3, 10).reshape(-1, 1)
pred_scratch = kernel_ridge_predict(X, alpha, X_test, gamma)

skl = KernelRidge(alpha=lam, kernel="rbf", gamma=gamma).fit(X, y)
pred_skl = skl.predict(X_test)

print("max abs difference (from-scratch vs sklearn):", np.max(np.abs(pred_scratch - pred_skl)))
```
Expect the max absolute difference to be at or near machine precision (e.g., `< 1e-8`) — confirming
the from-scratch representer-theorem solve matches `sklearn.kernel_ridge.KernelRidge` (whose
`alpha` parameter plays the role of $\lambda\cdot m$ internally, matched above by scaling).

## 7. In-Class Exercise
For the hard-margin SVM decision function $\hat f(x)=\sum_i\alpha_i y_i k(x_i,x)+b$, identify which
part of the representer-theorem proof (Section 4) explains why only the support vectors ($\alpha_i
>0$) appear with nonzero weight, connecting this back to complementary slackness from Week 6.
