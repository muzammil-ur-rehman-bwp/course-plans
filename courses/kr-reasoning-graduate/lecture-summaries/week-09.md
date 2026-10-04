# Week 9 Summary — Midterm Exam; Markov Logic Networks in Depth

**Key takeaways:**
- An MLN is a set of (formula, weight) pairs; grounding over a finite domain produces one random
  variable per ground atom and one feature per formula grounding.
- The log-linear distribution P(x) = (1/Z)exp(Σ w_i n_i(x)) makes worlds satisfying more
  groundings of a positively-weighted formula exponentially more probable, without making any
  formula strictly required; weight → ∞ recovers classical FOL as a limiting case.
- Exact inference requires summing over an exponential number of worlds (tractable only at toy
  scale); real systems use MAP (weighted-satisfiability) or MCMC-based approximate inference.

**You should now be able to:** ground a small MLN by hand; implement a brute-force evaluator that
computes a toy MLN's full probability distribution; explain how changing a formula's weight shifts
relative world probabilities, including the hard-constraint limiting case.

**Next week:** Knowledge graphs — RDF triples, the TransE embedding model and its scoring
function, and link prediction as a reasoning task.
