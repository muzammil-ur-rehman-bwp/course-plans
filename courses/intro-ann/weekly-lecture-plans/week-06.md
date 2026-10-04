# Week 6 Lecture Plan — Introduction to Artificial Neural Networks
## Topic: Gradient Descent

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Explain the gradient as the direction of steepest ascent and gradient descent as iterative
   movement opposite it. (*Understand*)
2. Apply the gradient descent update rule and reason about the effect of the learning rate.
   (*Apply*)
3. Analyze the trade-offs between batch, stochastic, and mini-batch gradient descent. (*Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap | We have a loss; now, how do we reduce it? |
| 0:15–0:40 | The gradient & the update rule | $\theta \leftarrow \theta - \eta \nabla L(\theta)$; geometric intuition on a bowl-shaped loss |
| 0:40–1:00 | Learning rate effects | Too small (slow), too large (diverges/oscillates), live demo on a quadratic |
| 1:00–1:10 | Break | — |
| 1:10–1:40 | Batch vs. stochastic vs. mini-batch | Definitions, computational trade-offs, noise vs. speed |
| 1:40–2:00 | Why shuffle | Data order and SGD; correlated updates without shuffling |

### Materials/Equipment
- Live-coding environment, NumPy, Matplotlib
- Animated/step plot: gradient descent path on a 2D quadratic bowl for 3 learning rates

### Formative Check (in-class)
Given a 1D quadratic loss $L(\theta)=(\theta-3)^2$ and a starting point, compute by hand two
gradient descent steps for a given learning rate and predict whether a second, larger learning
rate will converge or diverge.

### Link to Lab/Assessment
Lab 6: Implement batch, stochastic, and mini-batch gradient descent on a 2D loss surface; compare
convergence paths and speed.
