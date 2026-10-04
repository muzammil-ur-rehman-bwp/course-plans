# Week 2 Lecture Plan — Advanced Artificial Intelligence (Post Graduate)
## Topic: Online Learning and Regret Minimization

**Duration:** 2 hours lecture + 3 hour research seminar/lab

### Learning Objectives (Bloom's Level)
1. State the online-learning protocol and the formal definition of regret. (*Understand*)
2. Derive the multiplicative-weights algorithm's regret bound via the potential-function
   argument. (*Apply, Analyze*)
3. Implement multiplicative weights and empirically verify sublinear regret growth. (*Apply*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap + motivation | Why "no statistical assumption on losses" matters; regret as the right measure |
| 0:15–0:50 | Multiplicative weights | Board-worked algorithm + potential-function derivation of the regret bound |
| 0:50–1:20 | Optimizing η | Deriving the O(√(T ln N)) bound by tuning the learning rate |
| 1:20–1:50 | Connections | Weighted majority (mistake-bound setting) as the classical predecessor; preview: regret in games (Weeks 5–6) |
| 1:50–2:00 | Synthesis | Recap table: protocol, update rule, regret bound |

### Materials/Equipment
- Slides: "Online Learning & Regret Minimization"
- Whiteboard for the potential-function derivation
- Live-coding environment (Jupyter)

### Formative Check (in-class)
Given N=4 experts and T=1000 rounds, compute the optimized η and the resulting regret bound;
explain why doubling N only adds a logarithmic, not linear, cost to the bound.

### Link to Lab/Assessment
Lab 2: implement multiplicative weights and measure empirical regret (see `lab-manuals/lab-02.md`).
