# Week 9 Lecture Plan — Advanced Machine Learning (Post Graduate)
## Topic: Midterm Exam + Causal Inference in Depth II — Instrumental Variables and Do-Calculus

**Duration:** 2 hours lecture + 3 hour research seminar/lab (first portion of lecture is the exam)

### Learning Objectives (Bloom's Level)
1. Complete the midterm exam covering Weeks 1–8. (*Remember through Analyze*)
2. State the three IV assumptions and derive the linear-IV identification formula. (*Apply, Analyze*)
3. Apply the three do-calculus rules to determine identifiability of a causal query. (*Apply, Analyze, Evaluate*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–1:00 | Midterm Exam | Closed-book, qualifying-exam style, Weeks 1–8 |
| 1:00–1:20 | Instrumental variables | The three IV assumptions; why unconfoundedness can fail |
| 1:20–1:45 | IV identification | Deriving $\beta=\mathrm{Cov}(Z,Y)/\mathrm{Cov}(Z,T)$ |
| 1:45–2:15 | Do-calculus | The three rules stated precisely; the graph-surgery/$d$-separation justification |
| 2:15–2:45 | Worked examples | One identifiable graph, one non-identifiable graph |
| 2:45–3:00 | Synthesis | Recap: IV and do-calculus as two complementary routes around confounding |

### Materials/Equipment
- Midterm exam booklet/online exam system
- Slides: "Instrumental Variables and the Do-Calculus"
- Whiteboard for graph-surgery derivations

### Formative Check (in-class)
Given a DAG with an unobserved confounder between $T$ and $Y$ and a valid instrument $Z$, identify
which do-calculus rule(s), if any, would be needed to identify $P(y\mid\mathrm{do}(t))$ without
using $Z$ at all, and explain why IV is needed in this specific graph.

### Link to Lab/Assessment
Lab 9: implement a from-scratch two-stage-least-squares IV estimator and apply do-calculus rules
by hand to two graphs (see `lab-manuals/lab-09.md`).
