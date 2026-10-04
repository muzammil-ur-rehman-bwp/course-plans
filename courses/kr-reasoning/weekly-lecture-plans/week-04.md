# Week 4 Lecture Plan — Knowledge Representation and Reasoning
## Topic: First-Order Inference — Unification, Resolution, Skolemization

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Apply the unification algorithm to compute the most general unifier of two terms/atoms, or
   correctly report failure. (*Apply*)
2. Apply resolution to first-order clauses by unifying complementary literals before resolving.
   (*Apply*)
3. Trace Skolemization on a sentence containing existential quantifiers, and explain, at a
   result level, why FOL resolution is sound and refutation-complete. (*Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:20 | Why unification? | Propositional resolution requires exact literal match; FOL literals need matching via substitution |
| 0:20–0:55 | Unification algorithm | Substitutions, the occurs check, unifying terms recursively, worked traces |
| 0:55–1:05 | Break | — |
| 1:05–1:35 | FOL resolution | Variable renaming before resolving, unify-then-resolve, a worked derivation |
| 1:35–2:00 | Skolemization & meta-theory | Removing `∃` via Skolem functions/constants (conceptual, worked example); soundness/completeness (statement only) |

### Materials/Equipment
- Slides: unification trace diagrams, FOL resolution derivation, Skolemization worked example
- Starter notebook: unification function skeleton

### Formative Check (in-class)
Exercise: unify `Likes(x, mother(x))` with `Likes(john, mother(john))` by hand, showing the
substitution built one step at a time; identify why the occurs check matters for
`P(x)` vs. `P(f(x))`.

### Link to Lab/Assessment
Lab 4: Implement the unification algorithm from scratch (with occurs check) and a FOL resolution
step that unifies complementary literals before resolving.
