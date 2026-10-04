# Week 12 — Lecture Content: Probabilistic Reasoning and Bayesian Networks in Depth

## 1. Recap: Bayesian Networks
A Bayesian network represents a joint distribution over variables as a DAG plus one conditional
probability table (CPT) per node, giving `P(node | parents)`. The graph structure encodes
conditional independence: a node is independent of its non-descendants given its parents.

## 2. Beyond Enumeration: Why Variable Elimination?
Inference by enumeration (summing the full joint, expressed as a product of CPT entries, over
every combination of hidden variables) is always correct but recomputes the same sub-products
repeatedly. **Variable elimination** avoids this: it eliminates hidden variables one at a time,
each time multiplying together just the factors that mention that variable and then summing it
out of the resulting product — never forming the full joint distribution explicitly.

## 3. Factors
A **factor** is a function from a tuple of variable assignments to a number (not necessarily a
probability after intermediate steps). Each CPT is one initial factor.

```python
class Factor:
    def __init__(self, variables, table):
        self.variables = tuple(variables)      # e.g. ("Burglary", "Earthquake")
        self.table = table                     # dict: tuple of values -> probability

    def multiply(self, other):
        combined_vars = list(self.variables) + [v for v in other.variables if v not in self.variables]
        new_table = {}
        for assignment in itertools.product([True, False], repeat=len(combined_vars)):
            full = dict(zip(combined_vars, assignment))
            key_self = tuple(full[v] for v in self.variables)
            key_other = tuple(full[v] for v in other.variables)
            new_table[assignment] = self.table[key_self] * other.table[key_other]
        return Factor(combined_vars, new_table)

    def sum_out(self, variable):
        if variable not in self.variables:
            return self
        remaining = [v for v in self.variables if v != variable]
        idx = self.variables.index(variable)
        new_table = {}
        for assignment, value in self.table.items():
            key = assignment[:idx] + assignment[idx + 1:]
            new_table[key] = new_table.get(key, 0.0) + value
        return Factor(remaining, new_table)
```

(`import itertools` at the top of the module; the `multiply` method above enumerates the joint
domain of the combined variables, which is simple and correct for the small networks this course
uses — a production implementation would iterate only over each factor's own table entries.)

## 4. The Variable Elimination Algorithm
To compute `P(query | evidence)`: start with every CPT as a factor, restrict factors to the
observed evidence values, repeatedly pick a hidden variable, multiply together every factor that
mentions it, sum it out of the product, and replace those factors with the result. When no hidden
variables remain, multiply what is left and normalize.

```python
def restrict(factor, variable, value):
    if variable not in factor.variables:
        return factor
    idx = factor.variables.index(variable)
    remaining = tuple(v for v in factor.variables if v != variable)
    new_table = {
        assignment[:idx] + assignment[idx + 1:]: val
        for assignment, val in factor.table.items() if assignment[idx] == value
    }
    return Factor(remaining, new_table)

def variable_elimination(factors, query_var, evidence, elimination_order):
    factors = list(factors)
    for var, val in evidence.items():
        factors = [restrict(f, var, val) for f in factors]
    for var in elimination_order:
        if var == query_var:
            continue
        relevant = [f for f in factors if var in f.variables]
        if not relevant:
            continue
        factors = [f for f in factors if f not in relevant]
        product = relevant[0]
        for f in relevant[1:]:
            product = product.multiply(f)
        factors.append(product.sum_out(var))
    result = factors[0]
    for f in factors[1:]:
        result = result.multiply(f)
    total = sum(result.table.values())
    return {assignment[0]: val / total for assignment, val in result.table.items()}
```

## 5. Worked Example
Network: `Burglary`, `Earthquake` → `Alarm` (as in the Week 11-of-intro-ai-style motivating
example, now solved by elimination instead of full enumeration). Query `P(Burglary | Alarm=True)`:
1. Initial factors: `f_B = P(Burglary)`, `f_E = P(Earthquake)`, `f_A = P(Alarm | Burglary,
   Earthquake)`.
2. Restrict `f_A` to `Alarm = True`, giving a factor over `(Burglary, Earthquake)` only.
3. Eliminate `Earthquake`: multiply `f_E` and the restricted `f_A` (both mention `Earthquake`),
   then sum out `Earthquake`, producing a factor over `Burglary` alone.
4. Multiply this result by `f_B`, normalize over `Burglary`'s two values — this gives the same
   answer as Week-11-style enumeration, but only ever multiplied factors over at most two
   variables at a time, never the full 3-variable joint.

## 6. Elimination Ordering (Brief)
The order in which hidden variables are eliminated affects the size of the intermediate factors
created (and therefore the total work done), though not the final answer — any valid order gives
the same correct result. Choosing a good ordering in general is itself a hard combinatorial
problem (briefly mentioned, not solved algorithmically in this course); for the small networks
used here, any reasonable ordering is efficient enough.

## 7. In-Class Exercise
For a 4-node chain network `A → B → C → D`, write out which factors must be multiplied and which
variable summed out at each step of eliminating `B` and `C` to compute `P(D | A)`, without
executing the code.
