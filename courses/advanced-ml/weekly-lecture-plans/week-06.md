# Week 6 Lecture Plan — Advanced Machine Learning (Post Graduate)
## Topic: Nonparametric Bayesian Methods I — The Dirichlet Process

**Duration:** 2 hours lecture + 3 hour research seminar/lab

### Learning Objectives (Bloom's Level)
1. State the Dirichlet process's defining property. (*Understand*)
2. Derive and explain the stick-breaking construction. (*Understand, Apply*)
3. Implement a truncated stick-breaking sampler and explore the effect of the concentration
   parameter. (*Apply, Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Motivation | Why fixed-$K$ mixture models are limiting; a prior over infinite mixtures |
| 0:15–0:45 | The Dirichlet process | Defining property via finite partitions; base measure $H$, concentration $\alpha$ |
| 0:45–1:25 | Stick-breaking construction | Beta draws, weight construction, atom locations; derivation that weights sum to 1 |
| 1:25–1:50 | The role of $\alpha$ | Small vs. large $\alpha$; effect on weight decay |
| 1:50–2:00 | Synthesis | Recap: DP as an infinite-dimensional Dirichlet; preview CRP (Week 7) |

### Materials/Equipment
- Slides: "The Dirichlet Process and Stick-Breaking"
- Whiteboard for the stick-breaking weight-sum derivation
- Live-coding environment (Jupyter)

### Formative Check (in-class)
For $\alpha=0.5$ versus $\alpha=5$, sketch (by hand) the expected shape of the first 10
stick-breaking weights and explain which $\alpha$ concentrates more mass on the first atom.

### Link to Lab/Assessment
Lab 6: implement a truncated stick-breaking Dirichlet process sampler (see
`lab-manuals/lab-06.md`).
