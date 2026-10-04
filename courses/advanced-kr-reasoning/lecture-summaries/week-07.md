# Week 7 Summary — Neuro-Symbolic Integration I: Differentiable and Fuzzy Logic

**Key takeaways:**
- t-norms (product, Gödel/min, Łukasiewicz) relax conjunction to [0,1]; t-conorms relax
  disjunction via De Morgan duality; negation relaxes to `1-a`.
- The product t-norm is preferred for gradient-based learning because every input retains nonzero
  gradient, unlike Gödel's near-everywhere-zero gradient on the non-minimal argument.
- A differentiable soft-logic loss (`1 - mean soft-truth-value`) lets gradient descent learn to
  better satisfy a symbolic constraint with no labeled examples of the rule itself.

**You should now be able to:** implement and compare t-norms/t-conorms; build and train a
differentiable soft-logic loss in PyTorch; explain the product-vs-Gödel gradient argument.

**Next week:** Neuro-symbolic integration II — neural theorem proving and KG-embedding/
constraint hybrids; midterm review. **Quiz 3** (Weeks 5–6) this week.
