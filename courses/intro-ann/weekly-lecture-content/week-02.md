# Week 2 — Lecture Content: The Perceptron and Linear Separability

## 1. From Fixed Weights to Learned Weights
The Week 1 McCulloch-Pitts neuron requires a human to choose weights and threshold by hand. The
**perceptron** (Rosenblatt, 1958) uses the same weighted-sum-plus-threshold structure, but adds an
algorithm that *learns* the weights from labeled examples.

## 2. The Perceptron Model
For input vector $\mathbf{x} \in \mathbb{R}^n$, weight vector $\mathbf{w} \in \mathbb{R}^n$, and
bias $b \in \mathbb{R}$:

$$
z = \mathbf{w}^\top \mathbf{x} + b, \qquad \hat{y} = \begin{cases} 1 & z \ge 0 \\ 0 & z < 0 \end{cases}
$$

The bias $b$ shifts the decision boundary away from the origin; without it, the separating
hyperplane would be forced to pass through $\mathbf{0}$.

## 3. The Perceptron Learning Rule
For each training example $(\mathbf{x}, y)$, predict $\hat{y}$, then update:

$$
\mathbf{w} \leftarrow \mathbf{w} + \eta (y - \hat{y}) \mathbf{x}, \qquad b \leftarrow b + \eta (y - \hat{y})
$$

where $\eta > 0$ is the learning rate. If the prediction is correct, $(y - \hat{y}) = 0$ and
nothing changes. If $\hat{y}$ is too low ($y=1,\hat{y}=0$), weights move *toward* $\mathbf{x}$; if
too high, they move *away* from it. The **Perceptron Convergence Theorem** guarantees this process
finds a separating hyperplane in finitely many steps, provided the data is linearly separable.

```python
import numpy as np

def perceptron_train(X, y, lr=0.1, epochs=20):
    n_features = X.shape[1]
    w = np.zeros(n_features)
    b = 0.0
    for _ in range(epochs):
        for xi, yi in zip(X, y):
            z = np.dot(w, xi) + b
            y_hat = 1 if z >= 0 else 0
            error = yi - y_hat
            w += lr * error * xi
            b += lr * error
    return w, b

def perceptron_predict(X, w, b):
    return (X @ w + b >= 0).astype(int)
```

## 4. Training on AND / OR
```python
X = np.array([[0, 0], [0, 1], [1, 0], [1, 1]])
y_and = np.array([0, 0, 0, 1])
y_or  = np.array([0, 1, 1, 1])

w_and, b_and = perceptron_train(X, y_and)
print("AND predictions:", perceptron_predict(X, w_and, b_and))  # [0 0 0 1]
```
Both AND and OR are linearly separable — a single straight line (in 2D) can separate the 1-labeled
points from the 0-labeled points — so the perceptron learning rule converges to weights that
classify every training example correctly.

## 5. Linear Separability and the XOR Problem
A dataset is **linearly separable** if some hyperplane $\mathbf{w}^\top\mathbf{x}+b=0$ places all
positive examples on one side and all negative examples on the other. XOR is the classic
counterexample:

| $x_1$ | $x_2$ | XOR |
|---|---|---|
| 0 | 0 | 0 |
| 0 | 1 | 1 |
| 1 | 0 | 1 |
| 1 | 1 | 0 |

Plotting these four points, the two "1" points ($(0,1)$ and $(1,0)$) sit on opposite corners from
the two "0" points ($(0,0)$ and $(1,1)$) — no single straight line separates them. Running
`perceptron_train` on XOR will not converge to a correct solution; weights will oscillate forever
without ever classifying all four points correctly. This was the core finding of Minsky & Papert's
1969 critique, and it is resolved not by a cleverer single perceptron but by **stacking multiple
perceptrons into layers** — the subject of Week 4's multi-layer perceptron.

## 6. In-Class Exercise
By hand, starting from $\mathbf{w}=(0,0)$, $b=0$, $\eta=1$, run the perceptron update rule for two
passes over the AND training set in the order listed above, writing out $z$, $\hat y$, and the
updated weights after each example. Then verify your final weights against the code above.
