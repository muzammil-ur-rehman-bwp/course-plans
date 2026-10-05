# Week 7: Constraint Satisfaction Problems and Local Search

## Learning Objectives

By the end of this lecture, you should be able to:

1. Formulate a problem as a CSP with variables, domains and constraints.
2. Implement backtracking search, and improve it with forward checking.
3. Explain hill climbing, its failure modes, and how random restarts help.
4. Implement simulated annealing and describe the role of the temperature schedule.
5. Solve N-Queens with both families of method and compare runtime and solution quality.

## 1. A Different Kind of Problem

In the last two weeks we searched for a path, a sequence of actions. Many problems are not like that. When you build a timetable, colour a map or place eight queens on a chessboard, nobody cares about the order in which you made the decisions. Only the final assignment matters, and it either satisfies all the rules or does not.

These are constraint satisfaction problems, or CSPs. Treating them as CSPs gives us tools that are much more efficient than blind search, since the structure of the constraints is visible to the algorithm.

## 2. CSP Formulation

A CSP consists of three parts.

1. Variables: `X1, ..., Xn`.
2. Domains: `D1, ..., Dn`, the possible values for each variable.
3. Constraints: restrictions on which combinations of values are allowed.

A solution is a complete assignment of values to variables that violates no constraint.

### 2.1 Example: map colouring

Variables are the regions of a map. The domain of each is {red, green, blue}. The constraint is that neighbouring regions must have different colours. Here is the map of Australian states and territories, a standard textbook example.

```python
variables = ["WA", "NT", "SA", "Q", "NSW", "V", "T"]
domains = {v: ["red", "green", "blue"] for v in variables}

neighbors = {
    "WA": ["NT", "SA"],
    "NT": ["WA", "SA", "Q"],
    "SA": ["WA", "NT", "Q", "NSW", "V"],
    "Q": ["NT", "SA", "NSW"],
    "NSW": ["Q", "SA", "V"],
    "V": ["SA", "NSW"],
    "T": [],
}

def map_constraints(assignment):
    """True if no two assigned neighbours share a colour."""
    for a, color in assignment.items():
        for b in neighbors[a]:
            if b in assignment and assignment[b] == color:
                return False
    return True
```

Note that the constraint function accepts a partial assignment. It only checks pairs where both regions have been coloured. This is what allows us to detect failure early, before the assignment is complete.

### 2.2 Example: N-Queens

Place N queens on an N by N board so that no two attack each other. One neat formulation is to use one variable per column, whose value is the row of the queen in that column. This formulation already guarantees one queen per column. The constraints are that no two queens share a row, and none share a diagonal.

```python
def queens_ok(assignment):
    """assignment maps column -> row. True if no two queens attack each other."""
    cols = list(assignment)
    for i in range(len(cols)):
        for j in range(i + 1, len(cols)):
            c1, c2 = cols[i], cols[j]
            r1, r2 = assignment[c1], assignment[c2]
            if r1 == r2 or abs(r1 - r2) == abs(c1 - c2):
                return False
    return True

print(queens_ok({0: 1, 1: 3}))    # True
print(queens_ok({0: 1, 1: 2}))    # False: diagonal neighbours
print(queens_ok({0: 2, 1: 2}))    # False: same row
```

## 3. Backtracking Search

Backtracking is depth-first search over partial assignments. It picks an unassigned variable, tries each value in its domain, checks the constraints, and recurses. If a value leads to a dead end, it undoes the choice and tries the next one.

```python
def backtracking(assignment, variables, domains, constraints):
    if len(assignment) == len(variables):
        return assignment
    var = next(v for v in variables if v not in assignment)
    for value in domains[var]:
        assignment[var] = value
        if constraints(assignment):
            result = backtracking(assignment, variables, domains, constraints)
            if result is not None:
                return result
        del assignment[var]
    return None

solution = backtracking({}, variables, domains, map_constraints)
print(solution)
```

You should see a valid colouring, with Tasmania receiving any colour. The key difference from naive generate and test is that we check constraints after every assignment. A bad choice early on is discovered immediately, and the whole subtree below it is skipped.

Let us apply the same function to N-Queens, and count how many assignments it tries.

```python
import time

def solve_queens_bt(n):
    variables = list(range(n))
    domains = {c: list(range(n)) for c in variables}
    calls = [0]

    def checked(assignment):
        calls[0] += 1
        return queens_ok(assignment)

    t0 = time.perf_counter()
    sol = backtracking({}, variables, domains, checked)
    return sol, calls[0], time.perf_counter() - t0

for n in [4, 6, 8, 10]:
    sol, calls, secs = solve_queens_bt(n)
    print(f"n={n:2d}  constraint checks={calls:6d}  time={secs:.4f}s  solution={[sol[c] for c in range(n)]}")
```

### 3.1 Improving backtracking: forward checking

Plain backtracking only discovers that a choice was bad when it tries to assign a variable that has run out of legal values. Forward checking prunes earlier. After each assignment, we remove values from the domains of the other variables that are now impossible. If any domain becomes empty, we fail at once, without descending further.

```python
import copy

def forward_checking(assignment, variables, domains, conflict):
    """conflict(var1, val1, var2, val2) returns True if the pair is not allowed."""
    if len(assignment) == len(variables):
        return assignment
    var = next(v for v in variables if v not in assignment)
    for value in domains[var]:
        new_domains = copy.deepcopy(domains)
        new_domains[var] = [value]
        ok = True
        for other in variables:
            if other in assignment or other == var:
                continue
            new_domains[other] = [w for w in new_domains[other]
                                  if not conflict(var, value, other, w)]
            if not new_domains[other]:
                ok = False
                break
        if ok:
            assignment[var] = value
            result = forward_checking(assignment, variables, new_domains, conflict)
            if result is not None:
                return result
            del assignment[var]
    return None


def queen_conflict(c1, r1, c2, r2):
    return r1 == r2 or abs(r1 - r2) == abs(c1 - c2)

def solve_queens_fc(n):
    variables = list(range(n))
    domains = {c: list(range(n)) for c in variables}
    t0 = time.perf_counter()
    sol = forward_checking({}, variables, domains, queen_conflict)
    return sol, time.perf_counter() - t0

for n in [8, 10, 12]:
    sol, secs = solve_queens_fc(n)
    print(f"forward checking n={n:2d} time={secs:.4f}s {[sol[c] for c in range(n)]}")
```

Compare the times with plain backtracking for `n = 10` and larger. Two further ideas are worth knowing by name, though we will not implement them: choosing the most constrained variable next (the minimum remaining values heuristic), and trying the least constraining value first.

## 4. Local Search

Backtracking builds a solution piece by piece. Local search takes a completely different view. It starts with a complete assignment, which will probably violate some constraints, and repeatedly makes small changes that improve it. It remembers only the current state, so memory use is tiny. For problems with enormous state spaces, such as N-Queens with N equal to a thousand, this is often the only practical approach.

We need two ingredients: a cost function, such as the number of violated constraints, and a neighbourhood, the set of states reachable by one small change.

For N-Queens, a state is a list `state[c]` giving the row of the queen in column `c`. The cost is the number of attacking pairs, and a neighbour is obtained by moving one queen within its column.

```python
import random

def conflicts(state):
    n = len(state)
    count = 0
    for i in range(n):
        for j in range(i + 1, n):
            if state[i] == state[j] or abs(state[i] - state[j]) == j - i:
                count += 1
    return count

def queen_neighbors(state):
    n = len(state)
    result = []
    for col in range(n):
        for row in range(n):
            if row != state[col]:
                nxt = list(state)
                nxt[col] = row
                result.append(tuple(nxt))
    return result

print(conflicts((0, 1, 2, 3)))        # 6, all on one diagonal
print(conflicts((1, 3, 0, 2)))        # 0, a valid 4-queens solution
```

### 4.1 Hill climbing

Hill climbing always moves to the best neighbour, as long as it is strictly better than the current state.

```python
def hill_climbing(initial_state, neighbors_fn, cost_fn, max_steps=1000):
    current = initial_state
    for _ in range(max_steps):
        neighbors = neighbors_fn(current)
        best_neighbor = min(neighbors, key=cost_fn)
        if cost_fn(best_neighbor) >= cost_fn(current):
            break  # local optimum reached
        current = best_neighbor
    return current

random.seed(0)
n = 8
start = tuple(random.randrange(n) for _ in range(n))
end = hill_climbing(start, queen_neighbors, conflicts)
print("start cost:", conflicts(start), " end cost:", conflicts(end), end)
```

Run this several times with different seeds. Sometimes the result has cost 0, and sometimes it stops at cost 1 or 2. In those cases the algorithm has reached a local optimum: every neighbour is worse, yet the state is not a solution. There are two other awkward landscapes. A plateau is a flat region with no improving direction, and a ridge is a narrow path upwards that single moves cannot follow.

```python
def success_rate(trials=200, n=8):
    wins = 0
    for _ in range(trials):
        s = tuple(random.randrange(n) for _ in range(n))
        if conflicts(hill_climbing(s, queen_neighbors, conflicts)) == 0:
            wins += 1
    return wins / trials

random.seed(1)
print("hill climbing success rate on 8-queens:", success_rate())
```

Plain hill climbing solves only a fraction of random 8-queens instances, around fifteen percent. A simple remedy is random restarts: when stuck, start again from a fresh random state.

```python
def random_restart(n, restarts=100):
    for attempt in range(1, restarts + 1):
        s = tuple(random.randrange(n) for _ in range(n))
        s = hill_climbing(s, queen_neighbors, conflicts)
        if conflicts(s) == 0:
            return s, attempt
    return None, restarts

random.seed(2)
sol, tries = random_restart(8)
print(sol, "found after", tries, "restarts")
```

With restarts the method becomes reliable. A solution is typically found after about a dozen restarts, since each restart succeeds roughly one time in six or seven.

## 5. Simulated Annealing

Another way to escape a local optimum is to allow some moves that make things worse. Simulated annealing is named after the metallurgical process in which a metal is heated and then cooled slowly, so that its atoms settle into a low energy arrangement.

The rule: pick a random neighbour. If it is better, accept it. If it is worse by an amount `delta`, accept it anyway with probability `exp(delta / T)`, where `T` is the temperature and `delta` is negative. When `T` is high, bad moves are accepted often and the search wanders widely. As `T` falls, the search becomes more and more like pure hill climbing.

```python
import math

def simulated_annealing(initial_state, neighbors_fn, cost_fn, schedule):
    current = initial_state
    for t, temperature in enumerate(schedule):
        if temperature == 0:
            return current
        next_state = random.choice(neighbors_fn(current))
        delta = cost_fn(current) - cost_fn(next_state)  # positive = improvement
        if delta > 0 or random.random() < math.exp(delta / temperature):
            current = next_state
    return current
```

A schedule is any sequence of temperatures. A geometric one is common.

```python
def geometric_schedule(t0=2.0, rate=0.999, steps=20000):
    t = t0
    for _ in range(steps):
        yield t
        t *= rate
    while True:
        yield 0
```

Because the schedule is an infinite generator, the function returns once it sees a temperature of 0.

```python
random.seed(3)
n = 8
start = tuple(random.randrange(n) for _ in range(n))
t0 = time.perf_counter()
result = simulated_annealing(start, queen_neighbors, conflicts, geometric_schedule())
print("annealing:", result, "conflicts:", conflicts(result), f"time {time.perf_counter() - t0:.2f}s")
```

A few things to notice. The algorithm does not stop when it finds cost 0, since this implementation runs through the whole schedule. For a clean version, add a check `if cost_fn(current) == 0: return current` inside the loop. Also, annealing can wander away from a solution it has found, but the low temperature at the end makes that unlikely.

## 6. Worked Example: Comparing the Methods on N-Queens

Here we compare all three families on the same task, for a moderately large board. Backtracking with forward checking is complete, so it will always find a solution if one exists. The local methods are incomplete but need little memory.

```python
def annealing_queens(n, seed):
    random.seed(seed)
    start = tuple(random.randrange(n) for _ in range(n))

    def neighbors(state):
        # cheaper neighbourhood: move one random queen to a random new row
        col = random.randrange(n)
        row = random.randrange(n)
        nxt = list(state)
        nxt[col] = row
        return [tuple(nxt)]

    current = start
    t = 1.5
    for step in range(200000):
        if conflicts(current) == 0:
            return current, step
        t = max(t * 0.9995, 0.01)
        cand = neighbors(current)[0]
        delta = conflicts(current) - conflicts(cand)
        if delta > 0 or random.random() < math.exp(delta / t):
            current = cand
    return None, 200000

for n in [8, 12, 16]:
    t0 = time.perf_counter()
    sol, steps = annealing_queens(n, seed=n)
    elapsed = time.perf_counter() - t0
    ok = sol is not None and conflicts(sol) == 0
    print(f"annealing n={n:2d}  solved={ok}  steps={steps:6d}  time={elapsed:.2f}s")
```

The neighbourhood here is a single random move rather than the full list of all moves. This is a common practical choice, since scanning every neighbour would dominate the running time.

Typical conclusions:

1. Backtracking with forward checking is dependable and, on the boards we tried (up to 12), it is faster than annealing by an order of magnitude. Do not assume the fancier method wins.
2. Annealing needs more steps and uses randomness, so results vary from run to run. Its advantage appears on very large boards, where a bad early choice can trap a systematic search in a huge dead subtree, while local search needs only the memory for one board. Local search methods are known to place a million queens, which backtracking cannot.
3. Hill climbing is fastest per step but needs restarts.

## 7. In-Class Exercise

Model N-Queens as a CSP, solve it with backtracking and forward checking, then solve the same problem with simulated annealing. Compare runtime and solution quality.

Use this timing harness as a starting point. It measures both approaches for several board sizes, using the functions defined earlier in this file.

```python
def compare(sizes):
    print(f"{'n':>3} {'FC (s)':>9} {'SA (s)':>9} {'SA solved':>10}")
    for n in sizes:
        _, t_fc = solve_queens_fc(n)
        t0 = time.perf_counter()
        sol, _ = annealing_queens(n, seed=1)
        t_sa = time.perf_counter() - t0
        solved = sol is not None and conflicts(sol) == 0
        print(f"{n:3d} {t_fc:9.4f} {t_sa:9.4f} {str(solved):>10}")

compare([6, 8, 10, 12])
```

Questions:

1. On our runs forward checking is faster at every size listed. Try sizes such as 20, 30 and 40. Does the picture change? Note that forward checking may get slow or very slow at some sizes, depending on how the first choices turn out.
2. Run annealing with ten different seeds for the same `n`. Does it always succeed? What does that say about completeness?
3. Which method would you choose for a timetable with hard constraints that must never be violated, and why?

## 8. Common Mistakes

1. Forgetting to undo an assignment (`del assignment[var]`) after a failed branch.
2. Writing a constraint check that assumes a complete assignment, which then crashes on a partial one.
3. Using a temperature schedule that cools too fast, so the algorithm behaves like hill climbing and is stuck.
4. Using a schedule that cools too slowly, wasting time.
5. Forgetting to set a random seed when reporting experimental results, which makes them impossible to reproduce.
6. Treating a failed run of a local search as proof that no solution exists. It is not.

## 9. Summary

A CSP separates the description of a problem from the method used to solve it. Backtracking with forward checking is a systematic, complete method that exploits the constraints. Local search methods keep one complete state and improve it, which scales to huge problems but gives no guarantee. Hill climbing is quick but gets stuck, and random restarts or simulated annealing are standard ways to escape.

## 10. Practice Problems

1. Write a sudoku solver as a CSP with 81 variables and use forward checking.
2. Add the minimum remaining values heuristic to `forward_checking` by choosing the unassigned variable with the smallest domain.
3. Apply simulated annealing to the map colouring problem, with cost equal to the number of adjacent regions that share a colour.
4. Experiment with the starting temperature and cooling rate on 16-queens. Plot the success rate against the cooling rate.

## 11. Suggested Reading

1. Russell and Norvig, the chapters on constraint satisfaction problems and on local search.
2. Kirkpatrick, Gelatt and Vecchi, "Optimization by Simulated Annealing", Science, 1983.
