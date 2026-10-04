# Week 14 Lecture Plan — Introduction to Artificial Neural Networks
## Topic: Recurrent Neural Networks (Basics)

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Explain why sequential data requires a notion of memory that feedforward networks and CNNs
   lack. (*Understand*)
2. Apply the RNN cell's update equation to compute a hidden state across a short sequence, by
   hand and in a framework. (*Apply*)
3. Understand, conceptually, the vanishing-gradient problem across time steps and how LSTMs
   address it. (*Understand*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap & motivation | Why neither an MLP nor a CNN naturally handles variable-length sequences with order-dependence |
| 0:15–0:45 | The RNN cell | Hidden state update equation; unrolling through time |
| 0:45–0:55 | Break | — |
| 0:55–1:20 | Vanishing gradients across time | Same root cause as Week 9, now across time steps instead of layers |
| 1:20–1:45 | LSTMs (conceptual) | Gating intuition; additive cell-state update as the fix, no full gate-equation derivation |
| 1:45–2:00 | A minimal RNN in a framework | Code sketch: `nn.RNN`/`nn.LSTM` on a toy sequence task |

### Materials/Equipment
- Live-coding environment, PyTorch or Keras
- Diagram: an RNN cell unrolled across 3–4 time steps

### Formative Check (in-class)
Given a toy 1D RNN cell's weights and a 3-step input sequence, compute the hidden state at each
time step by hand.

### Link to Lab/Assessment
Lab 14: Implement a single RNN cell's forward pass by hand in NumPy for a short sequence; build a
minimal RNN/LSTM for a toy sequence task with the framework.
