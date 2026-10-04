# Week 6 Lecture Plan — Introduction to Deep Learning
## Topic: Sequence Models I — RNN Limitations, the LSTM Cell, the GRU

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Explain why vanilla RNNs struggle with long sequences, in terms of repeated multiplication
   during backpropagation through time. (*Understand*)
2. Analyze the LSTM cell's gate equations (forget, input, output, cell state). (*Analyze*)
3. Analyze the GRU as a simplified gated alternative and compare it to the LSTM. (*Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | RNN recap | The RNN cell equation from the prerequisite course, briefly restated |
| 0:15–0:40 | Why vanilla RNNs struggle | Backpropagation through time; repeated multiplication by the recurrent weight matrix; vanishing/exploding gradients across time steps |
| 0:40–0:50 | Break | — |
| 0:50–1:30 | The LSTM cell in depth | Forget/input/output gates, candidate cell state, cell state update — full equations, worked through step by step |
| 1:30–1:55 | The GRU | Update gate, reset gate, candidate hidden state; comparison table vs. LSTM |
| 1:55–2:00 | Framework mapping | `nn.LSTM`/`nn.GRU` input/output shape conventions |

### Materials/Equipment
- Live-coding environment, PyTorch
- Handout: LSTM and GRU gate-equation diagrams

### Formative Check (in-class)
Given a numeric forget-gate output of 0.1 and an input-gate output of 0.9 at a given time step,
explain qualitatively what happens to the cell state at that step.

### Link to Lab/Assessment
Lab 6: Implement the LSTM gate equations by hand (NumPy or plain PyTorch tensor ops) for one time
step; then use `nn.LSTM` and `nn.GRU` on a toy sequence and compare.
