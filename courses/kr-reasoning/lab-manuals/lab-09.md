# Lab Manual 9 — Closed-World Queries and Default-Logic Extensions

**Duration:** 3 hours | **Prerequisite:** Week 9 lecture

## Objectives
Implement a CWA query engine and a default-logic extension builder; demonstrate genuine
conclusion retraction as new facts are added.

## Setup
Create `lab09.ipynb`.

## Procedure
1. **Task A — CWA engine:** implement `cwa_query(facts, rules, atom)` from the lecture content;
   test it on a small rule base, confirming an atom not derivable is correctly treated as false
   (not "unknown").
2. **Task B — Default class and extension builder:** implement `Default` and `apply_defaults`
   from the lecture content; test on the bird/penguin example, confirming the default fires when
   unblocked.
3. **Task C — Retraction demonstration:** using the same default rule set from Task B, add a new
   fact that blocks the default's justification, and show `apply_defaults` now excludes the
   previously-derived conclusion. State explicitly, in a markdown cell, why this is impossible for
   the Week 5 forward-chaining engine on strict rules.
4. **Task D — A second domain:** build a different small default-logic scenario of your choosing
   (e.g., "an email from a known contact is not spam, by default") with at least 2 defaults, and
   demonstrate one default firing and one being blocked.

## Expected Output
A notebook with Tasks A–D; a working CWA engine, a working default-logic extension builder, and
two demonstrated retraction cases (bird/penguin and your own domain).

## Submission
Submit `lab09.ipynb` by the end of the lab session.
