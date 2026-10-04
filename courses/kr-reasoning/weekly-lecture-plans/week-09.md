# Week 9 Lecture Plan — Knowledge Representation and Reasoning
## Topic: Midterm Exam + Non-Monotonic Reasoning

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Recall Weeks 1–8 content for the midterm exam. (*Remember–Apply*)
2. Explain monotonicity and why classical logic cannot retract conclusions as new information
   arrives. (*Understand*)
3. Apply the closed-world assumption and default logic to derive — and correctly retract —
   conclusions from a small knowledge base. (*Apply, Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–1:30 | Midterm Exam | Covers Weeks 1–8 |
| 1:30–1:40 | Break | — |
| 1:40–2:00 | Monotonicity & CWA | Why `KB ⊨ α` can never be undone in classical logic; the closed-world assumption as an implicit fix used by most databases/rule engines |

*(Note: non-monotonic reasoning's full treatment — default logic and circumscription — continues
into the Week 9 lab and is completed in the lecture content; the in-class time budget above
reflects the exam's length.)*

### Materials/Equipment
- Midterm exam paper/online quiz (Weeks 1–8)
- Slides: monotonicity definition, CWA examples, default-logic rule schema

### Formative Check (in-class)
Post-exam exercise: given a small KB and the CWA, determine which currently-unlisted facts are
treated as false; then add one new fact and determine which, if any, default conclusion must be
retracted.

### Link to Lab/Assessment
Lab 9: Implement a CWA query engine and a default-logic extension builder that correctly retracts
a previously drawn conclusion when a new, conflicting fact is added.
