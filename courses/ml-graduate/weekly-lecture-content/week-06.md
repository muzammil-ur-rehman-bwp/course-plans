# Week 6 — Lecture Content: Convex Optimization for ML

## 1. Convex Sets and Convex Functions
A set $C\subseteq\mathbb{R}^n$ is **convex** if $x,y\in C \Rightarrow \lambda x+(1-\lambda)y\in C$
for all $\lambda\in[0,1]$. A function $f:\mathbb{R}^n\to\mathbb{R}$ is **convex** if its domain is
convex and $f(\lambda x+(1-\lambda)y)\leq \lambda f(x)+(1-\lambda)f(y)$ for all $x,y$ in the
domain, $\lambda\in[0,1]$. Equivalent first-order condition (for differentiable $f$): $f(y)\geq
f(x)+\nabla f(x)^\top(y-x)$ for all $x,y$ — the tangent plane at any point is a global
underestimator. Equivalent second-order condition (for twice-differentiable $f$): $\nabla^2
f(x)\succeq 0$ everywhere (the Hessian is positive semi-definite). $f$ is **$\mu$-strongly convex**
if $f(x)-\frac{\mu}{2}\|x\|^2$ is convex, equivalently $\nabla^2 f(x)\succeq\mu I$; $f$ is
**$L$-smooth** if $\nabla f$ is $L$-Lipschitz, equivalently $\nabla^2 f(x)\preceq L I$.

## 2. Gradient Descent Convergence — Convex, $L$-Smooth Case
**Descent lemma** (from $L$-smoothness): $f(y)\leq f(x)+\nabla f(x)^\top(y-x)+\frac{L}{2}\|y-x\|^2$
for all $x,y$. Plug in gradient descent's update $y=x-\eta\nabla f(x)$ with $\eta=1/L$:
$$
f(x^+) \leq f(x) - \frac{1}{L}\|\nabla f(x)\|^2 + \frac{1}{2L}\|\nabla f(x)\|^2 = f(x) - \frac{1}{2L}\|\nabla f(x)\|^2.
$$
By convexity's first-order condition applied at $x$ with $y=x^\star$: $f(x)-f(x^\star)\leq \nabla
f(x)^\top(x-x^\star)$. A standard algebraic identity (completing the square on the GD update) gives
$\|x^+-x^\star\|^2 = \|x-x^\star\|^2 - \frac{2}{L}\nabla f(x)^\top(x-x^\star) +
\frac{1}{L^2}\|\nabla f(x)\|^2 \leq \|x-x^\star\|^2 - \frac{1}{L^2}\|\nabla f(x)\|^2$ (using the
descent-lemma inequality $f(x)-f(x^+)\geq\frac{1}{2L}\|\nabla f(x)\|^2$ and convexity together).
Summing $f(x_t)-f(x^\star)\leq\nabla f(x_t)^\top(x_t-x^\star)=\frac{L}{2}(\|x_t-x^\star\|^2 -
\|x_{t+1}-x^\star\|^2)$ over $t=0,\dots,T-1$ telescopes:
$$
\sum_{t=0}^{T-1}\big(f(x_t)-f(x^\star)\big) \leq \frac{L}{2}\|x_0-x^\star\|^2.
$$
Since $f(x_t)$ is non-increasing (Section 2's first inequality), $f(x_{T-1})-f(x^\star) \leq
\frac{1}{T}\sum_{t=0}^{T-1}(f(x_t)-f(x^\star))$, giving the standard rate
$$
\boxed{f(x_T) - f(x^\star) \;\leq\; \frac{L\,\|x_0-x^\star\|^2}{2T} \;=\; O(1/T).}
$$

## 3. Gradient Descent Convergence — Strongly Convex + Smooth Case
If additionally $f$ is $\mu$-strongly convex, the gradient-descent iterate contracts linearly. Key
contraction lemma (for $\eta=1/L$): using strong convexity's lower bound $f(x^\star)\geq
f(x)+\nabla f(x)^\top(x^\star-x)+\frac{\mu}{2}\|x^\star-x\|^2$ together with the descent lemma from
Section 2 applied appropriately, one obtains
$$
\|x_{t+1}-x^\star\|^2 \;\leq\; \left(1-\frac{\mu}{L}\right)\|x_t-x^\star\|^2.
$$
Unrolling over $T$ steps gives **linear (geometric) convergence** of the iterates:
$$
\|x_T - x^\star\|^2 \leq \left(1-\frac{\mu}{L}\right)^T \|x_0-x^\star\|^2,
$$
and by $L$-smoothness ($f(x_T)-f(x^\star)\leq \frac{L}{2}\|x_T-x^\star\|^2$),
$$
\boxed{f(x_T)-f(x^\star) \;\leq\; \frac{L}{2}\left(1-\frac{\mu}{L}\right)^T \|x_0-x^\star\|^2.}
$$
This is exponentially faster in $T$ than the $O(1/T)$ merely-convex rate — the condition number
$L/\mu$ controls how fast. This is why, e.g., ridge-regularized objectives (strongly convex) train
faster than unregularized ones (only convex) under gradient descent.

## 4. Lagrangian Duality
For the constrained problem $\min_x f_0(x)$ s.t. $g_i(x)\leq0\ (i=1,\dots,k)$, $h_j(x)=0\
(j=1,\dots,l)$, define the **Lagrangian**
$$
\mathcal{L}(x,\lambda,\nu) = f_0(x) + \sum_i \lambda_i g_i(x) + \sum_j \nu_j h_j(x),\qquad \lambda_i\geq0.
$$
The **dual function** is $g(\lambda,\nu)=\inf_x \mathcal{L}(x,\lambda,\nu)$. **Weak duality**
always holds: $g(\lambda,\nu)\leq p^\star$ (the primal optimum) for any feasible $\lambda\geq0,\nu$
— because for any feasible $x$ (satisfying the constraints), $\mathcal{L}(x,\lambda,\nu)\leq
f_0(x)$ (the $\lambda_ig_i(x)$ terms are $\leq0$ and the $\nu_jh_j(x)$ terms are $0$), so
$g(\lambda,\nu)\leq\inf_{x\text{ feasible}}\mathcal{L}(x,\lambda,\nu)\leq\inf_{x\text{
feasible}}f_0(x)=p^\star$. Under convexity plus a regularity condition (e.g., **Slater's
condition**: a strictly feasible point exists), **strong duality** holds: the dual optimum equals
$p^\star$ exactly, with zero duality gap.

## 5. KKT Conditions
If $x^\star,(\lambda^\star,\nu^\star)$ are primal/dual optimal with zero duality gap and all
functions are differentiable, the **Karush-Kuhn-Tucker (KKT) conditions** hold:
1. **Primal feasibility:** $g_i(x^\star)\leq0$, $h_j(x^\star)=0$.
2. **Dual feasibility:** $\lambda_i^\star\geq0$.
3. **Complementary slackness:** $\lambda_i^\star g_i(x^\star)=0$ (an inactive constraint must have
   zero multiplier).
4. **Stationarity:** $\nabla f_0(x^\star)+\sum_i\lambda_i^\star\nabla g_i(x^\star)+\sum_j\nu_j^\star
   \nabla h_j(x^\star)=0$.
For a convex problem satisfying Slater's condition, these conditions are both **necessary and
sufficient** for optimality.

## 6. Worked Example — the SVM Primal/Dual via KKT
Primal: $\min_{w,b}\frac12\|w\|^2$ s.t. $y_i(w^\top x_i+b)\geq1\ \forall i$ (hard-margin SVM).
Lagrangian: $\mathcal{L}=\frac12\|w\|^2-\sum_i\alpha_i\big[y_i(w^\top x_i+b)-1\big]$, $\alpha_i\geq0$.
**Stationarity:** $\partial\mathcal{L}/\partial w = w-\sum_i\alpha_i y_i x_i=0 \Rightarrow w^\star =
\sum_i\alpha_i y_i x_i$; $\partial\mathcal{L}/\partial b=-\sum_i\alpha_i y_i=0 \Rightarrow
\sum_i\alpha_i y_i=0$. Substituting back gives the dual:
$$
\max_\alpha \sum_i\alpha_i - \frac12\sum_{i,j}\alpha_i\alpha_j y_iy_j x_i^\top x_j \quad\text{s.t. } \alpha_i\geq0,\ \sum_i\alpha_iy_i=0.
$$
**Complementary slackness** ($\alpha_i[y_i(w^\top x_i+b)-1]=0$) is exactly why only points with
$y_i(w^\top x_i+b)=1$ (the support vectors, sitting exactly on the margin) can have $\alpha_i>0$ —
every other point automatically gets $\alpha_i=0$ and drops out of $w^\star=\sum_i\alpha_iy_ix_i$.
This dual, expressed purely in terms of inner products $x_i^\top x_j$, is also exactly what makes
the kernel trick possible (Week 7): replace $x_i^\top x_j$ with $k(x_i,x_j)$.

## 7. Code: Verifying the Convergence Rates Empirically
```python
import numpy as np

def grad_descent(grad_f, x0, eta, T):
    x = x0.copy()
    traj = [x.copy()]
    for _ in range(T):
        x = x - eta * grad_f(x)
        traj.append(x.copy())
    return np.array(traj)

d = 10
rng = np.random.default_rng(0)
A_convex = rng.normal(size=(d, d)); A_convex = A_convex.T @ A_convex / d     # PSD, possibly singular -> merely convex
A_strong = A_convex + 1.0 * np.eye(d)                                        # add mu*I -> strongly convex
x_star = np.zeros(d)

for name, A in [("convex (mu=0)", A_convex), ("strongly convex (mu=1)", A_strong)]:
    L = np.linalg.eigvalsh(A).max()
    f = lambda x: 0.5 * x @ A @ x
    grad_f = lambda x: A @ x
    x0 = rng.normal(size=d)
    traj = grad_descent(grad_f, x0, eta=1.0 / L, T=200)
    gaps = np.array([f(x) - f(x_star) for x in traj])
    print(f"{name:24s} f(x_0)-f*={gaps[0]:.4f}  f(x_50)-f*={gaps[50]:.2e}  f(x_200)-f*={gaps[200]:.2e}")
```
Expect the merely-convex run's gap to shrink roughly like $1/T$ (polynomial decay, still
substantial at $T=200$), while the strongly-convex run's gap shrinks geometrically (many orders of
magnitude smaller by $T=50$) — a direct empirical confirmation of Sections 2–3's rates.

## 8. In-Class Exercise
For the SVM dual in Section 6, explain why $w^\star=\sum_i\alpha_i y_i x_i$ together with
complementary slackness implies that $w^\star$ depends only on the support vectors, and relate
this to the sparsity of $\alpha^\star$ you would expect to observe in practice.
