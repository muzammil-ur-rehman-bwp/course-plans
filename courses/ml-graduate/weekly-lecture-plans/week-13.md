# Week 13 Lecture Plan — Machine Learning (Graduate)
## Topic: Causal Inference Basics

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Distinguish correlation from causation and define confounding. (*Understand*)
2. Reproduce and explain a Simpson's paradox numerical example. (*Analyze*)
3. Classify a variable in a given scenario as a confounder, mediator, or collider using a causal DAG. (*Analyze*)
4. Explain do-notation and interventions conceptually, and why they matter for trustworthy ML. (*Understand*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Correlation vs. causation | Framing; why prediction and intervention can disagree |
| 0:15–0:40 | Confounding & Simpson's paradox | Full numerical worked example (treatment/severity table) |
| 0:40–1:05 | Causal DAGs | Confounder, mediator, collider patterns; when to adjust |
| 1:05–1:15 | Break | — |
| 1:15–1:40 | Do-notation & interventions | $P(Y\mid X=x)$ vs. $P(Y\mid\mathrm{do}(X=x))$, conceptually |
| 1:40–2:00 | Relevance to trustworthy ML | Spurious correlation, fairness, intervention-based decisions |

### Materials/Equipment
- Whiteboard for the Simpson's-paradox table and DAG diagrams
- Jupyter/pandas notebook reproducing the stratified vs. aggregate rates

### Formative Check (in-class)
Students classify the confounder in the Section-3 Simpson's-paradox example and explain why the
stratified comparison is the correct one.

### Link to Lab/Assessment
Lab 13: reproduce Simpson's paradox numerically with pandas; construct a second confounding
example from a provided dataset. **Quiz 5** (Weeks 11–12) administered.
