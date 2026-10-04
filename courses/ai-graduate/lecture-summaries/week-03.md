# Week 3 Summary — Game Theory & Adversarial Search I

**Key takeaways:**
- Alpha-beta pruning returns exactly the same value as minimax; the correctness argument is an
  induction on tree depth showing every cutoff only skips branches that are provably irrelevant
  to the value the parent will see.
- Minimax/alpha-beta assume perfect information; imperfect-information games break this and
  require richer equilibrium-based solution concepts (briefly noted, not covered in depth).
- A normal-form game is ⟨N, {Aᵢ}, {uᵢ}⟩; a Nash equilibrium is a strategy profile where no player
  can unilaterally improve their payoff.
- A Nash equilibrium is not the same as a globally/jointly optimal outcome — the Prisoner's
  Dilemma is the canonical counterexample.

**You should now be able to:** prove alpha-beta's correctness relative to minimax; define a
normal-form game and a Nash equilibrium precisely; find pure-strategy Nash equilibria of a small
2x2 game.

**Next week:** Game theory II and multi-agent systems — cooperative vs. competitive settings and
an introduction to mechanism design/auction theory.
