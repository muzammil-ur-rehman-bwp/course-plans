# Week 9 — Lecture Content: Midterm + Weight Initialization & Vanishing/Exploding Gradients

## 1. Midterm Exam
Covers Weeks 1–8: the McCulloch-Pitts neuron and history; the perceptron and linear separability;
activation functions and their derivatives; the MLP and matrix-form forward propagation; loss
functions and the loss/activation pairing; gradient descent variants; the backpropagation
derivation and its from-scratch NumPy implementation.

## 2. Why All-Zero Initialization Fails
Suppose every weight in a hidden layer is initialized to the same value (e.g., all zero). Every
hidden unit then computes the identical $z$ for a given input, hence the identical activation
$a$, hence — by the backpropagation formulas from Week 7 — the identical $\delta$, hence the
identical weight gradient. Every update step keeps all units in a layer identical to each other,
forever. This is the **symmetry problem**: the network effectively has only *one* distinct hidden
unit's worth of representational capacity per layer, no matter how many units it nominally has.
Initializing weights to small *random* values breaks this symmetry, letting different units learn
different features.

```python
import numpy as np

# Zero init: every row of W1 is identical -> every hidden unit identical forever.
W1_zero = np.zeros((4, 2))

# Small random init: breaks symmetry.
rng = np.random.default_rng(0)
W1_random = rng.normal(0, 0.1, size=(4, 2))
```

## 3. Xavier/Glorot and He Initialization
Small random initialization breaks symmetry, but the *scale* of the random values still matters:
too large, and pre-activations $z$ grow large layer after layer (risking saturation/explosion);
too small, and they shrink toward zero (risking vanishing signal). Two widely used schemes choose
the initialization variance based on layer size, tuned to the activation function in use:

$$
\text{Xavier/Glorot (for sigmoid/tanh):} \quad W \sim \mathcal{N}\!\left(0, \frac{1}{n_{\text{in}}}\right) \text{ or } \mathcal{U}\!\left(-\sqrt{\tfrac{6}{n_{\text{in}}+n_{\text{out}}}}, \sqrt{\tfrac{6}{n_{\text{in}}+n_{\text{out}}}}\right)
$$
$$
\text{He (for ReLU):} \quad W \sim \mathcal{N}\!\left(0, \frac{2}{n_{\text{in}}}\right)
$$

where $n_{\text{in}}$ is the number of input units to that layer. Both are designed so that the
*variance* of activations (roughly) neither grows nor shrinks as data passes through many layers.
He initialization uses a larger variance ($2/n_{\text{in}}$ vs. $1/n_{\text{in}}$) because ReLU
zeroes out roughly half its inputs, so compensating variance keeps the surviving half's signal at
a comparable scale to Xavier's sigmoid/tanh case.

```python
def xavier_init(n_in, n_out, rng):
    limit = np.sqrt(6 / (n_in + n_out))
    return rng.uniform(-limit, limit, size=(n_out, n_in))

def he_init(n_in, n_out, rng):
    std = np.sqrt(2 / n_in)
    return rng.normal(0, std, size=(n_out, n_in))
```

## 4. Vanishing and Exploding Gradients (Conceptual)
Recall backpropagation's recursive rule: $\delta^{(l)} = (W^{(l+1)\top}\delta^{(l+1)}) \odot
g'(z^{(l)})$. Across $L$ layers, the gradient reaching an early layer involves a *product* of $L$
such factors (weight matrices and activation derivatives). Two failure modes follow directly:
- **Vanishing gradients:** if each factor's typical magnitude is less than 1 (e.g., sigmoid's
  derivative is at most 0.25, and is much smaller away from $z=0$), the product shrinks
  exponentially with depth, and early layers receive a gradient too small to learn from in a
  reasonable number of steps.
- **Exploding gradients:** if each factor's typical magnitude is greater than 1 (e.g., weights
  initialized too large), the product grows exponentially with depth, causing unstable, divergent
  updates.

This is why both **activation choice** (Week 3: ReLU's non-saturating derivative for $z>0$ helps
avoid vanishing gradients compared to sigmoid/tanh) and **initialization scale** (this week) matter
jointly — neither alone fully solves the problem in very deep networks, and this conceptual
introduction sets up Week 10's optimizers, which also partially compensate for difficult gradient
scales.

## 5. In-Class Exercise
For a 2-unit hidden layer initialized with identical weights $w_1 = w_2 = (0.3, 0.3)$, compute
$z$, $a$ (sigmoid), and then $\delta$ for both units given the same upstream gradient signal;
confirm symbolically that they remain equal, and explain in one sentence why this generalizes to
any number of identically-initialized units in a layer.
