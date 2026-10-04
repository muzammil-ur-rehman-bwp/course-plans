# Week 3 Lecture Plan — Advanced Artificial Neural Network (Post Graduate)
## Topic: Mean-Field Theory of Neural Networks

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Explain the mean-field limit of wide neural networks as a distributional (not kernel) view of
   width → ∞. (*Understand*)
2. Contrast the mean-field limit's scaling with the NTK limit's scaling, and state why only the
   former admits feature learning. (*Analyze*)
3. Analyze signal propagation through depth in the mean-field regime, connecting it to the
   graduate course's initialization-theory formulas as a special case. (*Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap | Week 2's NTK limit tracked a kernel; today's limit tracks a distribution |
| 0:15–0:45 | The mean-field limit | A hidden layer's weights as a population with an empirical distribution; width → ∞ as a law-of-large-numbers limit over that population |
| 0:45–1:10 | Contrast with NTK | Two different scaling conventions for the same wide network; why mean-field scaling allows O(1) relative parameter movement (not lazy) |
| 1:10–1:40 | Signal propagation | Variance/correlation recursion through depth; order-to-chaos transition; recovering Xavier/He as the small-signal, linear special case |
| 1:40–2:00 | Synthesis | Table: NTK limit vs. mean-field limit, side by side |

### Materials/Equipment
- Slides: "Mean-Field Theory: A Distributional View of Wide Networks"
- Whiteboard for the variance/correlation recursion
- Live-coding environment (Jupyter, PyTorch/NumPy)

### Formative Check (in-class)
State, in one sentence each, what quantity the NTK limit holds fixed and what quantity the
mean-field limit instead allows to evolve, and why this difference is exactly the difference
between "no feature learning" and "feature learning is possible."

### Link to Lab/Assessment
Lab 3: simulate variance/correlation propagation through a wide random network's depth and compare
to the Xavier/He formulas in the regime where they should agree (see `lab-manuals/lab-03.md`).
