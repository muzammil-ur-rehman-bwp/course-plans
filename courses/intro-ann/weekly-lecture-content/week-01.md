# Week 1 — Lecture Content: Biological Inspiration, History, and Course Roadmap

## 1. The Biological Neuron (Loose Inspiration)
A biological neuron receives signals from other neurons through **dendrites**, accumulates them
in the **soma** (cell body), and if the accumulated signal is strong enough, "fires" an electrical
impulse down its **axon**, which is passed to other neurons through **synapses**. Artificial
neural networks borrow only the loosest version of this idea: a unit that combines weighted
inputs and produces an output if the combination is large enough. Nothing in this course depends
on biological accuracy — the biology is motivation, not mechanism.

## 2. A Brief History of Neural Networks
| Year | Milestone |
|---|---|
| 1943 | McCulloch & Pitts propose a binary threshold model of a neuron. |
| 1949 | Hebb proposes "cells that fire together, wire together" — an early learning rule. |
| 1958 | Rosenblatt introduces the **perceptron** and a learning algorithm for its weights. |
| 1969 | Minsky & Papert prove a single perceptron cannot represent XOR, stalling the field. |
| 1986 | Rumelhart, Hinton & Williams popularize **backpropagation**, enabling multi-layer training. |
| 1990s | Convolutional networks (LeCun) succeed on digit recognition; LSTMs (1997) address RNN memory. |
| 2012 | Deep convolutional networks (AlexNet) dramatically outperform prior methods on ImageNet, launching the modern deep learning era. |

The 1969–1986 gap is often called the first "AI winter" for neural networks: without a working
algorithm to train multi-layer networks, and with a proof that single-layer networks were
fundamentally limited, research funding and interest declined sharply until backpropagation gave
a practical way to train deeper, more expressive networks.

## 3. The McCulloch-Pitts Neuron
The McCulloch-Pitts (MP) neuron is a binary threshold unit. Given binary inputs $x_1, \dots, x_n$
and fixed weights $w_1, \dots, w_n$, it computes a weighted sum and compares it to a threshold
$\theta$:

$$
z = \sum_{i=1}^n w_i x_i, \qquad \text{output} = \begin{cases} 1 & \text{if } z \ge \theta \\ 0 & \text{otherwise} \end{cases}
$$

Unlike the perceptron (Week 2), the MP neuron's weights and threshold are *fixed by the designer*,
not learned from data. Even so, with the right fixed weights, a single MP neuron can realize basic
logic gates:

```python
import numpy as np

def mp_neuron(x, w, theta):
    z = np.dot(w, x)
    return 1 if z >= theta else 0

# AND gate: fires only when both inputs are 1
w_and, theta_and = np.array([1, 1]), 2
# OR gate: fires when at least one input is 1
w_or, theta_or = np.array([1, 1]), 1
# NOT gate: fires when the single input is 0
w_not, theta_not = np.array([-1]), 0

for x1 in (0, 1):
    for x2 in (0, 1):
        x = np.array([x1, x2])
        print(x1, x2, "AND:", mp_neuron(x, w_and, theta_and),
                      "OR:", mp_neuron(x, w_or, theta_or))

for x1 in (0, 1):
    print(x1, "NOT:", mp_neuron(np.array([x1]), w_not, theta_not))
```

This confirms AND, OR, and NOT are all realizable by a single threshold unit with suitably chosen
fixed weights and threshold — the building blocks of propositional logic, computed by a "neuron."

## 4. Course Roadmap
This course is a full semester dedicated entirely to neural networks, in contrast to the two-week
survey (perceptron, forward pass, a framework training loop) given in *Programming for Artificial
Intelligence*. The roadmap:

| Phase | Weeks | What's new beyond the prior course's brief treatment |
|---|---|---|
| Foundations | 1–4 | The *history and mechanics* behind the perceptron and MLP, not just their use |
| Learning Theory | 5–8 | Loss functions in depth, gradient descent, and a full hand-derivation and from-scratch implementation of backpropagation |
| Training in Practice | 9–11 | Initialization, vanishing/exploding gradients, optimizers (momentum/RMSProp/Adam), regularization — none of this appears in the prior course |
| Frameworks & Architectures | 12–14 | Framework-based training plus a first, correct conceptual look at CNNs and RNNs |
| Evaluation & Synthesis | 15–16 | Systematic debugging/tuning, and a capstone project with a required ablation experiment |

## 5. In-Class Exercise
Given a shuffled list of the history timeline's milestones (without years), put them in correct
chronological order and briefly justify each placement. Then, by hand, compute the output of an MP
neuron with $w = [1, 1, 1]$, $\theta = 2$ for inputs $(1, 0, 1)$ and $(0, 0, 1)$, and verify with
the code above.
