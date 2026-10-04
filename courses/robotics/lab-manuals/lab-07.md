# Lab Manual 7 — Torque & Actuator Calculations

**Duration:** 3 hours | **Prerequisite:** Week 7 lecture

## Objectives
Compute torque requirements for simple arm/wheel scenarios and relate them to actuator specs.

## Setup
Create `lab07.ipynb`.

## Procedure
1. **Task A — Arm torque:** for 4 given `(mass, link_length)` combinations, compute the required
   shoulder-joint torque (worst case, horizontal arm).
2. **Task B — Feasibility check:** given a reference handout of small-servo torque ratings,
   determine which of the Task A scenarios are feasible with a single servo vs. requiring
   gearing or a larger motor.
3. **Task C — Gearing:** for one infeasible scenario, compute the gear ratio `N` needed to make
   it feasible, and the resulting output speed reduction.
4. **Task D — Wheeled robot:** given a robot's mass and a desired acceleration, compute the
   required wheel torque.

## Expected Output
A notebook with Tasks A–D and a short written feasibility conclusion for each scenario.

## Submission
Submit `lab07.ipynb` by the end of the lab session.
