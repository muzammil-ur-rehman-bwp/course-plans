# Week 2 — Lecture Content: Universal Approximation and Expressivity

## 1. Statement of the Theorem
**Universal Approximation Theorem (single hidden layer, informal statement).** Let $g$ be a
bounded, non-constant, continuous (sigmoidal) activation function. Then for any continuous
function $f:[0,1]^n \to \mathbb{R}$ and any $\epsilon > 0$, there exist an integer $N$, weights
$w_i \in \mathbb{R}^n$, biases $b_i \in \mathbb{R}$, and output weights $v_i \in \mathbb{R}$ such
that
$$
F(x) = \sum_{i=1}^N v_i\, g(w_i^\top x + b_i)
$$
satisfies $\sup_{x\in[0,1]^n} |F(x) - f(x)| < \epsilon$.

This is an **existence** result (Cybenko and Hornik's classical results, among others, establish
versions of this for sigmoidal and more general activations). It says nothing about:
- how large $N$ must be (no bound is given — it can be astronomically large for a hard $f$),
- whether gradient descent, started from a random initialization, will actually *find* such
  weights,
- whether the function the network ends up computing after training on finite data will
  generalize to new inputs.

## 2. Proof Sketch (1D case, for intuition)
Consider $f:[0,1]\to\mathbb{R}$ continuous. The construction proceeds in two steps.

**Step 1 — A sigmoid approximates a step function.** For a steep sigmoid $g(z) = \sigma(kz)$ with
large $k$, $g(x - t)$ transitions sharply from $\approx 0$ to $\approx 1$ as $x$ crosses $t$, so it
approximates the Heaviside step function $\mathbb{1}[x > t]$ as $k \to \infty$.

**Step 2 — A difference of two shifted steps approximates a localized "bump."** The combination
$$
\text{bump}_{t_1,t_2}(x) \approx \sigma(k(x - t_1)) - \sigma(k(x - t_2)), \qquad t_1 < t_2
$$
is $\approx 1$ on $(t_1, t_2)$ and $\approx 0$ outside it — two hidden units produce one localized
bump of adjustable height (via an output weight), width, and position.

**Step 3 — A sum of bumps approximates any continuous $f$.** Partition $[0,1]$ into $N$ small
intervals; on each interval, use one bump scaled to height $\approx f(\text{interval midpoint})$.
As $N\to\infty$ (intervals shrink), this piecewise-constant approximation converges uniformly to
$f$ by $f$'s continuity (and hence uniform continuity on the compact domain $[0,1]$). This is
exactly a single-hidden-layer network — $N$ grows with the desired precision, which is why the
theorem gives no fixed bound on width.

## 3. Depth-versus-Width Expressivity
The theorem says one hidden layer *suffices in principle* — it does not say one hidden layer is
*efficient*. A line of **depth-separation** results (e.g., Telgarsky's work on the benefits of
depth) exhibits specific function families — built from repeated composition of simple sawtooth
or triangle-wave functions — that:
- can be represented **exactly** by a network of depth $k$ with a number of units that grows only
  *polynomially* in $k$, but
- require a shallow (depth-2) network to use a number of units that grows *exponentially* in $k$
  to approximate even crudely.

The intuition: composing $k$ simple piecewise-linear "folding" maps produces a function with
$O(2^k)$ linear pieces from only $O(k)$ units (each layer roughly doubles the piece count), while
a shallow network's piece count grows only linearly in its unit count. Depth buys *multiplicative*
expressivity growth that width alone cannot match for these function families. This is a
conceptual, qualitative takeaway for this course — the formal separation theorems are detailed
enough to be their own research topic and are not re-derived here.

## 4. Code: Width vs. Approximation Error
```python
import numpy as np

def target_fn(x):
    return np.sin(6 * x) + 0.5 * np.sin(17 * x)

def fit_shallow_network(x_train, y_train, width, epochs=3000, lr=0.05, seed=0):
    rng = np.random.default_rng(seed)
    n = 1
    W1 = rng.normal(0, 1.0, size=(width, n))
    b1 = rng.normal(0, 1.0, size=(width,))
    W2 = rng.normal(0, 1.0 / np.sqrt(width), size=(width,))
    b2 = 0.0
    for _ in range(epochs):
        z1 = x_train[:, None] @ W1.T + b1          # (m, width)
        a1 = np.tanh(z1)
        y_hat = a1 @ W2 + b2                        # (m,)
        err = y_hat - y_train                        # dL/dy_hat for 0.5*MSE
        gW2 = a1.T @ err / len(x_train)
        gb2 = err.mean()
        da1 = err[:, None] * W2[None, :]
        dz1 = da1 * (1 - a1 ** 2)
        gW1 = dz1.T @ x_train[:, None] / len(x_train)
        gb1 = dz1.mean(axis=0)
        W2 -= lr * gW2; b2 -= lr * gb2
        W1 -= lr * gW1; b1 -= lr * gb1
    return W1, b1, W2, b2

x_train = np.linspace(0, 1, 200)
y_train = target_fn(x_train)
for width in [2, 8, 32, 128]:
    W1, b1, W2, b2 = fit_shallow_network(x_train, y_train, width)
    y_hat = np.tanh(x_train[:, None] @ W1.T + b1) @ W2 + b2
    mse = np.mean((y_hat - y_train) ** 2)
    print(f"width={width:4d}  train MSE={mse:.5f}")
```
Running this shows mean squared error dropping sharply as width grows from 2 to 128 — a direct,
small-scale illustration of the approximation theorem's content: more units, better fit, with no
guarantee of *how many* units a given target needs in advance.

## 5. In-Class Exercise
For a target function with 3 well-separated bumps, sketch how many hidden units the Step 1–3
construction needs in the worst case, and explain why a smoother target (fewer, wider bumps)
needs fewer units under this same construction.
