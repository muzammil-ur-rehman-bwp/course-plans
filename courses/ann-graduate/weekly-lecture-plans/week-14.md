# Week 14 Lecture Plan — Artificial Neural Network (Graduate)
## Topic: Information-Theoretic Perspectives

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Explain the information bottleneck idea as applied to deep network training. (*Understand*)
2. Evaluate the idea as a debated, active research area rather than settled theory, citing its
   published critiques. (*Evaluate*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap | NTK and Lottery Ticket are two structural/dynamical theories; information bottleneck offers an information-theoretic one |
| 0:15–0:45 | The information bottleneck idea | Layers trade a "fitting" term (mutual information between a representation and the label) against a "compression" term (mutual information between that representation and the raw input) |
| 0:45–1:00 | Break | — |
| 1:00–1:30 | The evidence and the dispute | The empirical mutual-information estimates underlying some strong claims (e.g., observed compression phases) have been disputed by follow-up work; the field has not converged |
| 1:30–2:00 | Framing it honestly | How to present an unsettled research idea in a paper/talk: state the claim, state the evidence, state the published counter-evidence, and avoid overclaiming |

### Materials/Equipment
- Slides: information-bottleneck fitting/compression tradeoff diagram
- Live-coding environment (Jupyter) for a toy binned mutual-information estimate

### Formative Check (in-class)
Students write one sentence stating the information bottleneck's core claim and one sentence
stating a reason it remains disputed, demonstrating they can hold both without overclaiming either.

### Link to Lab/Assessment
Lab 14: Estimate toy, binned mutual-information-like quantities between a small network's hidden
layers and its input/output across training epochs, with explicit discussion of the estimator's
limitations (see `lab-manuals/lab-14.md`).
