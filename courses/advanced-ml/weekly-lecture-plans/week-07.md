# Week 7 Lecture Plan — Advanced Machine Learning (Post Graduate)
## Topic: Nonparametric Bayesian Methods II — The Chinese Restaurant Process

**Duration:** 2 hours lecture + 3 hour research seminar/lab

### Learning Objectives (Bloom's Level)
1. State the Chinese Restaurant Process seating rule and its equivalence to the DP. (*Understand*)
2. Explain exchangeability and why the CRP's partition distribution does not depend on arrival
   order. (*Understand, Analyze*)
3. Implement a CRP sampler and an infinite-mixture generative model, and verify the $O(\alpha\log
   n)$ cluster-growth prediction. (*Apply, Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap | Stick-breaking view (Week 6); motivating the combinatorial alternative |
| 0:15–0:45 | The CRP seating rule | $n_k/(n+\alpha)$ vs. $\alpha/(n+\alpha)$; worked small-$n$ example |
| 0:45–1:15 | Exchangeability | Why partition probability does not depend on customer order; why this matters for inference |
| 1:15–1:45 | Cluster growth and DP mixtures | $O(\alpha\log n)$ growth; building a full DP mixture model on top of the CRP |
| 1:45–2:00 | Synthesis | CRP vs. stick-breaking: combinatorial vs. constructive views of the same object |

### Materials/Equipment
- Slides: "The Chinese Restaurant Process and Infinite Mixture Models"
- Whiteboard for the exchangeability argument
- Live-coding environment (Jupyter)

### Formative Check (in-class)
For $n=10$ customers already seated at 3 tables with occupancies $(5,3,2)$ and $\alpha=2$, compute
the probability the 11th customer starts a new table.

### Link to Lab/Assessment
Lab 7: implement a CRP sampler, verify seating probabilities and cluster-count growth, and build a
CRP-based infinite Gaussian mixture generator (see `lab-manuals/lab-07.md`).
