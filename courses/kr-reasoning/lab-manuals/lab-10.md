# Lab Manual 10 — Grounded STRIPS and Partial-Order Planning

**Duration:** 3 hours | **Prerequisite:** Week 10 lecture

## Objectives
Extend STRIPS with variables/grounding; implement a small partial-order planner; construct
planning-graph levels by hand.

## Setup
Create `lab10.ipynb`.

## Procedure
1. **Task A — Variabilized schemas:** implement `ActionSchema.ground` and `GroundedAction` from
   the lecture content; define at least 2 schemas for a toy domain of your choosing and ground
   them over a 3–4 object domain, printing every resulting grounded action.
2. **Task B — Partial-order planner:** implement a minimal `Plan` class (steps, causal links,
   orderings) plus `pop_resolve_open_precondition` and `pop_resolve_threats`; build a plan for a
   2–3 goal problem in your domain, printing the final causal links and orderings.
3. **Task C — Threat demonstration:** construct a scenario in your domain where a third action
   genuinely threatens an existing causal link, and show your `pop_resolve_threats` adds an
   ordering constraint that resolves it.
4. **Task D — Planning graph by hand:** in a markdown cell, construct `P0`, `A0`, `P1` by hand for
   a small 2-action domain (facts and actions provided or chosen), and list at least one mutex
   pair at the `A0` level with a one-sentence justification.

## Expected Output
A notebook with Tasks A–D; a working grounding function, a working partial-order planner with a
genuine threat-resolution example, and a correctly hand-built first planning-graph level.

## Submission
Submit `lab10.ipynb` by the end of the lab session.
