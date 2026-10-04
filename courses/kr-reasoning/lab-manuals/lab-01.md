# Lab Manual 1 — A Mini-World Triple Store

**Duration:** 3 hours | **Prerequisite:** Week 1 lecture

## Objectives
Implement a simple triple-store knowledge base; practice comparing representation choices against
the Week 1 desiderata.

## Setup
Install Python 3.10+ and Jupyter (or use Google Colab). Create `lab01.ipynb`.

## Procedure
1. **Task A — Triple store:** implement the `TripleStore` class from the lecture content (`add`
   and `query` methods). Test `query` with every combination of `None`/non-`None` arguments.
2. **Task B — Build a mini-world:** populate the triple store with at least 12 facts about a
   small domain of your choosing (e.g., a university department: students, courses, instructors,
   prerequisites). Use at least 3 distinct predicates.
3. **Task C — Querying:** write 4 queries against your mini-world (e.g., "all courses alice has
   completed," "who teaches CS101") using only `query()` calls, and print readable results.
4. **Task D — Desiderata discussion:** in a markdown cell, discuss in 3–5 sentences one piece of
   knowledge about your domain that the triple store represents *awkwardly* (e.g., "every student
   who has completed two specific courses is eligible for a third" — a rule, not a single fact),
   and name which desideratum (expressiveness, inferential efficiency, naturalness) suffers.

## Expected Output
A notebook with Tasks A–D; a working `TripleStore`, a populated mini-world of at least 12 triples,
4 working queries, and a short written desiderata discussion.

## Submission
Submit `lab01.ipynb` by the end of the lab session.
