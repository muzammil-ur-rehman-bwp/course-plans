# Week 2 Lecture Plan — Advanced Knowledge Representation and Reasoning (Post Graduate)
## Topic: Higher-Order Logic and Type Theory

**Duration:** 2 hours lecture + 3 hour research seminar/lab

### Learning Objectives (Bloom's Level)
1. State a concrete property expressible in second-order logic but not FOL, and explain why.
   (*Understand*)
2. Type-check and β-reduce simply-typed lambda terms by hand. (*Apply*)
3. Implement a small simply-typed lambda calculus interpreter in Python. (*Apply, Analyze*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:15 | Motivation | Why FOL cannot quantify over predicates; the induction-schema example |
| 0:15–0:45 | Second-order and higher-order logic | Quantifying over predicates/relations; expressive power hierarchy |
| 0:45–1:25 | Simply-typed lambda calculus | Types, abstraction, application, β-reduction, worked derivations |
| 1:25–1:50 | KR connections | Typed representations in compositional semantics and ontology formalisms |
| 1:50–2:00 | Synthesis | Recap: FOL limits → HOL → typed lambda calculus → typed KR systems |

### Materials/Equipment
- Slides: "Higher-Order Logic & Type Theory"
- Whiteboard for β-reduction derivations
- Jupyter for the lambda-calculus interpreter

### Formative Check (in-class)
Given the term `(λx:e. λy:e. R x y) a b`, type-check it against a signature and β-reduce it to
normal form by hand; state which single FOL limitation this term's construction illustrates.

### Link to Lab/Assessment
Lab 2: implement and test a simply-typed lambda calculus interpreter (see `lab-manuals/lab-02.md`).
