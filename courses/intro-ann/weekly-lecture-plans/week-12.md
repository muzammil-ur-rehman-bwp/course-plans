# Week 12 Lecture Plan — Introduction to Artificial Neural Networks
## Topic: Introduction to a Deep Learning Framework

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Explain automatic differentiation (autograd) as a generalization of the backpropagation
   implemented by hand in Weeks 7–8. (*Understand*)
2. Apply a deep learning framework (PyTorch or Keras) to define, train, and evaluate an MLP on a
   real dataset. (*Apply*)
3. Analyze training/validation curves produced by the framework to confirm correct training.
   (*Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap | Everything up to now was "what the framework does under the hood" |
| 0:15–0:40 | Autograd | Tensors, computational graphs, `.backward()`; relating it directly to Week 7's $\delta$ |
| 0:40–1:05 | Defining a model | `nn.Module`/`Sequential`, layers, forward method |
| 1:05–1:15 | Break | — |
| 1:15–1:45 | The training loop | Loss, optimizer, `zero_grad`/`backward`/`step`; mapping to Weeks 6–10's manual loop |
| 1:45–2:00 | Training on MNIST | Live demo: load data, train a few epochs, plot curves |

### Materials/Equipment
- Live-coding environment, PyTorch (or TensorFlow/Keras), Matplotlib
- MNIST or Fashion-MNIST dataset (via framework's built-in loader)

### Formative Check (in-class)
Given a framework training loop with one line removed (e.g., `optimizer.zero_grad()`), predict
and explain the resulting training failure mode before running the code.

### Link to Lab/Assessment
Lab 12: Build and train an MLP on MNIST/Fashion-MNIST with the framework; plot training/
validation curves; report test accuracy.
