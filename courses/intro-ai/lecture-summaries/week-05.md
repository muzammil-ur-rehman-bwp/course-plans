# Week 5 Summary — Adversarial Search

**Key takeaways:**
- Two-player zero-sum games are search problems over a game tree where MAX and MIN alternate,
  each optimizing their own utility.
- Minimax computes the value of a state by recursively assuming optimal play from both sides.
- Alpha-beta pruning returns the exact same result as minimax while skipping provably irrelevant
  branches, using alpha/beta bounds.
- Tic-Tac-Toe is small enough to solve exhaustively; larger games (chess) require evaluation
  functions and depth-limited search in practice.

**You should now be able to:** implement minimax and alpha-beta pruning for Tic-Tac-Toe; explain
why alpha-beta pruning never changes the result, only the number of nodes explored.

**Next week:** knowledge representation and propositional logic — syntax, semantics, truth
tables, and logical equivalence.
