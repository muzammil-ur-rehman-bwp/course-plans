# Week 5 Lecture Plan — Advanced Deep Learning (Post Graduate)
## Topic: In-Context Learning Mechanics

**Duration:** 2 hours lecture + 3 hour research seminar/lab

### Learning Objectives (Bloom's Level)
1. Precisely restate the in-context learning (ICL) phenomenon and why "no weight update" is the
   operative claim. (*Understand*)
2. Trace an induction-head circuit on a toy sequence and explain the ablation evidence connecting
   it to ICL. (*Analyze*)
3. Critically evaluate the implicit-gradient-descent analogy, distinguishing what the linear-
   attention toy result establishes from what it is sometimes taken to establish. (*Evaluate*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:20 | The phenomenon | Few-shot prompting with no gradient update; why this is surprising relative to fine-tuning |
| 0:20–0:50 | Induction heads | The two-head "complete the pattern" circuit, traced on a toy sequence; ablation evidence |
| 0:50–1:30 | Implicit gradient descent | The linear-attention/linear-regression toy equivalence, derived at the level of its assumptions |
| 1:30–1:55 | Evidence-grading discussion | Structured critique: what's established vs. speculative; why extrapolation outruns evidence |
| 1:55–2:00 | Synthesis | Recap table of claims and their evidentiary status |

### Materials/Equipment
- Slides: "In-Context Learning Mechanics"
- Live attention-pattern visualization notebook
- Handout: structured evidence-grading worksheet

### Formative Check (in-class)
Given a short repeated token sequence, identify by hand which positions an induction head would
attend to, and state one ablation experiment that would test whether removing that head harms
few-shot ICL accuracy specifically (not just general language-modeling loss).

### Link to Lab/Assessment
Lab 5 (critical-writing + light code): trace an induction-head circuit and write a structured
evidence-graded critique of the implicit-gradient-descent analogy (see `lab-manuals/lab-05.md`).
Quiz 2 this week (Weeks 4–5 content).
