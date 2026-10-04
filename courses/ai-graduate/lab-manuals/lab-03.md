# Lab Manual 3 — Alpha-Beta Pruning & Nash Equilibrium

**Duration:** 3 hours | **Prerequisite:** Week 3 lecture

## Objectives
Implement alpha-beta pruning with node-count instrumentation and compute pure-strategy Nash
equilibria for small normal-form games.

## Setup
1. Reuse your course virtual environment.
2. Create `lab03.ipynb`.

## Procedure
1. **Task A — Minimax baseline:** implement plain minimax for Tic-Tac-Toe, counting total nodes
   visited to solve from the empty board.
2. **Task B — Alpha-beta:** implement alpha-beta pruning (per the Week 3 lecture content) for the
   same game, counting total nodes visited. Report the percentage reduction vs. Task A.
3. **Task C — Nash equilibrium:** implement `find_pure_nash_equilibria` from the Week 3 lecture
   content; encode (i) the Prisoner's Dilemma and (ii) a Battle-of-the-Sexes-style coordination
   game as payoff matrices, and report all pure Nash equilibria found for each.
4. **Task D — Mini-challenge:** show, by computing payoffs directly, that in your Prisoner's
   Dilemma encoding the (Cooperate, Cooperate) outcome gives both players a strictly higher
   payoff than the Nash equilibrium found in Task C, confirming it is not itself an equilibrium.

## Expected Output
A notebook with four clearly labeled cells/sections (A–D), each producing correct, runnable
output with a one-line comment explaining the approach.

## Submission
Export/submit `lab03.ipynb` via the course submission system by the end of the lab session.
