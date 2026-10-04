# Lab Manual 12 — Variable Elimination for Bayesian Networks

**Duration:** 3 hours | **Prerequisite:** Week 12 lecture

## Objectives
Implement a `Factor` class and a `variable_elimination` query function; compare its work against
plain enumeration.

## Setup
Create `lab12.ipynb`.

## Procedure
1. **Task A — Factor class:** implement `Factor` (`multiply`, `sum_out`) and `restrict` from the
   lecture content; test `multiply` and `sum_out` separately on two small hand-built factors,
   checking results against hand computation.
2. **Task B — Variable elimination:** implement `variable_elimination`; run it on the
   Burglary/Earthquake/Alarm-style 3-node network to compute `P(Burglary | Alarm=True)`, and
   confirm the result matches an enumeration-based implementation (reuse or adapt one from an
   earlier reference, or implement a small one here) to at least 4 decimal places.
3. **Task C — A 5-node network:** extend the network with 2 more nodes (e.g., `JohnCalls`,
   `MaryCalls`, each depending on `Alarm`) and compute a query involving evidence on 2 variables;
   confirm the result still matches enumeration.
4. **Task D — Cost comparison:** count (or time) the number of factor-table entries multiplied
   in Task C's variable elimination versus the number of full joint rows enumeration would need
   to examine; report both numbers and discuss the difference in 2–3 sentences.

## Expected Output
A notebook with Tasks A–D; a correct `Factor`/`variable_elimination` implementation agreeing with
enumeration on both the 3-node and 5-node networks, and a cost comparison.

## Submission
Submit `lab12.ipynb` by the end of the lab session.
