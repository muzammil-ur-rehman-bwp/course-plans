# Week 2 — Lecture Content: The Neural Tangent Kernel Revisited in Depth

## 1. Recap (One Sentence, Not Re-Derived)
The graduate course established: for a network $f(x;\theta)$, the Neural Tangent Kernel
$\Theta(x,x';\theta) = \nabla_\theta f(x;\theta)^\top \nabla_\theta f(x';\theta)$ converges, as
width $\to\infty$, to a fixed deterministic kernel $\Theta^\infty$, and training then behaves like
kernel regression against it. This week derives *why*.

## 2. The Linearization Argument
Consider training by gradient flow (continuous-time gradient descent) on a loss
$L(\theta) = \tfrac12\sum_{i} (f(x_i;\theta) - y_i)^2$. The parameter dynamics are
$$
\dot\theta_t = -\nabla_\theta L(\theta_t) = -\sum_i \nabla_\theta f(x_i;\theta_t)\,(f(x_i;\theta_t)-y_i).
$$
Now track the *function values* $f_t(x) := f(x;\theta_t)$ at an arbitrary point $x$ (training or
test) using the chain rule:
$$
\dot f_t(x) = \nabla_\theta f(x;\theta_t)^\top \dot\theta_t
= -\sum_i \underbrace{\nabla_\theta f(x;\theta_t)^\top \nabla_\theta f(x_i;\theta_t)}_{\Theta(x,x_i;\theta_t)} \,(f_t(x_i)-y_i).
$$
In matrix form over the training set, with $\Theta_t \in \mathbb{R}^{n\times n}$ the NTK Gram
matrix at time $t$ and $u_t = f_t(X) - y$ the residual vector:
$$
\dot u_t = -\Theta_t\, u_t.
$$
This is *exact* at every width — no approximation has been made yet. The entire content of the
infinite-width NTK result is the claim that, as width $\to\infty$ (under the NTK parameterization,
which scales each layer's weights by $1/\sqrt{\text{fan-in}}$), two things happen:
1. $\Theta_0 := \Theta(\theta_0)$ at initialization converges (in probability, over the random
   initialization) to a **deterministic** limit $\Theta^\infty$ — the randomness self-averages away
   because each entry of $\Theta_0$ is itself an average (inner product) over a growing number of
   hidden units.
2. $\Theta_t \approx \Theta_0$ stays **approximately constant** for the whole training run, so that
   $\dot u_t \approx -\Theta^\infty u_t$, a **linear, time-invariant** ODE with closed-form solution
   $u_t = e^{-\Theta^\infty t} u_0$ — exactly the dynamics of **kernel gradient descent** (and, at
   $t\to\infty$, kernel ridge-regression-like interpolation) against the fixed kernel $\Theta^\infty$.

**Why does claim (2) hold?** Differentiate $\Theta_t$ with respect to $\theta$: $\Theta_t$ depends
on $\theta_t$ only through the *Hessian* of $f$ (how much the gradient $\nabla_\theta f$ itself
changes as $\theta$ moves) and on how far $\theta_t$ has moved from $\theta_0$. Under the NTK
parameterization's $1/\sqrt{\text{width}}$ scaling, a standard (and nontrivial) computation shows
the Hessian's operator norm shrinks as the width grows, while the gradient norm $\|\nabla_\theta
f\|$ stays $O(1)$ — so, for any *fixed* finite amount of parameter movement, the resulting change
in $\Theta_t$ shrinks to zero as width $\to\infty$. Combined with the fact that the total parameter
movement needed to fit the (fixed-size) training set also stays bounded as width grows, this gives
$\Theta_t \to \Theta_0 = \Theta^\infty$ for the whole trajectory, not just at initialization.

## 3. Lazy Training, Named Precisely
Chizat and Bach's analysis names this regime **lazy training**: a training run in which the
*relative* parameter movement
$$
\frac{\|\theta_t - \theta_0\|}{\|\theta_0\|} \longrightarrow 0 \quad \text{as width} \to \infty,
$$
even though the *absolute* movement $\|\theta_t - \theta_0\|$ need not vanish — the parameters move
enough, in absolute terms, to fit the data, but that movement becomes negligible relative to the
(growing) scale of $\theta_0$ itself, which is precisely the condition under which the Hessian-based
argument in §2 goes through. "Lazy" names the *mechanism* (parameters barely move, relatively
speaking) behind the NTK limit's consequence (the kernel barely moves).

## 4. What This Reveals, and the Sharper Critique
**What it explains.** A theoretically rigorous account of *trainability*: in the infinite-width,
NTK-parameterized limit, fitting the training data is provably not navigating a complicated
non-convex landscape in function space — it is solving a linear ODE against a fixed positive
(semi-)definite kernel, which has no bad local minima, matching the graduate course's saddle-point
intuition from a completely different (and here, exact) angle.

**The sharper critique, beyond the graduate-course survey.** Three specific limitations:
1. **No feature learning, by construction.** Since $\Theta_t \approx \Theta_0$, the hidden-layer
   representations that *generate* $\Theta_0$ do not meaningfully change — only the final linear
   combination (in function space) implied by kernel regression adapts. If feature learning is
   genuinely responsible for realistic networks' strong generalization (Week 5's topic), the NTK
   limit cannot be the explanation for *that* part of the picture, by its own internal logic.
2. **NTK-regime generalization bounds are often far looser than what is observed.** Treating a
   trained network as kernel regression against $\Theta^\infty$ lets one import classical
   kernel-method generalization bounds — but these bounds, evaluated for realistic network widths
   and datasets, are typically numerically far weaker than the generalization actually observed,
   indicating the NTK approximation is not capturing whatever mechanism is actually responsible.
3. **Finite, practical-width networks measurably leave the lazy regime.** Empirically, the kernel
   $\Theta_t$ measured during training on real tasks at practically used widths drifts substantially
   from $\Theta_0$ — the lazy-training idealization is a limit, approached only as width grows very
   large, and the width at which practitioners get their best empirical results is often nowhere
   near that limit.

## 5. Code: Measuring Lazy Training and Kernel Drift Across Width
```python
import torch
import torch.nn as nn

torch.manual_seed(0)

class MLP(nn.Module):
    """NTK-parameterized one-hidden-layer network: 1/sqrt(fan_in) scaling."""
    def __init__(self, in_dim, width):
        super().__init__()
        self.w1 = nn.Parameter(torch.randn(width, in_dim) / in_dim**0.5)
        self.b1 = nn.Parameter(torch.zeros(width))
        self.w2 = nn.Parameter(torch.randn(width) / width**0.5)

    def forward(self, x):
        return torch.relu(x @ self.w1.T + self.b1) @ self.w2

def ntk_matrix(model, X):
    params = list(model.parameters())
    n = X.shape[0]
    grads = []
    for i in range(n):
        model.zero_grad()
        out = model(X[i:i+1])
        g = torch.autograd.grad(out, params, retain_graph=False, create_graph=False)
        grads.append(torch.cat([gi.flatten() for gi in g]))
    G = torch.stack(grads)
    return G @ G.T

def run(width, steps=2000, lr=0.3):
    model = MLP(in_dim=3, width=width)
    X = torch.randn(8, 3)
    y = torch.sin(X[:, 0]) + 0.5 * X[:, 1]
    theta0 = torch.cat([p.detach().flatten().clone() for p in model.parameters()])
    K0 = ntk_matrix(model, X)

    opt = torch.optim.SGD(model.parameters(), lr=lr)
    for _ in range(steps):
        opt.zero_grad()
        loss = 0.5 * ((model(X) - y) ** 2).sum()
        loss.backward()
        opt.step()

    theta_T = torch.cat([p.detach().flatten().clone() for p in model.parameters()])
    K_T = ntk_matrix(model, X)
    rel_move = (theta_T - theta0).norm() / theta0.norm()
    kernel_drift = (K_T - K0).norm() / K0.norm()
    return rel_move.item(), kernel_drift.item()

for width in [20, 200, 2000]:
    rel_move, drift = run(width)
    print(f"width={width:5d}  relative param movement={rel_move:.4f}  kernel drift={drift:.4f}")
```
As width grows, both `rel_move` and `kernel_drift` should shrink — the empirical signature of
approaching the lazy-training/NTK limit. Students are expected to see both quantities decreasing
but not vanishing even at the largest width tried here, consistent with §4's point that practical
widths sit meaningfully outside the strict infinite-width limit.

## 6. In-Class Exercise
A colleague claims: "Since the NTK is fixed during training, a trained infinite-width network and
an untrained one compute the same function." Identify precisely what is wrong with this claim —
distinguish "the *kernel* is fixed" from "the *function* is fixed," using the closed-form solution
$u_t = e^{-\Theta^\infty t}u_0$ in §2 to explain why the function changes substantially even though
the kernel generating its dynamics does not.
