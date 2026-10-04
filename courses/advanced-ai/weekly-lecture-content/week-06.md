# Week 6 — Lecture Content: Algorithmic Game Theory I — Equilibrium Computation

## 1. From Defining Equilibria to Computing Them
The graduate course defined a Nash equilibrium: a strategy profile where no player can improve
their payoff by unilaterally deviating. This week asks the computational question the definition
alone does not answer: **given a game, how hard is it to actually find one?** The answer differs
sharply between two cases.

## 2. Zero-Sum Two-Player Games: Polynomial-Time via Linear Programming
For a two-player zero-sum game with payoff matrix A (row player maximizes, column player
minimizes the same quantity), von Neumann's minimax theorem guarantees:
```
max_x  min_y  xᵀ A y   =   min_y  max_x  xᵀ A y
```
over mixed strategies x, y (probability distributions over actions), and this common value is
the game's value. Finding an optimal x is exactly a linear program: maximize v subject to
Σᵢ xᵢ Aᵢⱼ ≥ v for every column j, Σᵢ xᵢ = 1, x ≥ 0 — a linear program with one variable per
action plus v, solvable in time polynomial in the game's size by standard LP algorithms.

```python
def solve_zero_sum_fictitious_play(A, iterations=10000):
    """Fictitious play: a simple, from-scratch approximation algorithm (converges to the
    game value for zero-sum games) when an LP solver is unavailable. A: payoff matrix,
    rows = row player's actions, cols = column player's actions (row player maximizes)."""
    import numpy as np
    n_rows, n_cols = A.shape
    row_counts = np.zeros(n_rows)
    col_counts = np.zeros(n_cols)
    row_counts[0] += 1
    col_counts[0] += 1
    for _ in range(iterations):
        col_best = int(np.argmin(A @ (row_counts / row_counts.sum())))
        col_counts[col_best] += 1
        row_best = int(np.argmax(A @ (col_counts / col_counts.sum())))
        row_counts[row_best] += 1
    return row_counts / row_counts.sum(), col_counts / col_counts.sum()
```

## 3. General (Non-Zero-Sum) Games: PPAD-Completeness
For general-sum games (or zero-sum games with more than two players), no analogous
polynomial-time algorithm is known, and computing a Nash equilibrium is **PPAD-complete**
(Daskalakis, Goldberg & Papadimitriou, 2009, for the general case; Chen & Deng established the
even-two-player case). **PPAD** ("Polynomial Parity Argument on Directed graphs") is a complexity
class capturing problems whose solutions are guaranteed to exist by a *parity/fixed-point*
argument — concretely, PPAD's defining complete problem, END-OF-LINE, guarantees a second
endpoint exists in an exponentially large implicitly-defined directed graph where every node has
in-degree and out-degree at most 1, given one known endpoint, purely by a parity-of-degrees
argument, without that endpoint being efficiently *locatable* by any known method. Nash's
existence proof (via Brouwer's fixed-point theorem) has exactly this flavor: it guarantees an
equilibrium exists without constructively producing one, which is precisely the kind of
non-constructive existence argument PPAD was defined to capture.

**What "PPAD-complete" means, precisely, and why it matters:** PPAD sits (believed, not proven)
strictly between P and NP-hardness — it is not known to be solvable in polynomial time, but it is
also not believed to be NP-hard (NP-hardness would imply the existence of problems with no
guaranteed solution, which contradicts PPAD problems always having one by construction). A
PPAD-complete problem is among the hardest problems *in* PPAD: every other PPAD problem reduces
to it in polynomial time, so finding a polynomial-time algorithm for Nash-equilibrium computation
would yield one for every problem in PPAD, and no such algorithm is known despite extensive
effort. The practical upshot: for general games, we should not expect an efficient exact
equilibrium-finding algorithm, and real systems instead use approximation algorithms,
restrict to structured subclasses of games, or settle for a different (more tractable) solution
concept — such as a correlated equilibrium.

## 4. Support Enumeration for Small Bimatrix Games
A simple (exponential-time, but fine for small games) exact method: enumerate candidate supports
(subsets of each player's actions assumed to be played with positive probability), and for each
candidate pair of supports, solve the linear system requiring each player to be indifferent among
their support's actions (the defining condition of a mixed-strategy equilibrium) and verify no
action outside the support is strictly better.
```python
import itertools
import numpy as np

def is_nash_support(A, B, support_row, support_col):
    """A, B: row/column player's payoff matrices. Checks whether a mixed equilibrium exists
    with exactly these supports, via the indifference conditions (small-game exact check)."""
    n = len(support_row)
    if n != len(support_col):
        return None
    # Row player's mixed strategy over support_row makes column player indifferent on support_col
    sub_B = B[np.ix_(support_row, support_col)]
    try:
        x = np.linalg.solve(sub_B.T[:-1] - sub_B.T[1:], np.zeros(n - 1)) if n > 1 else np.array([1.0])
    except np.linalg.LinAlgError:
        return None
    return None  # full implementation left as the Week 6 lab exercise; sketch shown here
```

## 5. Correlated Equilibria
A **correlated equilibrium** relaxes Nash equilibrium: a trusted mediator draws a joint action
profile from some publicly known distribution and privately recommends each player their
component; it is an equilibrium if no player benefits from deviating from their recommendation,
given what that recommendation reveals (via the known joint distribution) about the others'
likely actions. Every Nash equilibrium is a correlated equilibrium (an uncorrelated one), but the
reverse does not hold — and unlike Nash equilibrium, **a correlated equilibrium is computable in
polynomial time via linear programming for any number of players**, because the defining
incentive constraints are linear in the (exponentially large but LP-tractable-to-optimize-over)
joint distribution, with no fixed-point/parity structure forcing PPAD-hardness.

## 6. In-Class/Lab Exercise
For a given 3x3 bimatrix game, run `solve_zero_sum_fictitious_play` if it is zero-sum, or
complete the support-enumeration sketch above for small supports if it is general-sum; separately
set up and solve the correlated-equilibrium linear program for the same game and compare the
social welfare achievable under the best correlated equilibrium against the best Nash
equilibrium found.
