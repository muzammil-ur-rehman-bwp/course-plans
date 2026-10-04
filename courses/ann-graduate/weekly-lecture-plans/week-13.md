# Week 13 Lecture Plan — Artificial Neural Network (Graduate)
## Topic: The Lottery Ticket Hypothesis and Pruning

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. State the Lottery Ticket Hypothesis and explain the iterative magnitude-pruning procedure used
   to find winning tickets. (*Understand, Apply*)
2. Evaluate the evidence for the hypothesis's initialization-dependence claim. (*Evaluate*)
3. Explain model compression and knowledge distillation at a basic level. (*Understand*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap | NTK is one theory of trainability; the Lottery Ticket Hypothesis is an empirical structural claim about *sparsity* |
| 0:15–0:45 | The hypothesis | Frankle & Carbin's claim: a dense network at initialization contains a sparse "winning ticket" subnetwork that, trained alone from that same initialization, matches the full network |
| 0:45–1:05 | Iterative magnitude pruning | Train → prune smallest-magnitude weights → reset surviving weights to original init → retrain, repeated |
| 1:05–1:15 | Break | — |
| 1:15–1:40 | The initialization-dependence evidence | Winning tickets trained from original init vs. the same sparse mask with a random reinitialization — the control that supports the hypothesis |
| 1:40–2:00 | Compression & distillation | Pruning, quantization (brief mention), and knowledge distillation (student network trained against a teacher's soft outputs) as related, practically-motivated techniques |

### Materials/Equipment
- Slides: iterative magnitude-pruning procedure diagram
- Live-coding environment (Jupyter) for the pruning + distillation experiment

### Formative Check (in-class)
Students explain, in one or two sentences, why the random-reinitialization control is essential
to the Lottery Ticket Hypothesis's claim — i.e., why finding *any* sparse subnetwork that trains
well would not, by itself, support the hypothesis.

### Link to Lab/Assessment
Lab 13: Implement iterative magnitude pruning on a small trained network to find a sparse mask,
compare a winning-ticket retraining run against a random-reinitialization control at the same
sparsity, and run a minimal knowledge-distillation experiment (see `lab-manuals/lab-13.md`).
