# Week 6 Lecture Plan — Knowledge Representation and Reasoning (Graduate)
## Topic: Non-Monotonic Reasoning via Answer Set Programming

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Compute the Gelfond–Lifschitz reduct of a normal logic program with respect to a candidate
   atom set, and check whether that set is a stable model. (*Apply*)
2. Encode a small combinatorial problem (graph coloring) as an ASP program. (*Apply*)
3. Contrast ASP's negation-as-failure and stable-model semantics with the undergraduate course's
   default logic and circumscription. (*Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap | Undergraduate default logic/CWA as motivation for a computational non-monotonic formalism |
| 0:15–0:50 | Stable-model semantics | The GL-reduct, worked by hand on a 3-rule program |
| 0:50–1:10 | ASP syntax | Facts, rules, negation-as-failure vs. classical negation, integrity constraints |
| 1:10–1:20 | Break | — |
| 1:20–1:50 | Worked problem | Graph coloring as an ASP program; clingo-style syntax walkthrough |
| 1:50–2:00 | Contrast | ASP vs. default logic/circumscription: what's the same, what's different |

### Materials/Equipment
- Slides: "Stable Models and Answer Set Programming"
- Whiteboard for GL-reduct derivations
- Live-coding environment (Jupyter)

### Formative Check (in-class)
Given a 3-rule normal logic program and two candidate atom sets, compute each one's GL-reduct and
determine which (if either) is a stable model.

### Link to Lab/Assessment
Lab 6: implement a brute-force stable-model checker and use it to solve a small graph-coloring
instance (see `lab-manuals/lab-06.md`).
- **Quiz 1 this week** (Week 2 content).
