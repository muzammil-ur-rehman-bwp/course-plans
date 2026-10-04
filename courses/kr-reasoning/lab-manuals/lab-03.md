# Lab Manual 3 — Finite-Domain FOL Models and Translation

**Duration:** 3 hours | **Prerequisite:** Week 3 lecture

## Objectives
Represent a small finite-domain FOL model in Python; evaluate quantified sentences against it by
brute-force enumeration; practice English-to-FOL translation.

## Setup
Create `lab03.ipynb`.

## Procedure
1. **Task A — Model representation:** define a domain (a Python `set`) and at least two binary
   relations (sets of tuples) over it, modeling a small domain of your choosing (e.g., `enrolled`,
   `prerequisiteOf`).
2. **Task B — Quantifier evaluators:** implement `forall(domain, predicate)` and
   `exists(domain, predicate)` as generic functions taking a one-argument Python predicate
   function, then compose them to evaluate at least 2 nested-quantifier sentences (one `∀∃`, one
   `∃∀`) over your relations.
3. **Task C — Scope-trap demonstration:** construct a relation where `∀x∃y R(x,y)` is `True` but
   `∃y∀x R(x,y)` is `False`; show both evaluate correctly with your Task B functions.
4. **Task D — Translation:** translate 4 English sentences (provided by the instructor, at least
   one requiring nested quantifiers and one requiring the implication-not-conjunction pattern)
   into FOL (as a markdown cell, using logical notation) and encode each as a Task B evaluation
   over a small model you construct for it.

## Expected Output
A notebook with Tasks A–D; working `forall`/`exists` evaluators, a correct scope-trap
demonstration, and 4 correctly translated and encoded sentences.

## Submission
Submit `lab03.ipynb` by the end of the lab session.
