# Week 7 Lecture Plan — Introduction to AI
## Topic: Propositional Logic Inference — Resolution, Forward/Backward Chaining

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Explain entailment and inference by enumeration (model checking). (*Understand*)
2. Apply conversion to conjunctive normal form (CNF) and the resolution rule to derive a conclusion. (*Apply*)
3. Analyze a Horn-clause knowledge base using forward and backward chaining. (*Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:20 | Entailment & model checking | KB ⊨ α; checking entailment by enumerating models |
| 0:20–0:45 | Conjunctive normal form | Converting sentences to CNF (implication elimination, De Morgan's, distribution) |
| 0:45–1:00 | Break | — |
| 1:00–1:30 | Resolution | The resolution rule; resolution refutation (proof by contradiction); trace on a small KB |
| 1:30–2:00 | Horn clauses & chaining | Definite clauses; forward chaining (data-driven) and backward chaining (goal-driven), traced by hand |

### Materials/Equipment
- Slides: CNF conversion steps, resolution trace, forward/backward chaining trace diagrams
- Starter notebook: CNF converter and resolution-refutation skeleton

### Formative Check (in-class)
Exercise: given a small KB and a query, trace forward chaining and backward chaining by hand and
confirm they reach the same conclusion.

### Link to Lab/Assessment
Lab 7: Implement resolution refutation for a small propositional knowledge base.
