# Assignment 2 — Multi-Agent RL & Algorithmic Game Theory (Weeks 5–7)

**Weight:** 5% of course grade (one of 2 problem sets, 10% total) | **Assigned:** Week 7 |
**Due:** Start of Week 9 (before the Midterm Exam)

## Instructions
Submit a single Jupyter notebook `assignment02.ipynb` answering all questions below. Show your
work (code + a brief written explanation) for each question.

## Questions
1. **(Independent Q-learners, 25 pts)** Implement `run_independent_learners` as in the Week 5
   lecture content. Run it on a provided repeated matching-pennies-style zero-sum game (no
   pure-strategy Nash equilibrium) and on a provided repeated coordination game (a pure-strategy
   Nash equilibrium exists and is also the social optimum). Plot each player's action-frequency
   trajectory for both games, and in 3–5 sentences explain why one setting converges to stable
   play and the other cycles, referencing non-stationarity.
2. **(Nash-equilibrium computation, 20 pts)** For 2 provided general-sum 3x3 bimatrix games,
   find at least one Nash equilibrium of each via support enumeration over small supports. For 1
   provided two-player zero-sum game, compute the game's value via `solve_zero_sum_fictitious_play`
   (or an LP solver, if available) and report it.
3. **(PPAD-completeness reflection, 15 pts)** In 150–250 words, explain precisely what
   "PPAD-complete" means and why Nash-equilibrium computation being PPAD-complete for
   general-sum games is a meaningfully different kind of hardness result from NP-completeness.
4. **(Correlated equilibria, 20 pts)** For one of the general-sum games from Question 2, set up
   and solve the correlated-equilibrium linear program. Report the best social welfare achievable
   under the best correlated equilibrium found, and compare it to the best Nash equilibrium found
   in Question 2.
5. **(VCG mechanism, 20 pts)** Implement `vcg_mechanism` for a provided 5-bidder single-item
   auction (true valuations given). Confirm it reduces exactly to the second-price auction rule.
   Then sweep one bidder's reported value above and below their true valuation (holding the
   mechanism and others' reports fixed), plot their resulting utility, and confirm it is
   maximized exactly at the truthful report.

## Submission
Upload `assignment02.ipynb` via the course submission system. Late policy per syllabus
(`course-plan.md` §7).
