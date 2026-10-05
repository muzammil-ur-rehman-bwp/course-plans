# Week 14: Neural Networks I

## Learning Objectives

By the end of this lecture, you should be able to:

1. Describe an artificial neuron and train a single perceptron.
2. Explain why a single perceptron cannot learn XOR, and how a hidden layer fixes this.
3. Implement and compare the sigmoid, ReLU and softmax activation functions.
4. Write the forward pass of a two-layer network with NumPy, and compute one by hand.
5. Match loss functions to tasks: mean squared error, binary and categorical cross-entropy.

## 1. The Perceptron

A neural network is built from small units that are loosely inspired by nerve cells. The simplest is the artificial neuron, also called the perceptron. It multiplies each input by a weight, adds the results together with a bias, and passes the total through an activation function.

```
z = w1*x1 + w2*x2 + ... + wn*xn + b
output = activation(z)
```

In the original perceptron the activation is a step function: the output is 1 if `z` is positive and 0 otherwise. You have already met this idea. It is the linear model from Week 10 followed by a threshold. The weights encode how much each input matters, and the bias moves the threshold.

```python
import numpy as np

def step(z):
    return (z > 0).astype(int)

# A neuron that computes logical AND of two binary inputs
w = np.array([1.0, 1.0])
b = -1.5

inputs = np.array([[0, 0], [0, 1], [1, 0], [1, 1]])
print(step(inputs @ w + b))      # [0 0 0 1]
```

Check why this works. Only when both inputs are 1 does the sum reach 2, which exceeds 1.5. With one input on, the sum is 1, which is below 1.5. By choosing `b = -0.5` instead, the same weights compute OR.

### 1.1 Learning the weights

We should not need to choose weights by hand. The perceptron learning rule adjusts them from examples. For each training example, compute the prediction. If it is wrong, nudge the weights towards the correct answer.

```
w := w + lr * (target - prediction) * x
b := b + lr * (target - prediction)
```

```python
def train_perceptron(X, y, lr=0.1, epochs=50):
    w = np.zeros(X.shape[1])
    b = 0.0
    for epoch in range(epochs):
        errors = 0
        for xi, target in zip(X, y):
            pred = int(xi @ w + b > 0)
            update = lr * (target - pred)
            w += update * xi
            b += update
            errors += int(update != 0)
        if errors == 0:
            return w, b, epoch + 1
    return w, b, epochs

X = np.array([[0, 0], [0, 1], [1, 0], [1, 1]])
for name, y in [("AND", [0, 0, 0, 1]), ("OR", [0, 1, 1, 1]), ("XOR", [0, 1, 1, 0])]:
    w, b, used = train_perceptron(X, np.array(y))
    pred = step(X @ w + b)
    print(f"{name:3s} epochs used: {used:3d}  predictions: {pred}  correct: {(pred == y).all()}")
```

For AND and OR the loop finishes after a few epochs with correct predictions. For XOR it runs through all 50 epochs and never gets all four right.

### 1.2 Why XOR fails

XOR outputs 1 when exactly one input is 1. Plot the four points. The points (0,1) and (1,0) have the label 1, and (0,0) and (1,1) have the label 0. No single straight line can separate the two groups, because the classes sit on opposite diagonals. A perceptron draws exactly one straight line, since its boundary is `w.x + b = 0`. We say it can only represent linearly separable functions.

This limitation was famously pointed out by Minsky and Papert in 1969, and it slowed research on neural networks for years. The remedy is to stack neurons into layers.

### 1.3 A hidden layer solves it

The next network has two hidden neurons and one output neuron, and its weights are set by hand. The hidden neurons compute `relu(x1 + x2)` and `relu(x1 + x2 - 1)`, and the output computes `h1 - 2*h2`.

```python
def relu(z):
    return np.maximum(0, z)

W1 = np.array([[1.0, 1.0],
               [1.0, 1.0]])
b1 = np.array([0.0, -1.0])
W2 = np.array([1.0, -2.0])
b2 = 0.0

for x in X:
    h = relu(W1 @ x + b1)
    out = W2 @ h + b2
    print(x, "->", h, "->", out)
```

The output is 0, 1, 1, 0, which is XOR. The hidden layer has transformed the inputs into a new representation in which the problem becomes linearly separable. This is the core idea of deep learning: layers learn useful representations of their inputs.

## 2. Activation Functions

If we stack layers without a non-linear activation, nothing is gained. Two linear layers in a row, `W2 @ (W1 @ x)`, collapse into one linear layer `(W2 @ W1) @ x`. The activation function between the layers is what gives a network its power.

1. Sigmoid, `1 / (1 + e^(-z))`. It squashes any input to the range between 0 and 1, so it is used at the output of a binary classifier to give a probability. In deep hidden layers it has a problem: for large positive or negative inputs the curve is nearly flat, so gradients become tiny, and learning slows down. This is the vanishing gradient problem.
2. ReLU, `max(0, z)`. It is cheap, and for positive inputs its gradient is exactly 1, so it avoids vanishing gradients much better. It is the usual default for hidden layers. A unit whose input is always negative outputs zero and receives no gradient, which is sometimes called a dead unit.
3. Softmax, `e^(z_i) / sum_j e^(z_j)`. It turns a vector of scores into a probability distribution that sums to 1, so it is used at the output of a multi-class classifier.

```python
def sigmoid(z):
    return 1 / (1 + np.exp(-z))

def relu(z):
    return np.maximum(0, z)

def softmax(z):
    exp_z = np.exp(z - np.max(z))   # subtract max for numerical stability
    return exp_z / exp_z.sum()

z = np.array([-5.0, -1.0, 0.0, 1.0, 5.0])
print("sigmoid:", sigmoid(z).round(3))
print("relu:   ", relu(z))

scores = np.array([2.0, 1.0, 0.1])
p = softmax(scores)
print("softmax:", p.round(3), "sum =", p.sum().round(3))
```

Why subtract the maximum in softmax? Without it, an input such as 1000 makes `e^1000` overflow to infinity. Subtracting a constant from all scores does not change the result, since it cancels between numerator and denominator, and it keeps the numbers manageable. Try it.

```python
big = np.array([1000.0, 1001.0, 1002.0])
print("stable:  ", softmax(big).round(3))
with np.errstate(all="ignore"):
    naive = np.exp(big) / np.exp(big).sum()
print("naive:   ", naive)
```

The naive version returns `nan` values.

A simple plot helps in remembering the shapes.

```python
import matplotlib.pyplot as plt

zs = np.linspace(-6, 6, 200)
plt.plot(zs, sigmoid(zs), color="black", label="sigmoid")
plt.plot(zs, relu(zs), color="black", linestyle="--", label="relu")
plt.plot(zs, np.tanh(zs), color="gray", label="tanh")
plt.ylim(-1.5, 3)
plt.xlabel("z")
plt.legend()
plt.title("Common activation functions")
plt.show()
```

## 3. Forward Propagation From Scratch

Pushing an input through the network to obtain an output is called the forward pass. For a network with one hidden layer, where the hidden layer uses ReLU and the output uses sigmoid:

```python
# A 2-layer network: input -> hidden (ReLU) -> output (sigmoid)
def forward_pass(x, W1, b1, W2, b2):
    z1 = W1 @ x + b1
    a1 = relu(z1)
    z2 = W2 @ a1 + b2
    a2 = sigmoid(z2)
    return a2
```

This is exactly the pattern `W @ x + b` that we saw in Week 3, applied twice with a non-linearity in between. Matrix shapes are the thing to watch. If the input has 3 features and the hidden layer has 4 units, then `W1` has shape `(4, 3)`, and `b1` has shape `(4,)`. With one output, `W2` has shape `(1, 4)`.

```python
rng = np.random.default_rng(0)
W1 = rng.normal(size=(4, 3))
b1 = np.zeros(4)
W2 = rng.normal(size=(1, 4))
b2 = np.zeros(1)

x = np.array([0.5, -1.0, 2.0])
print("output probability:", forward_pass(x, W1, b1, W2, b2))
```

With random weights, the output is meaningless. Training, which we do next week, is the process of finding weights that give sensible outputs.

### 3.1 Processing many examples at once

In practice we feed in a whole batch of inputs together, stored as a matrix with one row per example. Then the layer computes `X @ W.T + b`, using broadcasting for the bias.

```python
def forward_batch(X, W1, b1, W2, b2):
    A1 = relu(X @ W1.T + b1)
    return sigmoid(A1 @ W2.T + b2)

Xb = rng.normal(size=(5, 3))
print(forward_batch(Xb, W1, b1, W2, b2).ravel().round(3))
print("same first row as single-example version:",
      np.allclose(forward_batch(Xb[:1], W1, b1, W2, b2).ravel(),
                  forward_pass(Xb[0], W1, b1, W2, b2)))
```

This is the vectorization lesson from Week 3 once more. Libraries such as PyTorch work on batches because it is far faster than looping over examples.

## 4. Loss Functions

A loss function measures how far the network's output is from the target, and training means making it small. The loss should match the type of output.

1. Mean squared error, for regression: `mean((y_pred - y_true)^2)`.
2. Binary cross-entropy, for a binary output after a sigmoid: `-[y log(p) + (1 - y) log(1 - p)]`.
3. Categorical cross-entropy, for a multi-class output after a softmax: `-sum_k y_k log(p_k)`, where `y` is a one-hot vector. Because only one component of `y` is 1, this is just the negative log of the probability given to the true class.

```python
def mse(y_true, y_pred):
    return np.mean((y_pred - y_true) ** 2)

def binary_cross_entropy(y_true, p, eps=1e-12):
    p = np.clip(p, eps, 1 - eps)
    return -np.mean(y_true * np.log(p) + (1 - y_true) * np.log(1 - p))

def categorical_cross_entropy(true_class, probs, eps=1e-12):
    return -np.log(np.clip(probs[true_class], eps, 1.0))

print("confident and right:", round(binary_cross_entropy(np.array([1]), np.array([0.99])), 3))
print("unsure:             ", round(binary_cross_entropy(np.array([1]), np.array([0.5])), 3))
print("confident and wrong:", round(binary_cross_entropy(np.array([1]), np.array([0.01])), 3))

print("softmax example loss:", round(categorical_cross_entropy(0, softmax(np.array([2.0, 1.0, 0.1]))), 3))
```

Notice how cross-entropy treats a confident wrong answer, with a loss of about 4.6, compared with an unsure answer at 0.69. This harsh penalty is what pushes the network to be honest about its uncertainty. Squared error would be much gentler, which is one reason cross-entropy is preferred for classification.

The clipping with `eps` avoids taking the logarithm of exactly zero.

## 5. Worked Example: Hand Computation

Consider a network with 2 inputs, 2 hidden units with ReLU, and 1 output with sigmoid. The input is `x = [1, 2]`, and the weights are:

```
W1 = [[ 0.5, -0.2],     b1 = [ 0.1, -0.1]
      [ 0.3,  0.8]]
W2 = [[ 1.0, -1.5]]     b2 = [ 0.2]
```

Step by step:

1. Hidden unit 1: `z = 0.5*1 + (-0.2)*2 + 0.1 = 0.5 - 0.4 + 0.1 = 0.2`. ReLU gives 0.2.
2. Hidden unit 2: `z = 0.3*1 + 0.8*2 - 0.1 = 0.3 + 1.6 - 0.1 = 1.8`. ReLU gives 1.8.
3. Output pre-activation: `z = 1.0*0.2 + (-1.5)*1.8 + 0.2 = 0.2 - 2.7 + 0.2 = -2.3`.
4. Sigmoid: `1 / (1 + e^(2.3)) = 1 / (1 + 9.974) = 0.0911`.

The network says the probability of class 1 is about 0.09. Now verify with code.

```python
W1 = np.array([[0.5, -0.2],
               [0.3,  0.8]])
b1 = np.array([0.1, -0.1])
W2 = np.array([[1.0, -1.5]])
b2 = np.array([0.2])
x = np.array([1.0, 2.0])

print("hidden pre-activation:", W1 @ x + b1)
print("hidden activation:    ", relu(W1 @ x + b1))
print("output pre-activation:", W2 @ relu(W1 @ x + b1) + b2)
print("output probability:   ", forward_pass(x, W1, b1, W2, b2).round(4))
```

The printed values should match your hand calculation: 0.2, 1.8, -2.3 and 0.0911. If they do not, recheck the arithmetic before suspecting the code.

If the true label is 1, the binary cross-entropy loss for this prediction is `-log(0.0911) = 2.40`, which is large, and tells us the weights are poor for this example.

```python
p = forward_pass(x, W1, b1, W2, b2)
print("loss if the label is 1:", round(binary_cross_entropy(np.array([1.0]), p), 3))
print("loss if the label is 0:", round(binary_cross_entropy(np.array([0.0]), p), 3))
```

## 6. In-Class Exercise

By hand, compute the forward pass of a tiny network with 2 inputs, 2 hidden units and 1 output for a given input and weight set, and verify the result with `forward_pass`.

Use this variation. Input `x = [-1, 3]`, with the following weights.

```
W1 = [[ 1.0,  0.5],     b1 = [0.0, 0.5]
      [-0.5,  1.0]]
W2 = [[ 2.0, -1.0]]     b2 = [-0.5]
```

Work out `z1`, `a1`, `z2` and the output by hand, and write the numbers in your notebook before running the code.

```python
W1 = np.array([[1.0, 0.5], [-0.5, 1.0]])
b1 = np.array([0.0, 0.5])
W2 = np.array([[2.0, -1.0]])
b2 = np.array([-0.5])
x = np.array([-1.0, 3.0])

z1 = W1 @ x + b1
print("z1:", z1)
print("a1:", relu(z1))
print("output:", forward_pass(x, W1, b1, W2, b2).round(4))
```

The hidden pre-activations are 0.5 and 4.0, so ReLU leaves them unchanged. The output pre-activation is `2*0.5 - 1*4.0 - 0.5 = -3.5`, and the sigmoid of that is about 0.029.

Questions:

1. What would happen if we replaced ReLU with the identity function? Show that the network is then equivalent to a single linear model.
2. Change the first input to a large positive number and observe the sigmoid output saturate near 1. What does that suggest about its gradient?
3. Construct weights for a network that computes XNOR, the opposite of XOR.

## 7. Common Mistakes

1. Getting the weight matrix shapes wrong. Write the shapes of every array on paper.
2. Leaving out the non-linear activation, so the whole network is secretly linear.
3. Computing softmax without subtracting the maximum.
4. Taking `log(0)` in a loss function.
5. Using softmax with binary cross-entropy, or sigmoid with categorical cross-entropy, without checking that the loss and output layer match.
6. Expecting a single perceptron to learn a problem that is not linearly separable.

## 8. Summary

A neuron is a weighted sum followed by an activation function. A single neuron draws one straight boundary, so it cannot learn XOR, but a hidden layer with a non-linear activation can. ReLU is the default for hidden layers, sigmoid and softmax are used at the output for probabilities, and the loss function should match the task. The forward pass is a chain of matrix multiplications and activations, which is the linear algebra from Week 3 in a new role. Next week we find out how to learn the weights.

## 9. Practice Problems

1. Write a function `init_params(sizes, seed)` that creates weights and biases for a network with the given list of layer sizes, and a `forward` function that works for any number of layers.
2. Show numerically that `softmax(z + c)` equals `softmax(z)` for any constant `c`.
3. Train a perceptron on a linearly separable set of 2D points that you generate, and plot the learned decision line.
4. Find hidden-layer weights for XNOR by modifying the XOR network.

## 10. Suggested Reading

1. Goodfellow, Bengio and Courville, Deep Learning, the chapter on deep feedforward networks.
2. Nielsen, Neural Networks and Deep Learning, the first two chapters, available online.
