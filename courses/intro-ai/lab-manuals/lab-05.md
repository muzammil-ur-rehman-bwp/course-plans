# Lab Manual 5 — Adversarial Search: Minimax & Alpha-Beta Pruning

**Duration:** 3 hours | **Prerequisite:** Week 5 lecture

## Objectives
Implement minimax and alpha-beta pruning for Tic-Tac-Toe and compare nodes explored.

## Setup
Create `lab05.ipynb`; use the Tic-Tac-Toe board representation (`legal_moves`, `result`,
`winner`, `is_terminal`, `utility`) from the lecture content.

## Procedure
1. **Task A — Game setup:** implement/paste the Tic-Tac-Toe board functions; verify `winner`
   and `is_terminal` work correctly on 3–4 example boards.
2. **Task B — Minimax:** implement `minimax(state, maximizing)` and `best_move(state)`;
   instrument it to count total recursive calls (nodes visited).
3. **Task C — Alpha-beta pruning:** implement `alphabeta(state, maximizing, alpha, beta)` with
   the same instrumentation; confirm it chooses the same best move as minimax on at least 3
   different board states.
4. **Task D — Analysis:** for the empty starting board, report nodes visited by minimax vs.
   alpha-beta pruning and briefly explain the reduction (2–3 sentences).

## Expected Output
A notebook with Tasks A–D; minimax and alpha-beta pruning must agree on best move for every test
board, with alpha-beta pruning visiting strictly fewer (or equal) nodes.

## Submission
Submit `lab05.ipynb` by the end of the lab session.
