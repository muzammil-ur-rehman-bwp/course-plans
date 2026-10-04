# Lab Manual 13 — Lexical Ontology Matcher

**Duration:** 3 hours | **Prerequisite:** Week 13 lecture

## Objectives
Implement a toy lexical ontology matcher and audit its proposed correspondences between two
independently named class lists.

## Setup
1. Reuse your course virtual environment.
2. Create `lab13.ipynb`.

## Procedure
1. **Task A — Matcher implementation:** implement `tokenize`, `jaccard`, and
   `propose_alignments` from the Week 13 lecture content. Reproduce the §5 in-class exercise
   example and inspect the proposed correspondences.
2. **Task B — Audit:** for the Task A output, manually classify each proposed correspondence as
   correct, incorrect, or ambiguous, and write one sentence justifying each classification.
3. **Task C — Synonymy failure case:** construct a small example (e.g., `Employee`/`Staff`,
   `Vehicle`/`Automobile`) where true equivalence has zero lexical overlap, confirm the matcher
   misses it, and describe (in a markdown cell) what structural or instance-based evidence could
   catch it instead.
4. **Task D — Mini-challenge:** write 4 competency questions for a toy "university" domain
   ontology, sketch a minimal class hierarchy (as a markdown list, no OWL required) that can
   answer all 4, and identify which single added class would be needed if a 5th, harder
   competency question were added.

## Expected Output
A notebook with four clearly labeled cells/sections (A–D), each producing correct, runnable
output (A–C) or a clear written answer (D).

## Submission
Export/submit `lab13.ipynb` via the course submission system by the end of the lab session.
