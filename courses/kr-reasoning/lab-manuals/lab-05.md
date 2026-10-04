# Lab Manual 5 — A General-Purpose Rule Engine

**Duration:** 3 hours | **Prerequisite:** Week 5 lecture

## Objectives
Build a reusable Python rule engine (forward and backward chaining) and apply it, unchanged, to
two different toy domains.

## Setup
Create `lab05.ipynb`.

## Procedure
1. **Task A — Rule representation:** implement the `Rule` dataclass and `is_applicable` method
   from the lecture content.
2. **Task B — Forward chaining with trace:** implement `forward_chain(facts, rules, query=None)`
   that also returns a `derived_by` dict mapping each derived fact to the rule that derived it.
3. **Task C — Backward chaining:** implement `backward_chain(facts, rules, goal, visited=None)`,
   including cycle protection via `visited`.
4. **Task D — Two domains, one engine:** build two distinct rule bases of at least 5 rules each
   (e.g., animal identification and a simple fault-diagnosis domain) and run both
   `forward_chain` and `backward_chain` on each **without modifying the engine functions**.
5. **Task E — Agreement check:** for 3 queries per domain, confirm `backward_chain` returns
   `True` if and only if `forward_chain`'s derived fact set contains that query.

## Expected Output
A notebook with Tasks A–E; one unmodified engine applied correctly to two rule bases, with
forward/backward agreement confirmed on 6 total queries.

## Submission
Submit `lab05.ipynb` by the end of the lab session.
