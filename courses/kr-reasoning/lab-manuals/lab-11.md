# Lab Manual 11 — Allen's Interval Algebra and Path Consistency

**Duration:** 3 hours | **Prerequisite:** Week 11 lecture

## Objectives
Implement Allen's thirteen interval relations and a path-consistency propagator for a small
temporal constraint network.

## Setup
Create `lab11.ipynb`.

## Procedure
1. **Task A — Relation function:** implement `allen_relation(x, y)` from the lecture content;
   test it on at least 8 interval pairs covering at least 8 of the 13 relations.
2. **Task B — Small composition table:** build (by hand, as a Python dict) a partial composition
   table covering at least 6 relation pairs relevant to your Task C network (the instructor will
   provide the needed entries, or you may derive them by reasoning about interval endpoints
   directly).
3. **Task C — Path consistency:** implement `propagate_path_consistency`; build a 4-interval
   network with some relations only partially known (a set of possible relations, not a single
   one), and run propagation to tighten them.
4. **Task D — Inconsistent network:** construct a 3-interval network whose stated relations are
   mutually inconsistent, and show `propagate_path_consistency` tightens some pair's possible
   relations to the empty set.

## Expected Output
A notebook with Tasks A–D; a correct relation function covering most of the 13 relations, a
working path-consistency propagator, and a correctly-detected inconsistent network.

## Submission
Submit `lab11.ipynb` by the end of the lab session.
