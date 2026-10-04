# Week 13 — Lecture Content: Introduction to Neural Networks (Survey)

**Scope note:** like Week 12, this is a brief conceptual survey — enough to understand what a
perceptron is and why it matters historically and pedagogically, not a deep-learning course.
Backpropagation and multi-layer training are described conceptually only; they are covered in
depth in this curriculum's dedicated Neural Networks and Deep Learning courses.

## 1. Biological Inspiration (Brief, and Loose)
Artificial neurons are loosely inspired by biological neurons: multiple inputs are combined and,
if the combined signal is strong enough, the neuron "fires." The analogy is useful for intuition
but should not be taken literally — artificial neural networks are a mathematical/computational
model, not a simulation of biological brains.

## 2. The Perceptron
A perceptron computes a weighted sum of its inputs plus a bias, then applies a step activation
function:

```python
def step(x):
    return 1 if x >= 0 else 0

class Perceptron:
    def __init__(self, n_inputs, learning_rate=0.1):
        self.weights = [0.0] * n_inputs
        self.bias = 0.0
        self.lr = learning_rate

    def predict(self, inputs):
        total = sum(w * x for w, x in zip(self.weights, inputs)) + self.bias
        return step(total)

    def train_step(self, inputs, target):
        prediction = self.predict(inputs)
        error = target - prediction
        for i in range(len(self.weights)):
            self.weights[i] += self.lr * error * inputs[i]
        self.bias += self.lr * error
        return error
```

## 3. The Perceptron Learning Rule
For a misclassified example, the rule nudges each weight in the direction that would have made
the prediction correct: `w_i <- w_i + lr * (target - prediction) * x_i`. This is guaranteed to
converge to a separating hyperplane **if and only if** the data is linearly separable.

```python
AND_DATA = [([0, 0], 0), ([0, 1], 0), ([1, 0], 0), ([1, 1], 1)]

def train(perceptron, data, epochs=20):
    for epoch in range(epochs):
        total_error = 0
        for inputs, target in data:
            total_error += abs(perceptron.train_step(inputs, target))
        if total_error == 0:
            print(f"converged after {epoch + 1} epochs")
            break

p = Perceptron(n_inputs=2)
train(p, AND_DATA)
for inputs, target in AND_DATA:
    print(inputs, "->", p.predict(inputs), "(expected", target, ")")
```

## 4. Linear Separability and Why XOR Fails
AND and OR are **linearly separable**: a single straight line (in 2D) can separate the positive
and negative examples. XOR is not — no single line separates `(0,0)->0, (1,1)->0` from
`(0,1)->1, (1,0)->1`. Running the same `Perceptron`/`train` code on XOR data will never converge
to zero error, which students should verify directly:

```python
XOR_DATA = [([0, 0], 0), ([0, 1], 1), ([1, 0], 1), ([1, 1], 0)]
p_xor = Perceptron(n_inputs=2)
train(p_xor, XOR_DATA, epochs=50)  # total_error never reaches 0
```

## 5. Why Deep Learning Took Off (Conceptual, No Derivation)
Stacking perceptron-like units into **multiple layers**, with differentiable activation
functions and the backpropagation algorithm to compute gradients, overcomes the linear-
separability limit — a multi-layer network *can* represent XOR and far more complex functions.
Historically, three factors converged in the 2010s to make deep (many-layer) networks practical
at scale: (1) much larger labeled datasets, (2) GPU hardware for fast matrix computation, and
(3) algorithmic/architectural refinements (better activation functions, initialization schemes,
normalization). None of this is a replacement for the classical techniques earlier in this
course — it is a different, complementary tool, most effective when a large amount of labeled
data is available and the rules are too complex to hand-specify (as in Weeks 6–9's logic and
planning).

## 6. In-Class Exercise
Hand-trace two perceptron weight updates on a misclassified AND example (pick specific starting
weights), then predict — and verify by running the code — that the same approach cannot drive
the error on XOR to zero.
