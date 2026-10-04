# Week 4 — Lecture Content: Initialization Theory

## 1. Why All-Zero Initialization Fails
If every weight in a layer is initialized to the same value (e.g., $0$), every unit in that layer
computes the same pre-activation, the same activation, and — crucially — the same gradient during
backpropagation (since $\partial L/\partial W_{ij}$ depends on $\delta_i$ and $a_j$, and all
$\delta_i$ within a layer are identical by symmetry). Every unit in the layer updates identically
forever: the layer never breaks symmetry and effectively behaves as a single unit no matter how
many units it has. This is the **symmetry-breaking problem**, and it is why initialization must be
random, not merely small.

## 2. Why Naive Random Initialization Fails
Let a layer compute $z = Wx$ with $W_{ij} \stackrel{iid}{\sim} \mathcal{N}(0, \sigma^2)$ and
$x_j$ independent of $W$ with $\mathrm{Var}(x_j) = v$. For fixed $x$, treating $W$ as the random
variable:
$$
\mathrm{Var}(z_i) = \mathrm{Var}\Big(\sum_{j=1}^{n_{in}} W_{ij}x_j\Big) = \sum_{j=1}^{n_{in}} \mathrm{Var}(W_{ij})\,x_j^2 \approx n_{in}\,\sigma^2 v
$$
(the approximation treats $\sum_j x_j^2 \approx n_{in} v$ for a typical input). If $\sigma^2$ is a
*fixed* constant independent of $n_{in}$ (e.g., $\sigma=1$ "naive" initialization), then
$\mathrm{Var}(z_i)$ **grows linearly with fan-in** — pre-activations and, after repeated layers,
outputs explode geometrically with depth. If $\sigma^2$ is instead too small, variance shrinks
geometrically with depth and activations collapse toward zero. Either way, signal is destroyed by
the time it reaches a deep layer, and the same multiplicative blow-up/collapse afflicts gradients
propagating backward. The fix, in both cases, is to **scale $\sigma^2$ by $1/n_{in}$** so that
variance is preserved regardless of layer width.

## 3. Deriving Xavier/Glorot Initialization
Xavier/Glorot initialization asks for variance preservation in **both directions**: forward
(activations) and backward (gradients), for an activation function that is approximately linear
near $0$ (tanh, or sigmoid after centering) so that $\mathrm{Var}(a) \approx \mathrm{Var}(z)$.

**Forward pass** ($z^{(l)} = W^{(l)}a^{(l-1)}$, fan-in $n_{in}$): from Section 2,
$\mathrm{Var}(z^{(l)}) = n_{in}\sigma^2\,\mathrm{Var}(a^{(l-1)})$. Preserving variance
($\mathrm{Var}(z^{(l)}) = \mathrm{Var}(a^{(l-1)})$) requires $\sigma^2 = 1/n_{in}$.

**Backward pass** (gradient flows through $W^{(l)\top}$, "fan-in" $n_{out}$ from the backward
direction): by the symmetric argument on $\delta^{(l-1)} = W^{(l)\top}\delta^{(l)}\odot(\cdots)$,
preserving gradient variance requires $\sigma^2 = 1/n_{out}$.

These two requirements generally conflict when $n_{in}\neq n_{out}$. Xavier/Glorot's compromise
is their harmonic-mean-flavored average:
$$
\mathrm{Var}(W) = \frac{2}{n_{in}+n_{out}}, \qquad \text{(uniform variant: } W \sim \mathcal{U}\Big(-\sqrt{\tfrac{6}{n_{in}+n_{out}}},\ \sqrt{\tfrac{6}{n_{in}+n_{out}}}\Big)\text{)}
$$
which preserves variance exactly only when $n_{in}=n_{out}$, and approximately otherwise.

## 4. Deriving He Initialization
For ReLU, $a = \max(0, z)$. If $z$ is symmetric about $0$ (true at initialization for
zero-mean Gaussian pre-activations), then ReLU zeroes exactly half of the distribution's mass, and
for zero-mean Gaussian $z$:
$$
\mathbb{E}[a^2] = \mathbb{E}[\max(0,z)^2] = \tfrac{1}{2}\,\mathbb{E}[z^2] = \tfrac{1}{2}\mathrm{Var}(z)
$$
(exploiting the symmetry of a zero-mean Gaussian: the positive half contributes exactly half of
$\mathbb{E}[z^2]$). So $\mathrm{Var}(a) = \tfrac{1}{2}\mathrm{Var}(z)$ — ReLU halves variance
every layer, on top of whatever the weights do. Redo the forward-variance-preservation argument
with this correction:
$$
\mathrm{Var}(z^{(l)}) = n_{in}\sigma^2\,\mathrm{Var}(a^{(l-1)}) = n_{in}\sigma^2 \cdot \tfrac{1}{2}\mathrm{Var}(z^{(l-1)})
$$
Preserving variance ($\mathrm{Var}(z^{(l)})=\mathrm{Var}(z^{(l-1)})$) now requires
$n_{in}\sigma^2/2 = 1$, i.e.
$$
\mathrm{Var}(W) = \frac{2}{n_{in}} \qquad \textbf{(He initialization)}
$$
— exactly Xavier's forward-only formula multiplied by $2$, which is precisely the correction
ReLU's variance-halving requires.

## 5. Code: Activation Variance Across Depth
```python
import numpy as np

def run_depth_experiment(init_scheme, activation, depth=30, width=256, n_samples=512, seed=0):
    rng = np.random.default_rng(seed)
    a = rng.normal(0, 1, size=(n_samples, width))      # unit-variance "input"
    variances = [a.var()]
    for _ in range(depth):
        if init_scheme == "zero":
            W = np.zeros((width, width))
        elif init_scheme == "naive":
            W = rng.normal(0, 1.0, size=(width, width))
        elif init_scheme == "xavier":
            W = rng.normal(0, np.sqrt(2.0 / (width + width)), size=(width, width))
        elif init_scheme == "he":
            W = rng.normal(0, np.sqrt(2.0 / width), size=(width, width))
        z = a @ W.T
        a = np.tanh(z) if activation == "tanh" else np.maximum(0, z)
        variances.append(a.var())
    return variances

for scheme in ["naive", "xavier"]:
    v = run_depth_experiment(scheme, "tanh")
    print(f"{scheme:8s} (tanh): var at layer 0/15/30 = {v[0]:.4f} / {v[15]:.6f} / {v[30]:.8f}")

for scheme in ["naive", "he"]:
    v = run_depth_experiment(scheme, "relu")
    print(f"{scheme:8s} (relu): var at layer 0/15/30 = {v[0]:.4f} / {v[15]:.6f} / {v[30]:.8f}")
```
Expect "naive" variance to decay (tanh) or decay/blow up (relu, depending on the exact constant)
across depth, while "xavier" (with tanh) and "he" (with relu) hold activation variance roughly
constant across all 30 layers — the empirical signature of the Section 3–4 derivations.

## 6. In-Class Exercise
For a layer with fan-in $n=100$, compute $\mathrm{Var}(W)$ under Xavier ($n_{in}=n_{out}=100$)
and under He, and verify the ratio is exactly $2$.
