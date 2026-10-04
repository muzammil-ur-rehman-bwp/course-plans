# Week 5 Lecture Plan — Advanced Knowledge Representation and Reasoning (Post Graduate)
## Topic: Structured Argumentation — ASPIC+

**Duration:** 2 hours lecture + 3 hour research seminar/lab

### Learning Objectives (Bloom's Level)
1. Build ASPIC+ arguments as trees from strict/defeasible rules and premises. (*Apply*)
2. Classify an attack between two arguments as rebutting, undercutting, or undermining.
   (*Apply, Analyze*)
3. Implement an argument-construction-and-attack calculator feeding a Dung-style extension
   computation. (*Apply, Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap + motivation | Dung AF's opaque nodes vs. ASPIC+'s internal structure |
| 0:15–0:45 | Rules, premises, arguments | Strict vs. defeasible rules; arguments as trees; top rule |
| 0:45–1:25 | Attack types | Rebutting (on a conclusion) vs. undercutting (on a rule's applicability); undermining |
| 1:25–1:50 | Preferences and defeat | Preference orderings over defeasible elements; attack → defeat |
| 1:50–2:00 | Synthesis | ASPIC+ as a layer producing a Dung framework, not replacing Dung semantics |

### Materials/Equipment
- Slides: "ASPIC+: Structured Argumentation"
- Whiteboard for argument-tree construction
- Jupyter for the calculator

### Formative Check (in-class)
Given a toy rule set, construct two arguments and determine whether the attack between them is
rebutting or undercutting, naming the specific sub-argument and rule targeted.

### Link to Lab/Assessment
Lab 5: build the ASPIC+ argument-construction-and-attack calculator (see `lab-manuals/lab-05.md`).
