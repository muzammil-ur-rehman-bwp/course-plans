# Week 1 Lecture Plan — Machine Learning (Graduate)
## Topic: The Statistical Learning Framework; Course Roadmap and Scope

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Recall the applied-ML foundation assumed from the prerequisite course. (*Remember*)
2. State the definitions of true risk, empirical risk, and the ERM principle precisely. (*Understand*)
3. Explain why unrestricted ERM can overfit, using the "memorizer" argument. (*Understand, Analyze*)
4. Map any given topic to the graduate course (this one, ANN, AI, DL, KR&R) that owns it. (*Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:20 | Course overview | Syllabus, assessment plan, scope boundaries vs. sibling graduate courses |
| 0:20–0:50 | Risk and empirical risk | Formal definitions, worked toy example |
| 0:50–1:10 | The ERM principle | Connecting ERM to `.fit()` calls from the prerequisite course |
| 1:10–1:20 | Break | — |
| 1:20–1:50 | Why unrestricted ERM fails | The memorizer argument; overfitting as a theory statement |
| 1:50–2:00 | Roadmap | 16-week map; what's coming in Weeks 2–16 |

### Materials/Equipment
- Live-coding environment (Jupyter/Colab)
- Course roadmap slide/diagram

### Formative Check (in-class)
Cold-call: "For $H=$ all functions, what is the minimum achievable $\widehat L_S(h)$, and what
does that number tell you about $L_D(h)$?" — target answer: 0, and nothing.

### Link to Lab/Assessment
Lab 1: implement and verify the empirical-risk-vs-true-risk gap for the memorizer hypothesis
class in NumPy.
