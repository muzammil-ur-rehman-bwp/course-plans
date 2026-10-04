# Week 4 Lecture Plan — Advanced Knowledge Representation and Reasoning (Post Graduate)
## Topic: Advanced Description Logics — SROIQ

**Duration:** 2 hours lecture + 3 hour research seminar/lab

### Learning Objectives (Bloom's Level)
1. Enumerate SROIQ's constructs beyond ALC (role hierarchies, role chains, number restrictions,
   nominals, role characteristics). (*Understand*)
2. Trace the ≥-rule and ≤-rule tableau extensions by hand, including the merge step. (*Apply,
   Analyze*)
3. State the N2ExpTime-completeness result for SROIQ and explain what drives the complexity jump
   beyond ALC. (*Analyze, Evaluate*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap | ALC constructors and tableau rules (graduate course review) |
| 0:15–0:45 | Role hierarchies & complex role inclusions | R⊑S, role chains, the regularity condition |
| 0:45–1:15 | Number restrictions & nominals | Qualified cardinality constraints; {a}-concepts |
| 1:15–1:45 | Extending the tableau | ≥-rule, ≤-rule and merging, role-hierarchy propagation |
| 1:45–2:00 | Complexity picture | N2ExpTime-completeness vs. ALC's PSPACE and SHIQ's EXPTIME |

### Materials/Equipment
- Slides: "SROIQ: Extending the ALC Tableau"
- Whiteboard for the ≥/≤-rule trace
- Jupyter for the fragment checker

### Formative Check (in-class)
Trace, on paper, the ≤-rule's merge step for a node asserted `≤1 R.C` with two distinct
R-successors both satisfying C; state the resulting model after merging.

### Link to Lab/Assessment
Lab 4: implement a from-scratch satisfiability checker for an ALC+number-restrictions+role-
hierarchy fragment (see `lab-manuals/lab-04.md`). **Assignment 1 assigned this week.**
