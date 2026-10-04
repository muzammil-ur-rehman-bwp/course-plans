# Lab Manual 2 — Agents: Simple Reflex & Model-Based Reflex

**Duration:** 3 hours | **Prerequisite:** Week 2 lecture; `TableDrivenAgent` from Lab 1

## Objectives
Implement a simple reflex agent and a model-based reflex agent for the vacuum-cleaner world, and
compare their behavior.

## Setup
Create `lab02.ipynb`. Use a 2-cell vacuum world (locations `"A"` and `"B"`), each either
`"Clean"` or `"Dirty"`.

## Procedure
1. **Task A — Environment simulator:** implement a `VacuumWorld` class that tracks the agent's
   location and the dirt status of each cell, and a `step(action)` method that updates the
   world and returns the new percept `(location, status)`.
2. **Task B — Simple reflex agent:** implement `simple_reflex_vacuum_agent(location, status)` as
   shown in lecture; run it in your simulator for 10 steps from a random dirt configuration and
   print the full action trace.
3. **Task C — Model-based reflex agent:** implement `ModelBasedVacuumAgent` that tracks an
   internal belief about both cells' dirt status and stops (`"NoOp"`) once it believes both are
   clean; run it for the same starting configuration and compare the number of steps taken to
   the simple reflex agent.
4. **Task D — Environment classification:** classify the vacuum world against the Week 2
   environment-property table (observable, deterministic, episodic, static, discrete,
   single-agent) and justify each entry in a markdown cell.

## Expected Output
A notebook with Tasks A–D; both agents must correctly clean a dirty 2-cell world from any
starting configuration.

## Submission
Submit `lab02.ipynb` by the end of the lab session.
