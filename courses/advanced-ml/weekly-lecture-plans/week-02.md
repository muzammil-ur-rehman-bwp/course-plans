# Week 2 Lecture Plan — Advanced Machine Learning (Post Graduate)
## Topic: Minimax Lower Bounds

**Duration:** 2 hours lecture + 3 hour research seminar/lab

### Learning Objectives (Bloom's Level)
1. State the minimax risk framework precisely. (*Understand*)
2. State Fano's inequality and explain its information-theoretic intuition. (*Understand, Apply*)
3. Derive a minimax lower bound for estimating a Gaussian mean via the packing-set + Fano recipe,
   and match it against the sample mean's known upper bound. (*Apply, Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Motivation | Why an upper bound alone never certifies optimality; the minimax risk definition |
| 0:15–0:40 | Fano's inequality | Statement; the Markov-chain picture; information-theoretic intuition |
| 0:40–1:15 | The three-step recipe | Reduction to testing; packing-set construction; KL-divergence information bound |
| 1:15–1:45 | Worked example | Gaussian location family: packing set, KL bound, resulting $\Omega(1/\sqrt n)$ rate |
| 1:45–2:00 | Synthesis | Matching the lower bound against the sample mean's upper bound; what "rate-optimal" means |

### Materials/Equipment
- Slides: "Minimax Lower Bounds and Fano's Inequality"
- Whiteboard for the packing-set and KL-divergence derivation
- Live-coding environment (Jupyter)

### Formative Check (in-class)
For a packing set of $M=4$ well-separated Gaussian means, write down Fano's inequality's
right-hand side and explain, in one sentence, why increasing $M$ (more, closer hypotheses)
tightens the resulting lower bound.

### Link to Lab/Assessment
Lab 2: implement an empirical Fano-bound sanity check and verify the Gaussian-mean minimax rate
(see `lab-manuals/lab-02.md`).
