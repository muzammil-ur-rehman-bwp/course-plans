# Week 10 Lecture Plan — Artificial Neural Network (Graduate)
## Topic: Generalization Theory II

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Define Rademacher complexity conceptually and explain why it is often a tighter,
   data-dependent alternative to VC-dimension bounds. (*Understand, Analyze*)
2. Explain margin-based generalization arguments. (*Analyze*)
3. Interpret the random-label-fitting experiment and its implications for classical capacity
   arguments. (*Analyze, Evaluate*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap | VC dimension is worst-case and data-independent; today's tools are tighter |
| 0:15–0:45 | Rademacher complexity | Conceptual definition: capacity measured by a class's ability to fit random ±1 labels, directly on the data distribution |
| 0:45–1:05 | Margin bounds | Why a large-margin separator generalizes better than the raw VC bound suggests |
| 1:05–1:15 | Break | — |
| 1:15–1:45 | The random-label experiment | Zhang et al.-style observation: networks that can fit random labels to zero training error still generalize normally when trained on real labels — what this does and does not imply |
| 1:45–2:00 | Synthesis | Why capacity alone is not the right lens; previewing Weeks 11–14's alternative accounts |

### Materials/Equipment
- Slides: Rademacher-complexity intuition diagram; margin diagram
- Live-coding environment (Jupyter) for the random-label-fitting demonstration

### Formative Check (in-class)
Students explain, in their own words, why a network fitting random labels to zero training error
does *not* by itself contradict that same network generalizing well on real labels — identifying
the distinction between raw capacity and what gradient descent actually finds.

### Link to Lab/Assessment
Lab 10: Empirically estimate a Rademacher-complexity-flavored quantity for a small hypothesis
class, and fit random labels with a small network, reporting training/test error alongside the
same network's normal-label performance (see `lab-manuals/lab-10.md`).
