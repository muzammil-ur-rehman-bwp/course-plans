# Week 3 Lecture Plan — Knowledge Representation and Reasoning
## Topic: First-Order Logic in Depth — Syntax, Semantics, Translation

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Identify the syntactic components of FOL — terms, predicates, functions, quantifiers.
   (*Remember, Understand*)
2. Explain FOL semantics in terms of models (a domain plus an interpretation) and satisfaction.
   (*Understand*)
3. Translate English sentences, including those with nested quantifiers, into FOL correctly.
   (*Apply*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:20 | Why FOL? | The expressiveness gap propositional logic cannot close (recap Week 1's worked example) |
| 0:20–0:50 | Syntax | Terms (constants, variables, functions), atomic/complex sentences, quantifiers and scope |
| 0:50–1:00 | Break | — |
| 1:00–1:30 | Semantics | Models (domain + interpretation), satisfaction of quantified sentences, worked small-model examples |
| 1:30–2:00 | Translation | English-to-FOL worked examples; the `∀∃` vs. `∃∀` scope trap |

### Materials/Equipment
- Slides: FOL syntax cheat sheet, small-model satisfaction diagrams, scope-trap examples
- Starter notebook: finite-domain FOL model skeleton

### Formative Check (in-class)
Exercise: for the sentence `∀x ∃y Likes(x, y)` vs. `∃y ∀x Likes(x, y)`, construct a tiny 2-object
model where one is true and the other false, and explain why.

### Link to Lab/Assessment
Lab 3: Represent a small finite-domain FOL model in Python and evaluate quantified sentences
against it by brute-force enumeration; translate English sentences to FOL.
