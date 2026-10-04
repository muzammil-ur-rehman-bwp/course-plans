# Lab Manual 3 — Many-Valued and Paraconsistent Logic Evaluators

**Duration:** 3 hours | **Prerequisite:** Week 3 lecture

## Objectives
Implement K3, Ł3, and Belnap–Dunn FDE evaluators; demonstrate the K3/Ł3 implication divergence
and FDE's non-explosion property.

## Setup
1. Reuse your Week 1 environment.
2. Create `lab03.ipynb`.

## Procedure
1. **Task A — K3/Ł3 implementation:** implement `k3_and`, `k3_or`, `k3_not`, `k3_implies`, and
   `luk_implies` exactly as in the Week 3 lecture content.
2. **Task B — Divergence demonstration:** evaluate `k3_implies(U,U)` and `luk_implies(U,U)` and
   confirm they differ (U vs. T). Construct one additional formula where K3 and Ł3 *agree* and one
   more (beyond `U→U`) where they *disagree*, and explain each disagreement in one sentence.
3. **Task C — FDE implementation:** implement `fde_not`, `fde_and`, `fde_or` exactly as in the
   lecture content.
4. **Task D — Non-explosion demonstration:** build a toy KB with one atom `P` given value B
   (contradictory evidence) and two unrelated atoms with ordinary, non-contradictory evidence.
   Evaluate a query on an unrelated atom and confirm it returns its own evidence-based value, not
   B; write one sentence contrasting this with what classical logic would conclude from the same
   KB (via explosion).

## Expected Output
A notebook with four clearly labeled cells/sections (A–D), each producing correct, runnable
output and the required written comparisons.

## Submission
Export/submit `lab03.ipynb` via the course submission system by the end of the lab session.
