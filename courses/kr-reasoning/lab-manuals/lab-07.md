# Lab Manual 7 — A Toy Description-Logic Subsumption Checker

**Duration:** 3 hours | **Prerequisite:** Week 7 lecture

## Objectives
Implement a model-based subsumption checker for a small fixed DL fragment, and sketch an RDF
triple representation of a small ontology fragment.

## Setup
Create `lab07.ipynb`. Represent concept expressions as nested tuples, as in the lecture content.

## Procedure
1. **Task A — Extension function:** implement `extension(concept, domain, roles, concepts)`
   supporting `and`, `not`, `exists`, `forall`; test it on 4 concept expressions against a small
   hand-built domain/roles/concepts model.
2. **Task B — Subsumption checker:** implement `subsumes(d_concept, c_concept, domain, roles,
   concepts)`; test it on at least one TBox axiom that holds in your model and one that does not.
3. **Task C — DL-to-FOL translation:** in a markdown cell, translate 3 DL concept expressions
   from Task A into FOL formulas by hand, following the lecture's translation table.
4. **Task D — RDF sketch:** represent the same small domain as a list of RDF-style
   `(subject, predicate, object)` triples (reusing the Week 1 `TripleStore` if convenient), and
   write 1–2 sentences comparing this representation to the DL concepts/roles used in Tasks A–B.

## Expected Output
A notebook with Tasks A–D; a working `extension`/`subsumes` pair correctly distinguishing a
holding axiom from a non-holding one, 3 correct DL-to-FOL translations, and an RDF triple sketch.

## Submission
Submit `lab07.ipynb` by the end of the lab session.
