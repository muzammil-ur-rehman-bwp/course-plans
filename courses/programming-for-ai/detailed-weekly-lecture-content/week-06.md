# Week 6: Informed Search, Greedy Best-First and A*

## Learning Objectives

By the end of this lecture, you should be able to:

1. Explain what a heuristic is, and define admissible and consistent heuristics.
2. Implement greedy best-first search and A* with a priority queue.
3. Choose a heuristic for a grid problem and argue whether it is admissible.
4. Measure nodes expanded and path cost for BFS, greedy search and A* on the same problem.
5. Explain why A* is optimal with an admissible heuristic, and why greedy search is not.

## 1. Motivation

Last week's algorithms are blind. BFS and DFS know nothing about where the goal is, so they spread in all directions or plunge down arbitrary branches. If you were walking to a railway station in an unfamiliar city, you would not explore every street at a fixed distance. You would head in the general direction of the station, and correct course when a street turned out to be a dead end.

Informed search captures this. We give the algorithm a function that estimates how far each state is from the goal, and it uses the estimate to decide what to explore next.

Before we write code, a reminder from Week 5. Our `Problem` interface and `MazeProblem` class will be reused here. The code below repeats them in short form so that this file runs on its own.

```python
class Problem:
    def __init__(self, initial, goal=None):
        self.initial = initial
        self.goal = goal

    def actions(self, state):
        raise NotImplementedError

    def result(self, state, action):
        raise NotImplementedError

    def is_goal(self, state):
        return state == self.goal


class GridProblem(Problem):
    """A grid with walls (#), a start S, a goal G, and optional costly cells (digits 2-9)."""
    MOVES = {"U": (-1, 0), "D": (1, 0), "L": (0, -1), "R": (0, 1)}

    def __init__(self, grid):
        self.grid = grid
        start = goal = None
        for r, row in enumerate(grid):
            for c, ch in enumerate(row):
                if ch == "S":
                    start = (r, c)
                elif ch == "G":
                    goal = (r, c)
        super().__init__(start, goal)
        self.expanded = 0

    def actions(self, state):
        self.expanded += 1
        r, c = state
        out = []
        for name, (dr, dc) in self.MOVES.items():
            nr, nc = r + dr, c + dc
            if (0 <= nr < len(self.grid) and 0 <= nc < len(self.grid[0])
                    and self.grid[nr][nc] != "#"):
                out.append(name)
        return out

    def result(self, state, action):
        dr, dc = self.MOVES[action]
        return (state[0] + dr, state[1] + dc)

    def cost_of_entering(self, cell):
        ch = self.grid[cell[0]][cell[1]]
        return int(ch) if ch.isdigit() else 1
```

The counter `expanded` goes up each time `actions` is called, which happens once for every state the algorithm expands. This is our measure of effort.

## 2. Heuristics

A heuristic `h(n)` is an estimate of the cost of the cheapest path from node `n` to a goal. It is a guess, and it does not have to be exact. What matters is how it relates to the true cost, written `h*(n)`.

1. Admissible: `h(n) <= h*(n)` for every `n`. The heuristic never overestimates. Optimism is safe, and pessimism is not. A* needs this property to guarantee optimal answers.
2. Consistent (also called monotonic): `h(n) <= cost(n, n') + h(n')` for every neighbour `n'` of `n`. This is a triangle inequality. Moving to a neighbour cannot reduce the estimate by more than the cost of the move. Every consistent heuristic is admissible, and consistency lets A* avoid expanding a node more than once.

Common examples on a grid with four-way movement and a step cost of 1:

1. Manhattan distance, `|r1 - r2| + |c1 - c2|`. It is admissible, because any path needs at least this many steps. It is also consistent.
2. Euclidean (straight line) distance. It is admissible and consistent too, but it is weaker than Manhattan on a four-way grid, since it is always smaller or equal, so it gives less guidance.
3. The zero heuristic, `h(n) = 0`. It is admissible but useless. With it A* turns into uniform cost search.

A useful rule: among admissible heuristics, the larger one is better, because it is closer to the truth and prunes more.

```python
def manhattan(goal):
    def h(state):
        return abs(state[0] - goal[0]) + abs(state[1] - goal[1])
    return h

def euclidean(goal):
    def h(state):
        return ((state[0] - goal[0]) ** 2 + (state[1] - goal[1]) ** 2) ** 0.5
    return h

h = manhattan((4, 7))
print(h((0, 0)), h((4, 7)), h((2, 3)))   # 11 0 6
```

Notice that `manhattan(goal)` returns a function. This is the Week 1 idea of functions as values, put to use. We can hand different heuristics to the same algorithm.

## 3. Greedy Best-First Search

Greedy best-first search always expands the frontier node with the smallest `h(n)`. It trusts the estimate completely and ignores how much it has already cost to reach the node.

A priority queue provides this behaviour. In Python the `heapq` module implements a binary heap on top of an ordinary list. `heappush` adds an item, and `heappop` removes the smallest. Items are compared as tuples, so the first field is the priority.

```python
import heapq

def greedy_best_first(problem, h):
    frontier = [(h(problem.initial), problem.initial, [])]
    visited = {problem.initial}
    while frontier:
        _, state, path = heapq.heappop(frontier)
        if problem.is_goal(state):
            return path
        for action in problem.actions(state):
            next_state = problem.result(state, action)
            if next_state not in visited:
                visited.add(next_state)
                heapq.heappush(frontier, (h(next_state), next_state, path + [action]))
    return None
```

The state sits in the tuple as a tie-breaker, so that if two entries have the same `h`, Python compares states (tuples of numbers) and never has to compare the paths. If your states cannot be compared with `<`, add a running counter in that position instead.

Greedy search is fast when the heuristic points the right way, but it can be led astray. If a wall stands between the current position and the goal, the cells next to the wall look attractive because their estimate is small, and the search pushes into them even when they lead into a dead end. We will see this happen in a concrete grid in section 5.

## 4. A* Search

A* orders the frontier by `f(n) = g(n) + h(n)`, where `g(n)` is the exact cost already spent to reach `n`, and `h(n)` is the estimated cost still to go. So `f(n)` estimates the cost of the best complete path through `n`.

```python
def a_star(problem, h, step_cost=lambda s, a: 1):
    frontier = [(h(problem.initial), 0, problem.initial, [])]
    best_g = {problem.initial: 0}
    while frontier:
        f, g, state, path = heapq.heappop(frontier)
        if g > best_g.get(state, float("inf")):
            continue                       # stale entry, a cheaper route was found
        if problem.is_goal(state):
            return path
        for action in problem.actions(state):
            next_state = problem.result(state, action)
            new_g = g + step_cost(state, action)
            if next_state not in best_g or new_g < best_g[next_state]:
                best_g[next_state] = new_g
                heapq.heappush(frontier, (new_g + h(next_state), new_g, next_state, path + [action]))
    return None
```

Important details:

1. `best_g` records the cheapest known cost to reach each state. If we later find a cheaper route to a state, we add it again to the frontier with the better cost.
2. The line that skips stale entries discards older, more expensive copies of a state still sitting in the heap. Python's heap cannot remove an item from the middle, so we simply ignore it when it comes out.
3. The goal test is applied when a node is popped, not when it is pushed. This is what preserves optimality. A goal first discovered by an expensive route might still be in the frontier when a cheaper route exists.

### 4.1 Why A* is optimal

Suppose `h` is admissible. When A* pops a goal node with cost `g`, every other frontier node has `f >= g`. Any better route to the goal would pass through some frontier node `n` on that route, and for that node `f(n) = g(n) + h(n) <= g(n) + h*(n)`, the true cost of the best route through `n`, which would be less than `g`. That contradicts `f(n) >= g`. Therefore no better route exists. This short argument is worth being able to reproduce. With a consistent heuristic the result holds even when closed nodes are not reopened.

### 4.2 The effect of the heuristic

1. If `h = 0`, A* behaves like uniform cost search. It is optimal, but explores widely.
2. If `h = h*` exactly, A* walks straight to the goal.
3. If `h` overestimates, A* may be faster, but it may also return a non-optimal path. The guarantee is gone.

## 5. Comparing the Strategies

We now put the algorithms side by side on the same grid. We need a BFS that works with our counter, so we reuse the one from Week 5.

```python
from collections import deque

def bfs(problem):
    frontier = deque([(problem.initial, [])])
    visited = {problem.initial}
    while frontier:
        state, path = frontier.popleft()
        if problem.is_goal(state):
            return path
        for action in problem.actions(state):
            next_state = problem.result(state, action)
            if next_state not in visited:
                visited.add(next_state)
                frontier.append((next_state, path + [action]))
    return None

```

We use an open grid with a few walls.

```python
grid = [
    "S.........",
    "..........",
    "....####..",
    "....#.....",
    "....#..G..",
    "....#.....",
    "..........",
]

def run(name, algo, *args):
    p = GridProblem(grid)
    path = algo(p, *args) if args else algo(p)
    print(f"{name:22s} path length {len(path):2d}   states expanded {p.expanded:3d}")
    return path

goal = GridProblem(grid).goal
run("BFS", bfs)
run("Greedy (Manhattan)", greedy_best_first, manhattan(goal))
run("A* (Manhattan)", a_star, manhattan(goal))
run("A* (zero heuristic)", a_star, lambda s: 0)
```

Typical output for this grid:

1. BFS and A* with the zero heuristic both find a shortest path (13 steps) and expand 54 and 52 states.
2. A* with Manhattan finds a path of the same length and expands only 38 states.
3. Greedy expands only 14 states. On this friendly grid it also finds a shortest path, so it looks perfect. That is the danger. Try a grid with a few more obstacles.

```python
tricky = [
    "S.......#",
    "...#.....",
    "....#...#",
    "#...##.#.",
    "........#",
    ".....###.",
    "........G",
]

for name, algo, args in [
    ("BFS", bfs, ()),
    ("Greedy", greedy_best_first, (manhattan((6, 8)),)),
    ("A* (Manhattan)", a_star, (manhattan((6, 8)),)),
]:
    p = GridProblem(tricky)
    path = algo(p, *args)
    print(f"{name:16s} path length {len(path):2d}   states expanded {p.expanded:3d}")
```

Here BFS and A* both return 14 steps, and A* expands fewer states (45 against 48 for BFS). Greedy expands only 22 states, but its path has 20 steps, six more than necessary. It rushed toward the goal along a route that the walls forced to detour. That is the trade: greedy search is cheap and sometimes wrong, and A* is the careful option.

### 5.1 Costs that differ

A* really shows its value when steps have different costs, because BFS stops being optimal. We make some cells expensive by writing digits for them in the grid. Entering a cell marked `5` costs 5.

```python
costly = [
    "S.....",
    ".5555.",
    ".5..5.",
    ".5.G5.",
    "......",
]
p = GridProblem(costly)
goal = p.goal

def step_cost(state, action):
    nxt = p.result(state, action)
    return p.cost_of_entering(nxt)

def path_cost(path):
    s, total = p.initial, 0
    for a in path:
        s = p.result(s, a)
        total += p.cost_of_entering(s)
    return total
```

The grid also contains digit cells, and our `GridProblem` treats them as open. The goal cell is `G`, which costs 1 to enter. Compare BFS (which ignores costs) with A*.

```python
bfs_path = bfs(GridProblem(costly))
astar_path = a_star(GridProblem(costly), manhattan(goal), step_cost)

print("BFS   steps", len(bfs_path), "cost", path_cost(bfs_path))
print("A*    steps", len(astar_path), "cost", path_cost(astar_path))
```

BFS minimizes the number of steps, so it plunges through the expensive cells. A* minimizes total cost, and it will happily take a longer route around them. The Manhattan heuristic is still admissible here because every step costs at least 1.

## 6. Worked Example: Reconstructing and Drawing the Path

```python
def draw(grid, path, start):
    cells = [list(r) for r in grid]
    r, c = start
    for a in path[:-1]:
        dr, dc = GridProblem.MOVES[a]
        r, c = r + dr, c + dc
        cells[r][c] = "*"
    return "\n".join("".join(row) for row in cells)

p = GridProblem(grid)
path = a_star(p, manhattan(p.goal))
print(draw(grid, path, p.initial))
```

Draw the greedy path too and compare. On a map with a dead end facing the goal, the picture shows exactly where greedy search wasted effort.

## 7. In-Class Exercise

For a given grid with obstacles, propose a heuristic, argue whether it is admissible, and run A* with it. Compare the path length and the number of states expanded with BFS.

A starting point: a heuristic that is not admissible, to see what goes wrong.

```python
def inflated(goal, factor=3):
    base = manhattan(goal)
    return lambda s: factor * base(s)

p = GridProblem(tricky)
good_problem = GridProblem(tricky)
good = a_star(good_problem, manhattan(p.goal))
bad_problem = GridProblem(tricky)
bad = a_star(bad_problem, inflated(p.goal, 5))
print("admissible    :", len(good), "steps, states expanded", good_problem.expanded)
print("overestimating:", len(bad), "steps, states expanded", bad_problem.expanded)
```

With the factor of 5, the heuristic overestimates. A* expands fewer states (28 against 45) but returns an 18 step path where 14 steps were possible. This is the trade that an inadmissible heuristic makes: it buys speed with optimality.

Questions:

1. Is the straight line distance admissible on a four-way grid? Is it consistent?
2. What happens to the heuristic if diagonal moves are allowed? Is Manhattan still admissible then?
3. Does a larger multiplier always give a worse path, or only sometimes? Try different grids.

## 8. Common Mistakes

1. Testing for the goal when pushing, not when popping, in A*.
2. Using a heuristic that overestimates and still claiming an optimal result.
3. Forgetting that greedy search ignores `g(n)`.
4. Pushing tuples whose later fields cannot be compared, which raises a `TypeError` on ties.
5. Using a visited set in A* that blocks cheaper routes to a state found later. The `best_g` dictionary is the right tool.

## 9. Summary

A heuristic adds knowledge to search. Greedy best-first uses only the estimate and is quick but unreliable. A* adds the real cost so far and, with an admissible heuristic, is guaranteed to return an optimal solution. The quality of the heuristic decides how much work A* saves, and designing good heuristics is a creative part of AI practice. Next week we study problems where the path does not matter, only the final assignment.

## 10. Practice Problems

1. Prove that Manhattan distance is consistent on a four-way grid with unit step costs.
2. Extend `GridProblem` to allow diagonal moves with cost 1.4, and choose an admissible heuristic.
3. Implement uniform cost search, and show that it equals A* with `h = 0`.
4. For the eight puzzle, compare the number of misplaced tiles and the sum of Manhattan distances as heuristics. Which one is admissible, and which is better?

## 11. Suggested Reading

1. Russell and Norvig, the chapter on informed search.
2. Amit Patel's online notes on A* for games, which include helpful visualizations.
