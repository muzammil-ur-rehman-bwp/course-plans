# Lab Notes 5 — Adversarial Search: Minimax & Alpha-Beta Pruning

**Concept recap:** minimax recursively assumes optimal play by both MAX and MIN; alpha-beta
pruning returns the identical result while skipping branches once `alpha >= beta`, because that
branch is provably irrelevant to the final decision at an ancestor node.

**Common pitfalls:**
- Swapping which player is "maximizing" partway through the recursion — the role must flip at
  every ply, and a bug here silently produces plausible-looking but wrong moves.
- Forgetting that alpha and beta must be **passed down and updated** through recursive calls,
  not reset at each call — resetting them disables pruning entirely without raising an error.
- Comparing node counts between minimax and alpha-beta on a board state with very few legal
  moves remaining (e.g., near the end of the game) — pruning has little to work with there, so
  use the empty or nearly-empty board for the clearest comparison.

**Debugging tip:** add a global counter incremented once per call to `minimax`/`alphabeta` and
print it after each top-level `best_move` call; if alpha-beta's count ever exceeds minimax's
count on the same input, there is a bug in the pruning condition.

**Instructor tip:** show the move-ordering effect — running alpha-beta with the best move
examined first vs. last at each node — to make vivid that pruning efficiency depends on the
order moves are considered, not just the pruning rule itself.
