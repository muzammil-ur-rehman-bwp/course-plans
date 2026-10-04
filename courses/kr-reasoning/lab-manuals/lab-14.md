# Lab Manual 14 — An Integrated Knowledge-Based Agent

**Duration:** 3 hours | **Prerequisite:** Week 14 lecture

## Objectives
Integrate rule-based and frame-based reasoning into one agent with a derivation trace; query it
on a toy domain.

## Setup
Create `lab14.ipynb`; reuse your Week 5 `Rule` class and Week 6 `Frame` class (copy or re-import
them).

## Procedure
1. **Task A — Unified agent:** implement `KnowledgeBasedAgent` (`add_fact`, `add_rule`,
   `add_frame`, `_expand_taxonomic_facts`, `ask_forward`, `ask_backward`, `ask`) from the lecture
   content.
2. **Task B — Toy domain:** build a toy domain (diagnostic, classification, or eligibility-style)
   with at least 4 facts, 4 rules, and 2 frames (at least one frame feeding a rule premise via
   `_expand_taxonomic_facts`).
3. **Task C — Queries both ways:** run at least 3 queries using `strategy="forward"` and the same
   3 queries using `strategy="backward"`; confirm agreement on each.
4. **Task D — Explanation trace:** implement `explain(agent, fact, depth=0)`; use it to print the
   full derivation chain for at least 2 derived facts, following the chain back to given facts.

## Expected Output
A notebook with Tasks A–D; a working integrated agent, a toy domain exercising both rules and
frames, 3 queries agreeing across both strategies, and 2 full, correct derivation traces.

## Submission
Submit `lab14.ipynb` by the end of the lab session.
