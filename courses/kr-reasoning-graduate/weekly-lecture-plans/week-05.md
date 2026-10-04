# Week 5 Lecture Plan — Knowledge Representation and Reasoning (Graduate)
## Topic: Automated Theorem Proving in Depth

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Construct a short sequent-calculus derivation for a simple valid formula. (*Apply*)
2. Build a first-order tableau by hand for a small unsatisfiable formula set, applying the
   eigenvariable/fresh-constant conditions correctly. (*Apply*)
3. Implement and compare set-of-support resolution against unrestricted resolution on the same
   clause set. (*Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Recap | Week 4's DL tableau as a special case of a more general proof method |
| 0:15–0:40 | Sequent calculus | Introduction/elimination rules, a worked derivation |
| 0:40–1:10 | FOL tableau | Branching rules extended with ∀/∃ instantiation; worked unsatisfiability example |
| 1:10–1:20 | Break | — |
| 1:20–1:50 | Resolution refinements | Set-of-support and ordering strategies; why each preserves completeness while pruning search |
| 1:50–2:00 | Synthesis | How these three proof methods relate (all sound/complete FOL decision procedures for different fragments/strategies) |

### Materials/Equipment
- Slides: "Sequent Calculus, FOL Tableau, and Resolution Refinements"
- Whiteboard for derivations
- Live-coding environment (Jupyter)

### Formative Check (in-class)
Given a 4-clause set and a designated set-of-support, identify which resolution steps the SOS
strategy permits and which it forbids, before running the solver.

### Link to Lab/Assessment
Lab 5: implement set-of-support resolution and compare search-space size against unrestricted
resolution (see `lab-manuals/lab-05.md`).
