# Week 8 Lecture Plan — Advanced Machine Learning (Post Graduate)
## Topic: Causal Inference in Depth I — Potential Outcomes and Propensity Scores; Midterm Review

**Duration:** 2 hours lecture + 3 hour research seminar/lab

### Learning Objectives (Bloom's Level)
1. State the potential-outcomes framework and the fundamental problem of causal inference.
   (*Understand*)
2. State unconfoundedness and overlap, and derive the IPW-ATE identity. (*Apply, Analyze*)
3. Implement propensity-score estimation and IPW-based ATE estimation on simulated confounded
   data. (*Apply*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap | Graduate course's conceptual confounding/do-notation intro — now made rigorous |
| 0:15–0:45 | Potential outcomes | $Y_i(1),Y_i(0)$; the fundamental problem; ATE vs. individual effect |
| 0:45–1:15 | Unconfoundedness and overlap | Precise statements; why both are needed for identification |
| 1:15–1:45 | Propensity scores and IPW | Definition; deriving the IPW-ATE identity via the tower property |
| 1:45–2:00 | Midterm review | Weeks 1–8 recap map |

### Materials/Equipment
- Slides: "Potential Outcomes and Propensity-Score Methods"
- Whiteboard for the IPW-ATE derivation
- Live-coding environment (Jupyter)

### Formative Check (in-class)
Given a simulated dataset where treatment probability depends on a covariate that also affects
the outcome, explain why the naive difference-in-means estimator is biased and which assumption
(unconfoundedness or overlap) IPW relies on to correct it.

### Link to Lab/Assessment
Lab 8: implement propensity-score estimation and IPW-based ATE estimation on simulated
observational data (see `lab-manuals/lab-08.md`).

### Capstone Milestone
Problem-statement check-in (informal, with instructor).
