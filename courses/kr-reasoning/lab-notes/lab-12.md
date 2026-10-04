# Lab Notes 12 — Variable Elimination for Bayesian Networks

**Concept recap:** `multiply` combines two factors by joining over shared variables and taking
the product; `sum_out` marginalizes a variable by summing all table entries that agree on every
other variable; `variable_elimination` restricts to evidence, eliminates hidden variables one at
a time, then normalizes.

**Common pitfalls:**
- Forgetting to restrict factors to the observed evidence *before* eliminating any hidden
  variable — evidence must shrink a factor's table to only the rows consistent with the observed
  value, not just be "kept in mind" and applied at the end.
- In `multiply`, building `combined_vars` incorrectly (e.g., losing the original variable order
  needed to look up `key_self`) — the lookup key for each parent factor must use *that factor's
  own* variable order, not the combined order, or the multiplication silently uses the wrong
  table entries.
- Eliminating the query variable itself — `variable_elimination`'s elimination loop must skip the
  query variable; summing it out early destroys the very distribution being asked for.
- Forgetting to normalize at the end — variable elimination's final factor is proportional to,
  but not necessarily equal to, the true posterior; always divide by the sum of its own values.
- In Task D, comparing against an enumeration whose variable or evidence set does not exactly
  match the elimination query — both must answer the *same* query over the *same* evidence to be
  a valid cost/correctness comparison.

**Debugging tip:** after each elimination step, print the remaining factors' variable sets; if a
variable meant to be eliminated still appears in some factor afterward, the `sum_out` call for
that step was skipped or applied to the wrong factor.

**Instructor tip:** have students compute the Task B query by hand via enumeration *first*
(reusing the Week-11-style hand calculation) before trusting their variable-elimination result —
agreement to 4 decimal places against an independently hand-computed number is a much stronger
check than agreement against another piece of code that might share the same bug.
