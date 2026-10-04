# Lab Manual 4 — Inverse Kinematics

**Duration:** 3 hours | **Prerequisite:** Week 4 lecture; reuse FK from Lab 3

## Objectives
Implement analytical IK for a 2-link arm and visualize its reachable workspace.

## Setup
Continue from `lab03.ipynb` or start `lab04.ipynb`, importing the FK function from Lab 3.

## Procedure
1. **Task A — Analytical IK:** implement `inverse_kinematics_2link` including the reachability
   check; verify by feeding its output back into the Lab 3 FK function and confirming you
   recover the original target position.
2. **Task B — Elbow up/down:** for a reachable target, compute and report both IK solutions.
3. **Task C — Unreachable target:** demonstrate (with a specific target point and the `d`
   calculation) a case where no solution exists, and explain why using the arm's link lengths.
4. **Task D — Workspace visualization:** sweep joint angles through FK to plot the reachable
   workspace boundary.

## Expected Output
A notebook with Tasks A–D.

## Submission
Submit `lab04.ipynb` by the end of the lab session.
