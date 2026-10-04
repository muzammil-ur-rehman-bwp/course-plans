# Week 5 — Lecture Content: Full-Information Online Convex Optimization

## 1. The Online Convex Optimization Protocol
At each round $t=1,\dots,T$: the learner chooses $x_t \in \mathcal K$ for a convex, compact
feasible set $\mathcal K \subset \mathbb{R}^d$; an adversary then reveals a convex loss function
$f_t : \mathcal K \to \mathbb{R}$ **in full** — the learner may evaluate $f_t$ (and its gradient)
at any point, not only at $x_t$. The learner suffers loss $f_t(x_t)$. This is the
**full-information** setting. It is explicitly distinct from the sibling postgraduate course's
bandit setting (*Advanced Artificial Intelligence*, Weeks 2–3), where the learner observes only
the realized scalar loss of the action actually taken, and has no access to the loss function's
value anywhere else — in particular, no gradient information is ever directly available in the
bandit setting, which is exactly why bandit algorithms (multiplicative weights, UCB) rely on
confidence bounds and exploration rather than gradients. Full information is a strictly easier
setting per round, but OCO still makes **no statistical assumption** about how the $f_t$ sequence
is generated — it may be adversarial, exactly as in the bandit setting — which is what makes OCO's
regret bounds non-trivial despite having more information per round.

## 2. Regret
As with multiplicative weights, "optimal loss" is undefined without a statistical model, so
performance is measured against the best fixed point in hindsight:
```
Regret_T = Σ_{t=1}^T f_t(x_t)  −  min_{x∈K} Σ_{t=1}^T f_t(x)
```
An OCO algorithm is **no-regret** if $\mathrm{Regret}_T/T \to 0$.

## 3. Follow-The-Regularized-Leader (FTRL)
The naive "Follow-The-Leader" strategy — play $x_{t+1} = \arg\min_{x\in\mathcal K}\sum_{s\leq t}
f_s(x)$ (whatever has been best so far) — can be shown to suffer linear regret in the worst case
(it overfits to the exact history and can be made to oscillate badly by an adversary). **FTRL**
stabilizes this by adding a strongly convex regularizer $R:\mathcal K\to\mathbb{R}$:
```
x_{t+1} = argmin_{x∈K}  ( η Σ_{s≤t} f_s(x) + R(x) )
```
for a learning-rate parameter $\eta>0$. The regularizer $R$ (e.g., $R(x)=\frac12\|x\|_2^2$) biases
each round's choice toward a fixed, stable point, damping the leader's tendency to swing wildly in
response to the most recent losses — the same stabilization role a prior plays in Bayesian
estimation, or a ridge penalty plays in regression (both assumed from the graduate course).

## 4. Online Gradient Descent (OGD) as Linearized FTRL
**Online gradient descent** replaces $f_t(x)$ in the FTRL objective with its first-order
(linearized) approximation at the played point, $\langle \nabla f_t(x_t), x\rangle$, and uses the
Euclidean-norm-squared regularizer. The resulting update, after simplification, is simply
projected gradient descent:
```
x_{t+1} = Π_K ( x_t − η_t ∇f_t(x_t) )
```
where $\Pi_{\mathcal K}$ denotes Euclidean projection onto $\mathcal K$. OGD is exactly the
graduate course's gradient descent (assumed background), now applied *online*, one new convex
function at a time, rather than *offline* to a single fixed objective.

## 5. Deriving OGD's Regret Bound
Assume $\mathcal K$ has diameter $D$ (i.e. $\|x-y\|\leq D$ for all $x,y\in\mathcal K$) and every
loss $f_t$ is convex with $\|\nabla f_t(x)\|\leq G$ on $\mathcal K$. Fix any comparator
$x^\star\in\mathcal K$ (in particular the hindsight minimizer). By convexity,
```
f_t(x_t) − f_t(x*) ≤ ⟨∇f_t(x_t), x_t − x*⟩
```
so it suffices to bound $\sum_t \langle\nabla f_t(x_t), x_t - x^\star\rangle$. Using the standard
non-expansiveness of Euclidean projection onto a convex set, $\|x_{t+1}-x^\star\|^2 =
\|\Pi_{\mathcal K}(x_t-\eta_t\nabla f_t(x_t)) - x^\star\|^2 \leq \|x_t - \eta_t \nabla f_t(x_t) -
x^\star\|^2$. Expanding the right-hand side:
```
‖x_t − η_t∇f_t(x_t) − x*‖² = ‖x_t − x*‖² − 2η_t⟨∇f_t(x_t), x_t − x*⟩ + η_t²‖∇f_t(x_t)‖²
```
Rearranging for the inner product term:
```
⟨∇f_t(x_t), x_t − x*⟩ ≤ ( ‖x_t−x*‖² − ‖x_{t+1}−x*‖² ) / (2η_t)  +  η_t G² / 2
```
Summing over $t=1,\dots,T$ with a **constant** step size $\eta_t=\eta$, the first term telescopes:
$\sum_t (\|x_t-x^\star\|^2 - \|x_{t+1}-x^\star\|^2)/(2\eta) = (\|x_1-x^\star\|^2 -
\|x_{T+1}-x^\star\|^2)/(2\eta) \leq D^2/(2\eta)$ (dropping the non-negative final term, and
bounding the first by the diameter $D$). So
```
Regret_T ≤ D²/(2η) + η G² T / 2
```
Optimizing $\eta$ over this bound (set $\eta = D/(G\sqrt T)$, balancing the two terms exactly as
in the Week-2-style bias/variance tradeoffs seen throughout this course) gives
```
Regret_T ≤ D G √T
```
**OGD achieves $O(\sqrt T)$ regret** — sublinear, exactly like the multiplicative-weights bound in
the sibling course, but now derived via convex-optimization machinery (projection
non-expansiveness + telescoping) rather than a potential-function argument, reflecting the
different (full-information, convex, continuous-decision-set) structure of this setting. A
time-varying step size $\eta_t = D/(G\sqrt t)$ (not requiring $T$ to be known in advance) achieves
the same $O(\sqrt T)$ rate via a near-identical telescoping argument.

## 6. Python: Implementing FTRL and OGD, and Comparing Regret
```python
import numpy as np

def project_to_ball(x, radius):
    norm = np.linalg.norm(x)
    return x if norm <= radius else x * (radius / norm)

def online_gradient_descent(grad_fns, x0, D, G, radius):
    T = len(grad_fns)
    x = x0.copy()
    xs = [x.copy()]
    for t in range(1, T + 1):
        eta_t = D / (G * np.sqrt(t))
        g = grad_fns[t - 1](x)
        x = project_to_ball(x - eta_t * g, radius)
        xs.append(x.copy())
    return xs

def ftrl_quadratic_reg(loss_fns, grad_fns, eta, radius, d):
    """FTRL with regularizer R(x) = (1/2)||x||^2, implemented via its closed form for
    quadratic losses f_t(x) = 0.5*||x - a_t||^2: the FTRL minimizer is the (regularized)
    running average of the a_t's."""
    T = len(loss_fns)
    cum_a = np.zeros(d)
    xs, x_hist = [], []
    for t in range(1, T + 1):
        # x_{t} minimizes eta * sum_{s<t} 0.5||x-a_s||^2 + 0.5||x||^2
        # closed form: x_t = eta * cum_a / (1 + eta*(t-1))
        x_t = (eta * cum_a) / (1 + eta * (t - 1)) if t > 1 else np.zeros(d)
        x_t = project_to_ball(x_t, radius)
        xs.append(x_t)
        a_t = grad_fns[t - 1](None)  # here grad_fns stores each round's center a_t directly
        cum_a += a_t
    return xs

rng = np.random.default_rng(0)
d, T, radius = 2, 500, 1.0
centers = rng.standard_normal((T, d)) * 0.3   # adversarial-ish but bounded sequence of targets

loss_fns = [lambda x, a=a: 0.5 * np.sum((x - a) ** 2) for a in centers]
grad_fns = [lambda x, a=a: (x - a) for a in centers]
ogd_xs = online_gradient_descent(grad_fns, np.zeros(d), D=2 * radius, G=2 * radius, radius=radius)

def regret_curve(xs, loss_fns, comparator):
    cum_alg, cum_cmp, regret = 0.0, 0.0, []
    for t, (x, f) in enumerate(zip(xs[1:], loss_fns)):
        cum_alg += f(x)
        cum_cmp += f(comparator)
        regret.append(cum_alg - cum_cmp)
    return np.array(regret)

x_star = np.clip(centers.mean(axis=0), -radius, radius)   # best fixed point in hindsight (approx.)
regret_ogd = regret_curve(ogd_xs, loss_fns, x_star)
print("Final OGD regret:", regret_ogd[-1], " theoretical O(sqrt(T)) scale:", 2 * radius * 2 * radius * np.sqrt(T))
```
Confirm the printed final regret is comfortably below the $DG\sqrt T$ scale (constants are not
tight — the bound is an upper bound, not an exact prediction), and that `regret_ogd` grows
visibly sublinearly (e.g., plot it against $\sqrt t$ and against $t$ to see which shape it tracks).

## 7. In-Class/Lab Exercise
Extend the §6 code to also implement FTRL's closed-form running-average update for the same
quadratic loss sequence, plot both algorithms' regret curves on the same axes, and discuss (2–3
sentences) why FTRL's running-average update and OGD's projected-gradient update can look similar
for quadratic losses specifically, while differing substantially for a loss sequence with sharp
corners (e.g., losses built from the absolute value function).
