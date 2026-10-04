# Week 3 Lecture Plan — Knowledge Representation and Reasoning (Graduate)
## Topic: Temporal Logic

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. State the semantics of LTL's X, G, F, and U operators over an execution trace. (*Understand*)
2. Evaluate an LTL formula against a finite trace by hand and in code. (*Apply*)
3. Explain why CTL's path-quantified operators (AG, EF) are not interchangeable with LTL's G, F
   once branching futures are allowed. (*Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap | Modal logic's single accessibility relation vs. the need to reason about an ordered sequence of states |
| 0:15–0:50 | LTL syntax/semantics | X, G, F, U defined precisely; worked evaluation on a 5-step trace |
| 0:50–1:20 | CTL (conceptual) | Branching computation trees; A/E path quantifiers; AGφ vs. Gφ, EFφ vs. Fφ contrasted on a branching example |
| 1:20–1:30 | Break | — |
| 1:30–1:55 | Applications | Expressing planning goals/safety constraints and verification properties as LTL/CTL formulas |
| 1:55–2:00 | Synthesis | Where temporal logic connects to the undergraduate course's STRIPS/Allen's-algebra material |

### Materials/Equipment
- Slides: "LTL and CTL: Reasoning About Time"
- Whiteboard for trace-based LTL evaluation and the branching-tree diagram
- Live-coding environment (Jupyter)

### Formative Check (in-class)
Given a 5-step trace of propositional valuations, evaluate Gφ, Fφ, and φUψ by hand at step 0,
then verify with the provided Python evaluator.

### Link to Lab/Assessment
Lab 3: implement a finite-trace LTL evaluator and test it on sample execution traces (see
`lab-manuals/lab-03.md`).
