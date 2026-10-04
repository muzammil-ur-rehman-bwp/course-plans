# Week 6 — Lecture Content: Optimization Landscape Theory I

## 1. Classifying Critical Points via the Hessian
At a critical point $\theta^*$ (where $\nabla L(\theta^*) = 0$), a second-order Taylor expansion
gives $L(\theta^*+\delta) \approx L(\theta^*) + \tfrac{1}{2}\delta^\top H \delta$, where $H$ is the
Hessian. The eigenvalues of $H$ classify $\theta^*$:
- all eigenvalues $> 0$: a **local minimum** (the surface curves up in every direction),
- all eigenvalues $< 0$: a **local maximum**,
- a mix of positive and negative eigenvalues: a **saddle point** — the surface curves up in some
  directions and down in others.

## 2. Why Saddle Points Dominate in High Dimensions
Treat $H$'s eigenvalues, heuristically, as $p$ draws from some underlying distribution of
curvature values (the precise distribution depends on the loss and data, but this coarse random-
matrix picture is a standard way to build intuition, used for instance in analyses of
high-dimensional non-convex loss surfaces). If each eigenvalue is positive or negative with roughly
independent, roughly even odds, the probability that **all $p$ of them land on the same sign** —
the requirement for a true local min or max — falls off like $2^{-p}$: exponentially fast in the
number of parameters. A modern network has $p$ in the millions, so under this heuristic, a
critical point with *every single* eigenvalue positive is fantastically unlikely; a critical point
with a mix of signs (a saddle) is overwhelmingly the typical case. This is the core argument for
why **saddle points, not bad local minima, are the characteristic obstacle** in high-dimensional
neural network optimization — not an absence of local minima, but their vanishing relative
frequency compared to saddles as dimension grows.

A related, important pathology: plain Newton's method (Section 3) is not just slow near a saddle
— it can be actively **attracted** to one, because it rescales each eigendirection by
$1/|\lambda_i|$ without regard to $\lambda_i$'s *sign*, so it happily takes a large step *up* a
negative-curvature direction, mistaking "small magnitude curvature" for "near the optimum."
First-order gradient descent does not have this particular pathology — it is merely slow near a
saddle (small gradient in the flat directions) — but still eventually escapes with the help of
noise (e.g., from mini-batching) breaking the exact symmetry that would otherwise strand it exactly
on the saddle's stable manifold.

## 3. Newton's Method
Newton's method uses the full second-order model to jump directly to its minimizer:
$$
\Delta\theta = -H^{-1}\nabla L(\theta), \qquad \theta \leftarrow \theta + \Delta\theta
$$
Near a true local minimum with $H \succ 0$, this converges **quadratically** (error squares each
step) — far faster than gradient descent's linear convergence. But as Section 2 notes, near a
saddle, $H^{-1}\nabla L$ is not guaranteed to be a descent direction at all.

## 4. The Gauss-Newton Approximation
For losses of least-squares form $L(\theta) = \tfrac{1}{2}\|r(\theta)\|^2$ for a residual vector
$r$, the exact Hessian is $H = J^\top J + \sum_k r_k \nabla^2 r_k$, where $J$ is $r$'s Jacobian.
The **Gauss-Newton** approximation drops the second (often small, near a good fit) term:
$$
H_{GN} = J^\top J \quad (\text{always positive semi-definite})
$$
Gauss-Newton sidesteps the saddle-attraction pathology (its approximate Hessian can never have a
negative eigenvalue) at the cost of being only an approximation, valid mainly when residuals are
small or the model is close to linear in $\theta$ locally.

## 5. Why Not at Scale
For $p$ parameters, $H$ (or $J^\top J$) is a $p\times p$ matrix: $O(p^2)$ floats to **store**, and
solving the linear system $H\Delta\theta = -\nabla L$ exactly costs $O(p^3)$ with direct methods
(or $O(p^2)$ per iteration with iterative Krylov methods such as conjugate gradient, which still
requires repeated Hessian-vector products). For $p$ in the tens of millions, $O(p^2)$ storage alone
is far beyond any practical memory budget — this is the concrete, practical reason production
training uses first-order and adaptive-first-order methods (Week 7) rather than exact second-order
methods, despite the latter's superior local convergence rate.

## 6. Code: Hessian Eigenspectrum and Newton vs. Gradient Descent Near a Saddle
```python
import numpy as np

def f(theta):
    x, y = theta
    return x**2 - y**2                      # a canonical saddle at the origin

def grad_f(theta):
    x, y = theta
    return np.array([2*x, -2*y])

def hess_f(theta):
    return np.array([[2.0, 0.0], [0.0, -2.0]])

theta0 = np.array([0.01, 1.0])              # start just off the saddle

# Gradient descent
theta_gd = theta0.copy()
for _ in range(50):
    theta_gd = theta_gd - 0.1 * grad_f(theta_gd)

# Newton's method (will be attracted along the negative-curvature y-direction)
theta_nt = theta0.copy()
for _ in range(50):
    H = hess_f(theta_nt)
    theta_nt = theta_nt - np.linalg.solve(H, grad_f(theta_nt))

eigvals = np.linalg.eigvalsh(hess_f(theta0))
print("Hessian eigenvalues at the saddle:", eigvals)   # [-2, 2]: mixed sign -> saddle
print("Gradient descent final point:", theta_gd)        # moves away from the saddle in y
print("Newton's method final point:", theta_nt)         # jumps straight onto/through the saddle
```
Gradient descent's step in $y$ shrinks $y$ toward $0$ slowly but monotonically escapes along $x$;
Newton's method, by contrast, treats the negative-curvature $y$-direction identically to the
positive-curvature $x$-direction (both rescaled to a single Newton step), illustrating the
saddle-attraction pathology directly.

## 7. In-Class Exercise
For the Hessian $\begin{psmallmatrix}3 & 0\\0 & -1\end{psmallmatrix}$, classify the critical point
and state, without computing it, whether a vanilla Newton step from a nearby point would move
toward or away from the critical point along the second coordinate.
