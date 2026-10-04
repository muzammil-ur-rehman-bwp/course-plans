# Week 6 Lecture Plan — Introduction to AI
## Topic: Knowledge Representation & Propositional Logic

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. State the syntax of propositional logic (atomic sentences, connectives, well-formed sentences). (*Remember*)
2. Explain the semantics of propositional logic using truth tables and models. (*Understand*)
3. Apply truth tables to determine satisfiability, validity, and logical equivalence. (*Apply*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Why knowledge representation? | Limits of pure search/reflex agents; the role of a knowledge base |
| 0:15–0:45 | Propositional logic syntax | Atomic sentences, ¬, ∧, ∨, →, ↔; building complex sentences |
| 0:45–1:10 | Semantics & truth tables | Models, truth-table construction for a sentence |
| 1:10–1:20 | Break | — |
| 1:20–1:45 | Satisfiability & validity | Satisfiable vs. valid (tautology) vs. unsatisfiable (contradiction) |
| 1:45–2:00 | Logical equivalence | De Morgan's laws, implication elimination; proving equivalence via truth tables |

### Materials/Equipment
- Slides: truth-table templates, equivalence law reference sheet
- Starter notebook: sentence-evaluator skeleton (parses a fixed small grammar)

### Formative Check (in-class)
Exercise: build the truth table for `(P → Q) ↔ (¬P ∨ Q)` by hand and confirm it is a tautology.

### Link to Lab/Assessment
Lab 6: Build a propositional-logic truth-table generator/evaluator in Python; use it to check
logical equivalence of two sentences.
