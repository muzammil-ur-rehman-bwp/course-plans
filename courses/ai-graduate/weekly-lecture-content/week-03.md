# Week 3 — Lecture Content: Game Theory & Adversarial Search I

## 1. Minimax, Restated
For a two-player, zero-sum, perfect-information game, the minimax value of a state s is:
```
minimax(s) = Utility(s)                                        if s is terminal
           = max over actions a of minimax(Result(s, a))       if Player(s) == MAX
           = min over actions a of minimax(Result(s, a))       if Player(s) == MIN
```

## 2. Alpha-Beta Pruning
Alpha-beta augments minimax with two bounds: α (the best value MAX can guarantee so far on the
path to the root) and β (the best value MIN can guarantee so far). A branch is pruned as soon as
it is proven irrelevant to the final decision:

```python
def alpha_beta(state, depth, alpha, beta, maximizing, game):
    if game.is_terminal(state) or depth == 0:
        return game.utility(state)
    if maximizing:
        value = float("-inf")
        for action in game.actions(state):
            value = max(value, alpha_beta(game.result(state, action), depth - 1,
                                           alpha, beta, False, game))
            alpha = max(alpha, value)
            if alpha >= beta:
                break  # beta cutoff: MIN will never let MAX reach this branch
        return value
    else:
        value = float("inf")
        for action in game.actions(state):
            value = min(value, alpha_beta(game.result(state, action), depth - 1,
                                           alpha, beta, True, game))
            beta = min(beta, value)
            if beta <= alpha:
                break  # alpha cutoff
        return value
```

### 2.1 Correctness Argument (Proof Sketch by Induction on Depth)
**Claim.** `alpha_beta(s, d, -inf, +inf, maximizing, game)` returns exactly `minimax(s)`.

**Base case (d = 0 or s terminal).** Both functions return `Utility(s)` directly. Equal.

**Inductive step.** Assume alpha-beta (called with any valid α, β window) returns the true
minimax value for every state at depth < d, *provided the true value falls within [α, β]*. At a
MAX node at depth d, minimax takes the max over children's true minimax values. Alpha-beta
visits children in order, maintaining a running best value and tightening α. Two cases:
- **No cutoff occurs.** Every child is explored with a window that still contains its true value
  (a standard sub-claim proved alongside this one), so by the inductive hypothesis each child
  returns its true minimax value, and alpha-beta's max over them equals minimax's max over them.
- **A cutoff occurs** (α ≥ β at some child). This only happens when the current running value
  already proves the remaining children's true values cannot change the MAX node's final value
  *as seen by its parent* — a child whose true value is ≤ the current α cannot increase the
  running max, so skipping it changes nothing about the value returned. Hence the returned value
  still equals minimax's value even though fewer nodes were visited.

The symmetric argument holds for MIN nodes. By induction on depth, alpha-beta returns the exact
minimax value at every depth, for any game tree — it only ever prunes branches that are provably
irrelevant to the final decision, never branches that could change it. This is why alpha-beta is
described as returning the same value as minimax while potentially visiting far fewer nodes: with
a good move ordering, it can reduce the effective branching factor from b to roughly √b.

## 3. Imperfect-Information Games (Brief)
Minimax and alpha-beta assume **perfect information**: both players always know the exact game
state. Many real games (card games, most negotiations) are **imperfect-information**: a player
must act under uncertainty about the opponent's private information (e.g., their hand). This
breaks the clean minimax recursion because a "state" is no longer common knowledge; it motivates
representing the game instead as an extensive-form game with information sets, and solving for
equilibrium strategies rather than a single minimax value. This course treats this only at the
level of "here is why minimax is not enough" — a full treatment (e.g., counterfactual regret
minimization) is a specialized topic beyond this course's scope.

## 4. Normal-Form Games
A **normal-form (strategic-form) game** for n players is a tuple ⟨N, {Aᵢ}, {uᵢ}⟩ where N is the
set of players, Aᵢ is player i's set of available (pure) strategies, and uᵢ: A₁×...×Aₙ → ℝ is
player i's payoff (utility) function over the joint strategy profile. For two players this is
usually drawn as a payoff matrix.

**Dominant strategy.** Strategy a*ᵢ strictly dominates aᵢ for player i if, for every possible
choice of the other players' strategies, a*ᵢ gives player i a strictly higher payoff than aᵢ.

## 5. Nash Equilibrium — Formal Definition
A strategy profile (a₁*, a₂*, ..., aₙ*) is a **Nash equilibrium** if, for every player i,
```
uᵢ(aᵢ*, a₋ᵢ*) ≥ uᵢ(aᵢ, a₋ᵢ*)   for all aᵢ ∈ Aᵢ
```
where a₋ᵢ* denotes the other players' equilibrium strategies held fixed. In words: no single
player can improve their own payoff by unilaterally deviating, given everyone else's strategy is
fixed. A Nash equilibrium is **not** necessarily a jointly optimal (highest total payoff)
outcome — the canonical Prisoner's Dilemma has a unique Nash equilibrium (Defect, Defect) that is
strictly worse for both players than (Cooperate, Cooperate), which is not an equilibrium because
each player can unilaterally improve by defecting.

```python
def is_nash_equilibrium(payoff_row, payoff_col, i, j):
    """payoff_row, payoff_col: 2D payoff tables for row/column player.
    (i, j): a candidate pure-strategy profile (row player plays i, column player plays j).
    Returns True iff neither player can improve by unilaterally deviating."""
    n_rows, n_cols = len(payoff_row), len(payoff_row[0])
    row_optimal = all(payoff_row[i][j] >= payoff_row[r][j] for r in range(n_rows))
    col_optimal = all(payoff_col[i][j] >= payoff_col[i][c] for c in range(n_cols))
    return row_optimal and col_optimal

def find_pure_nash_equilibria(payoff_row, payoff_col):
    n_rows, n_cols = len(payoff_row), len(payoff_row[0])
    return [(i, j) for i in range(n_rows) for j in range(n_cols)
            if is_nash_equilibrium(payoff_row, payoff_col, i, j)]
```

## 6. In-Class/Lab Exercise
Encode the Prisoner's Dilemma payoff matrices and run `find_pure_nash_equilibria` to confirm
(Defect, Defect) is the unique pure Nash equilibrium; discuss why it is not the jointly best
outcome.
