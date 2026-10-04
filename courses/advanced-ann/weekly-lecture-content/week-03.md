# Week 3 — Lecture Content: Mean-Field Theory of Neural Networks

## 1. A Different Width-→-∞ Limit
Week 2's NTK limit tracks a single scalar-valued object per pair of inputs — the kernel
$\Theta(x,x')$ — and shows it becomes fixed. The **mean-field limit** instead tracks a richer
object: the *empirical distribution* of a layer's incoming weights (or, equivalently for a
two-layer network, of the pairs $(w_j, v_j)$ of a hidden unit $j$'s input weights and output
weight) over the $m$ hidden units, viewed as $m$ i.i.d. (at initialization) samples from some
distribution $\rho_0$. As $m \to \infty$, a law-of-large-numbers argument lets one replace a sum
over units, $\frac1m\sum_j g(w_j,v_j)$, by an expectation $\mathbb{E}_{(w,v)\sim\rho}[g(w,v)]$ —
and gradient descent on the finite-$m$ network becomes, in this limit, a **deterministic PDE**
(technically, a Wasserstein-gradient-flow / continuity equation) describing how the *distribution*
$\rho_t$ evolves over training time, rather than how finitely many individual parameters evolve.

## 2. Why Two Different Infinite-Width Limits Exist
Both limits start from "a network with a very wide hidden layer," but they use **different scaling
conventions** for the output layer, and that single choice changes everything:
- **NTK parameterization:** output layer scaled by $1/\sqrt{m}$ (as in Week 2). Each hidden unit's
  individual contribution to the output is $O(1/\sqrt m)$ — vanishingly small — so no single unit's
  movement matters on its own; what survives in the limit is the *aggregate* second-order object
  (the kernel), and the relative movement of any individual unit's parameters stays negligible
  (lazy training).
- **Mean-field parameterization:** output layer scaled by $1/m$, with the compensating fact that
  there are $m$ units each now treated as a sample from a *distribution* that itself is allowed to
  move by $O(1)$ amounts. Individual units' parameters can move substantially (relative to their
  own scale) under this convention, and the limiting object that stays well-defined as $m\to\infty$
  is the *distribution* $\rho_t$, not a fixed kernel.

**The key conceptual payoff:** the mean-field limit, under its own natural scaling, is a genuine
width-$\to\infty$ idealization of a wide network that *does* allow feature learning — the
distribution of hidden-unit weights can reshape itself over training to fit the task, in direct
contrast to the NTK limit's frozen kernel. Both are legitimate, rigorous infinite-width limits of
"a wide network"; they are simply different limits, obtained by different (and mutually exclusive,
for the same finite-width network) scaling choices of the same construction. Neither is "the"
correct account of what a *finite*, practically-sized network does — Week 5 returns to exactly this
question of where practical networks actually sit.

## 3. Signal Propagation in the Mean-Field Regime
Mean-field analysis is not only about training dynamics — it also describes *forward* signal
propagation through depth in a wide random network at initialization, which is precisely the
graduate course's initialization-theory question, generalized. Consider a fully-connected network
with activation $\phi$, weights $W^l_{ij} \sim \mathcal N(0, \sigma_w^2/n_{l-1})$, biases
$b^l_i \sim \mathcal N(0,\sigma_b^2)$. Define the mean-field pre-activation variance at layer $l$,
$q^l := \frac1{n_l}\sum_i (h^l_i)^2$ (self-averaging over the layer as width $\to\infty$). It obeys
the recursion
$$
q^{l+1} = \sigma_w^2\, \mathbb E_{z\sim\mathcal N(0,1)}\!\big[\phi(\sqrt{q^l}\,z)^2\big] + \sigma_b^2 .
$$
This is the width-$\to\infty$, *distributional* statement of exactly the variance-propagation
argument behind Xavier/He initialization. Two useful special cases recover the graduate course's
formulas directly:
- **Linear/small-signal regime** ($\phi(x)\approx x$, as in a linearized tanh near 0): the
  recursion becomes $q^{l+1} = \sigma_w^2 q^l + \sigma_b^2$, and demanding a fixed point
  $q^\star = q^{l+1}=q^l$ with $\sigma_b=0$ forces $\sigma_w^2 = 1$ — the Xavier/Glorot
  variance-preservation condition, recovered as the fixed point of this more general recursion.
- **ReLU** ($\phi(x)=\max(0,x)$, so $\mathbb E[\phi(\sqrt{q}z)^2] = \tfrac12 q$ since ReLU zeroes
  exactly half the symmetric Gaussian input): the fixed-point condition becomes
  $\sigma_w^2 = 2$ — **He initialization**, recovered the same way.
A second recursion tracks the **correlation** $c^l$ between two different inputs' pre-activations
at the same layer; its fixed points reveal an **order-to-chaos transition** as $\sigma_w$ varies —
for small $\sigma_w$ nearby inputs' representations become identical deep in the network ("order":
$c^l \to 1$), while above a critical $\sigma_w$ they decorrelate exponentially fast regardless of
input similarity ("chaos": $c^l \to$ some $c^\star < 1$ with a finite correlation length in depth).
Networks initialized very close to this critical boundary (the "edge of chaos") empirically support
training at much greater depth than networks initialized away from it — connecting mean-field
signal-propagation theory directly to a practical initialization criterion.

## 4. Code: Variance and Correlation Propagation vs. Xavier/He
```python
import numpy as np

def relu(x): return np.maximum(0, x)

def propagate_variance(phi, sigma_w2, sigma_b2, depth, q0=1.0, n_mc=200000):
    q = q0
    trace = [q]
    z = np.random.randn(n_mc)
    for _ in range(depth):
        q = sigma_w2 * np.mean(phi(np.sqrt(q) * z) ** 2) + sigma_b2
        trace.append(q)
    return trace

depth = 40
# Xavier-matched linear/tanh-small-signal regime: sigma_w^2 = 1 should hold q roughly fixed.
trace_lin = propagate_variance(lambda x: x, sigma_w2=1.0, sigma_b2=0.0, depth=depth)
# He-matched ReLU regime: sigma_w^2 = 2 should hold q roughly fixed.
trace_relu = propagate_variance(relu, sigma_w2=2.0, sigma_b2=0.0, depth=depth)
# Mis-scaled ReLU (sigma_w^2 = 1, i.e. Xavier used on ReLU): q should decay geometrically.
trace_relu_mis = propagate_variance(relu, sigma_w2=1.0, sigma_b2=0.0, depth=depth)

print("Linear, Xavier-scaled, final q:  ", round(trace_lin[-1], 4))
print("ReLU, He-scaled, final q:        ", round(trace_relu[-1], 4))
print("ReLU, Xavier-scaled (mis-set) q: ", round(trace_relu_mis[-1], 4))
```
The first two traces should stay close to `q0 = 1.0` across all 40 layers (the fixed point is
preserved by construction); the third should decay sharply toward zero, reproducing — from the
general mean-field recursion rather than a special-cased derivation — exactly the vanishing-
activation failure mode the graduate course used to motivate He initialization in the first place.

## 5. In-Class Exercise
Using the variance recursion in §3, derive the fixed-point condition on $\sigma_w^2$ for the
**leaky-ReLU** activation $\phi(x) = \max(x, \alpha x)$ with $0<\alpha<1$, and check that it reduces
to He's condition ($\sigma_w^2=2$) as $\alpha \to 0$ and to the linear condition ($\sigma_w^2=1$) as
$\alpha \to 1$.
