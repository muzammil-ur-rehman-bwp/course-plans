# Week 7 — Lecture Content: Backpropagation Derivation

## 1. The Chain Rule, Reviewed
For a composition $L = f(g(\theta))$, the chain rule gives
$\frac{dL}{d\theta} = \frac{dL}{dg}\cdot\frac{dg}{d\theta}$. A neural network's loss is a long
composition of layers, so computing $\partial L/\partial W^{(l)}$ for an early layer $l$ requires
chaining derivatives through every layer between $l$ and the loss. **Backpropagation** is simply
an efficient, systematic way to apply the chain rule once, backward, and reuse intermediate
results across layers instead of recomputing them for each parameter separately.

## 2. Network and Notation
Consider the 2-layer network from Weeks 4–5: input $x \in \mathbb{R}^2$, hidden layer
$z^{(1)} = W^{(1)}x + b^{(1)}$, $a^{(1)} = \sigma(z^{(1)})$ (sigmoid, 2 units), output layer
$z^{(2)} = W^{(2)}a^{(1)} + b^{(2)}$, $\hat y = a^{(2)} = \sigma(z^{(2)})$ (sigmoid, 1 unit),
trained with binary cross-entropy loss $L$.

## 3. The Output-Layer Gradient
Define $\delta^{(2)} \equiv \partial L/\partial z^{(2)}$. From Week 5's derivation, for
cross-entropy paired with a sigmoid output:
$$
\delta^{(2)} = \hat y - y
$$
This single clean result is *why* cross-entropy is used — it makes the output layer's gradient
trivial to compute. From $\delta^{(2)}$, the parameter gradients follow directly by the chain
rule, since $z^{(2)} = W^{(2)}a^{(1)} + b^{(2)}$ is linear in $W^{(2)}, b^{(2)}$:
$$
\frac{\partial L}{\partial W^{(2)}} = \delta^{(2)} \, a^{(1)\top}, \qquad \frac{\partial L}{\partial b^{(2)}} = \delta^{(2)}
$$

## 4. The Hidden-Layer Gradient
To get $\delta^{(1)} \equiv \partial L/\partial z^{(1)}$, apply the chain rule through
$a^{(1)} = \sigma(z^{(1)})$ and through $z^{(2)} = W^{(2)}a^{(1)} + b^{(2)}$:
$$
\frac{\partial L}{\partial a^{(1)}} = W^{(2)\top}\delta^{(2)}, \qquad
\delta^{(1)} = \frac{\partial L}{\partial a^{(1)}} \odot \sigma'(z^{(1)}) = \bigl(W^{(2)\top}\delta^{(2)}\bigr) \odot a^{(1)}\odot(1-a^{(1)})
$$
where $\odot$ is element-wise multiplication (since each hidden unit's activation depends only on
its own $z^{(1)}_j$). This is the **general recursive backpropagation rule**: each layer's
$\delta$ is the next layer's $\delta$, projected backward through that layer's weights
($W^{(l+1)\top}\delta^{(l+1)}$), then scaled by the current layer's activation derivative. Given
$\delta^{(1)}$, the first-layer parameter gradients follow the same pattern as the output layer:
$$
\frac{\partial L}{\partial W^{(1)}} = \delta^{(1)} \, x^\top, \qquad \frac{\partial L}{\partial b^{(1)}} = \delta^{(1)}
$$

## 5. Worked Numeric Example
Let $x = (0.5, 0.8)$, $y = 1$, and:
$$
W^{(1)} = \begin{pmatrix}0.1 & 0.2\\0.3 & 0.4\end{pmatrix}, \; b^{(1)} = \begin{pmatrix}0.1\\0.1\end{pmatrix}, \;
W^{(2)} = \begin{pmatrix}0.5 & 0.6\end{pmatrix}, \; b^{(2)} = (0.2)
$$

**Forward pass:**
$$
z^{(1)} = \begin{pmatrix}0.31\\0.57\end{pmatrix}, \quad a^{(1)} = \begin{pmatrix}0.5769\\0.6388\end{pmatrix}, \quad
z^{(2)} = 0.8717, \quad \hat y = a^{(2)} = 0.7051
$$

**Backward pass:**
$$
\delta^{(2)} = \hat y - y = 0.7051 - 1 = -0.2949
$$
$$
\frac{\partial L}{\partial W^{(2)}} = \delta^{(2)}\,a^{(1)\top} = (-0.1702,\ -0.1884), \qquad
\frac{\partial L}{\partial b^{(2)}} = -0.2949
$$
$$
\frac{\partial L}{\partial a^{(1)}} = W^{(2)\top}\delta^{(2)} = \begin{pmatrix}0.5\\0.6\end{pmatrix}(-0.2949) = \begin{pmatrix}-0.1475\\-0.1770\end{pmatrix}
$$
$$
a^{(1)}\odot(1-a^{(1)}) = \begin{pmatrix}0.2441\\0.2308\end{pmatrix}, \qquad
\delta^{(1)} = \begin{pmatrix}-0.1475 \times 0.2441\\-0.1770 \times 0.2308\end{pmatrix} = \begin{pmatrix}-0.0360\\-0.0408\end{pmatrix}
$$
$$
\frac{\partial L}{\partial W^{(1)}} = \delta^{(1)} x^\top = \begin{pmatrix}-0.0360\times0.5 & -0.0360\times0.8\\-0.0408\times0.5 & -0.0408\times0.8\end{pmatrix} = \begin{pmatrix}-0.0180 & -0.0288\\-0.0204 & -0.0327\end{pmatrix}
$$
$$
\frac{\partial L}{\partial b^{(1)}} = \begin{pmatrix}-0.0360\\-0.0408\end{pmatrix}
$$

Every one of these six gradient quantities was produced by the same two-step recursive pattern:
compute $\delta$ for a layer, then read off that layer's weight/bias gradients directly from it.

## 6. In-Class Exercise
Starting from the forward-pass values given above, re-derive $\delta^{(2)}$, then
$\partial L/\partial W^{(2)}$ and $\partial L/\partial b^{(2)}$, by hand, showing every
intermediate number. Students who finish early continue to $\delta^{(1)}$.
