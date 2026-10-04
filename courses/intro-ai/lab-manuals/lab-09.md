# Lab Manual 9 — Classical Planning: A Toy STRIPS Planner

**Duration:** 3 hours | **Prerequisite:** Week 9 lecture

## Objectives
Implement a tiny STRIPS-style planner (forward state-space search) for a toy blocks-world
domain.

## Setup
Create `lab09.ipynb`; use the `StripsAction` class and blocks-world example from the lecture
content as a starting point.

## Procedure
1. **Task A — Action schemas:** implement at least 3 STRIPS action schemas for an extended
   blocks-world domain (e.g., `Stack(x, y)`, `Unstack(x, y)`, `PutDown(x)`, `PickUp(x)`) with
   correct preconditions, add-lists, and delete-lists.
2. **Task B — Forward search planner:** implement `strips_forward_search(initial_state, goal,
   actions)` as shown in lecture; verify it finds a correct 1-step plan on the simple example
   from the lecture.
3. **Task C — Multi-step goal:** define an initial state and goal that require at least a
   2-step plan (e.g., moving a 3-block tower into a different configuration); run your planner
   and verify the returned plan actually achieves the goal when applied step by step.
4. **Task D — Analysis:** report the number of states expanded for the 2-step goal and briefly
   discuss (2–3 sentences) how this planner relates to the BFS/UCS solvers from Lab 3.

## Expected Output
A notebook with Tasks A–D; the planner must return a correct plan for both the 1-step and
2-step goals, verifiable by applying each action in sequence and checking the goal is satisfied.

## Submission
Submit `lab09.ipynb` by the end of the lab session.
