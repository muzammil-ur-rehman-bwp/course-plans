# Week 3 Lecture Plan — Advanced Knowledge Representation and Reasoning (Post Graduate)
## Topic: Many-Valued and Paraconsistent Logics

**Duration:** 2 hours lecture + 3 hour research seminar/lab

### Learning Objectives (Bloom's Level)
1. State Kleene's and Łukasiewicz's three-valued truth tables and identify where they differ.
   (*Understand, Apply*)
2. Explain why paraconsistent logics avoid explosion under contradiction. (*Understand, Analyze*)
3. Implement truth-table evaluators for K3, Ł3, and a Belnap/Dunn four-valued logic. (*Apply*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Motivation | Why two-valued logic is wrong for incomplete/contradictory sources |
| 0:15–0:45 | Kleene K3 | Truth tables for ∧,∨,¬,→; the "determinacy" reading of U |
| 0:45–1:10 | Łukasiewicz Ł3 | The differing implication table; the "degree of truth preservation" reading |
| 1:10–1:40 | Paraconsistency | Explosion, paraconsistent negation, Belnap/Dunn four-valued logic |
| 1:40–2:00 | Applications | Reasoning over disagreeing information sources without collapse |

### Materials/Equipment
- Slides: "Many-Valued & Paraconsistent Logics"
- Whiteboard truth-table derivations
- Jupyter for the evaluators

### Formative Check (in-class)
For the formula `U → U`, compute its value under K3 and under Ł3, state the two differing
results, and explain in one sentence which philosophical commitment each encodes.

### Link to Lab/Assessment
Lab 3: build K3, Ł3, and Belnap/Dunn evaluators; demonstrate non-explosion on a contradictory
toy KB (see `lab-manuals/lab-03.md`).
