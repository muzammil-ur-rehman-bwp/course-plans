# Week 5: Problem Solving as Search

## Learning Objectives

By the end of this lecture, you should be able to:

1. Formulate a problem as a search problem with states, actions, a transition model, a goal test and a path cost.
2. Implement breadth-first search (BFS) and depth-first search (DFS) against one common problem interface.
3. Compare the two in terms of completeness, optimality, time and memory.
4. Apply both algorithms to a maze and to the classic water jug puzzle.
5. Trace both algorithms by hand on a small graph and confirm the result with code.

## 1. Why Search?

Many AI problems can be framed as finding a sequence of actions that leads from a starting situation to a desired one. Route finding, puzzle solving, game playing and robot motion planning all fit this description. Search is the general machinery for such problems. It is not intelligent in any mysterious sense. It is systematic exploration, and the interesting questions are how to explore so that we find an answer, find a good answer, and do not run out of time or memory.

## 2. Formulating a Search Problem

A search problem is defined by six things.

1. State space: the set of all possible configurations of the world.
2. Initial state: where the agent starts.
3. Actions: `actions(state)` returns the actions that can be taken in a state.
4. Transition model: `result(state, action)` returns the state that follows.
5. Goal test: `is_goal(state)` says whether a state is a goal.
6. Path cost: a numeric cost for a path. Often each step costs 1, so the cost is the number of steps.

The solution is a sequence of actions from the initial state to a goal state. An optimal solution is one with the lowest path cost.

Here is a base class that captures this definition. Every problem in the next three weeks will subclass it.

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
```

The search algorithms only talk to this interface. They do not know whether they are solving a maze or a puzzle. That separation is why a few dozen lines of search code can be reused on many problems.

### 2.1 A first concrete problem: a grid maze

The maze is a list of strings. `#` is a wall, `.` is open, `S` is the start and `G` is the goal. A state is a `(row, col)` tuple, and tuples can go into a set, which we need for the visited record.

```python
class MazeProblem(Problem):
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

    def actions(self, state):
        r, c = state
        result = []
        for name, (dr, dc) in self.MOVES.items():
            nr, nc = r + dr, c + dc
            if (0 <= nr < len(self.grid) and 0 <= nc < len(self.grid[0])
                    and self.grid[nr][nc] != "#"):
                result.append(name)
        return result

    def result(self, state, action):
        dr, dc = self.MOVES[action]
        return (state[0] + dr, state[1] + dc)


maze = [
    "S.#.....",
    ".##.###.",
    "....#...",
    ".####.#.",
    "......#G",
]
problem = MazeProblem(maze)
print(problem.initial, problem.goal)
print(problem.actions(problem.initial))
```

Check that the printed start is `(0, 0)`, the goal is `(4, 7)`, and that from the start the agent can move down or right. A small test like this is worth doing before running any search, because a bug in the problem definition will make every algorithm look broken.

## 3. Breadth-First Search

BFS explores the state space level by level. It first visits every state one step from the start, then every state two steps away, and so on. A first-in first-out queue gives exactly this order.

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

Walk through the code:

1. The frontier holds states that have been discovered but not yet expanded. Each entry also carries the path of actions that led to it.
2. `visited` records states already added, so we never queue the same state twice. Without it, the search could loop forever in a graph that contains cycles.
3. `popleft` removes the oldest entry, which is what makes the search breadth first.
4. If the frontier becomes empty without finding a goal, no solution exists and we return `None`.

Because BFS reaches shallower states before deeper ones, the first time it reaches the goal it has used the fewest possible steps. This holds when every step costs the same.

```python
path = bfs(problem)
print(path, len(path))
```

## 4. Depth-First Search

DFS follows one branch as deep as it can, and only backs up when it reaches a dead end. A last-in first-out stack does this. In Python a plain list used with `append` and `pop` is a stack.

```python
def dfs(problem):
    frontier = [(problem.initial, [])]
    visited = {problem.initial}
    while frontier:
        state, path = frontier.pop()
        if problem.is_goal(state):
            return path
        for action in problem.actions(state):
            next_state = problem.result(state, action)
            if next_state not in visited:
                visited.add(next_state)
                frontier.append((next_state, path + [action]))
    return None
```

The only structural difference from BFS is `pop()` in place of `popleft()`. That one change swaps the exploration order completely.

```python
path_dfs = dfs(problem)
print(path_dfs, len(path_dfs))
```

In this particular maze both paths happen to have 15 steps, because the corridors leave very little choice. DFS only shows its weakness when there is open space, so we try a second maze, an open room with one obstacle.

```python
room = [
    "S.....",
    "......",
    "......",
    "...#..",
    "....G.",
]
room_problem = MazeProblem(room)
print("BFS:", len(bfs(room_problem)), "steps")
print("DFS:", len(dfs(room_problem)), "steps")
```

BFS needs 8 steps, which is the Manhattan distance, so it cannot be beaten. DFS returns a path of 18 steps. It is a valid path, but it wanders across the room.

### 4.1 Seeing the difference

It helps to draw the paths. Here we use the open room.

```python
def draw(grid, path, start):
    cells = [list(row) for row in grid]
    r, c = start
    for a in path[:-1]:
        dr, dc = MazeProblem.MOVES[a]
        r, c = r + dr, c + dc
        cells[r][c] = "*"
    return "\n".join("".join(row) for row in cells)

path_b = bfs(room_problem)
path_d = dfs(room_problem)
print("BFS path, length", len(path_b))
print(draw(room, path_b, room_problem.initial))
print()
print("DFS path, length", len(path_d))
print(draw(room, path_d, room_problem.initial))
```

The DFS route snakes back and forth. That is acceptable if all we need is any solution, and it is not acceptable if the shortest route matters.

## 5. Comparing BFS and DFS

Let `b` be the branching factor (the number of actions per state, roughly), `d` the depth of the shallowest goal, and `m` the maximum depth of the state space.

1. Completeness. Both are complete on a finite state space when a visited set is used. Without a visited set, DFS can run forever along a cycle.
2. Optimality. BFS finds a path with the fewest steps when all steps cost the same. DFS gives no such guarantee.
3. Time. BFS takes about O(b^d). DFS takes O(b^m), which can be much worse if `m` is far greater than `d`, but can also be lucky and finish quickly.
4. Space. BFS keeps a whole level in memory, O(b^d), and this is its real weakness. DFS only keeps the current branch and its siblings, O(bm).

An important practical point is memory. A BFS on a problem with branching factor 10 and a solution at depth 8 might have to hold on the order of a hundred million states. DFS would hold perhaps eighty. When memory is the limit, DFS or a depth-limited variant becomes attractive.

## 6. Worked Example: Water Jugs

This puzzle shows that the same algorithms work on a problem that has nothing to do with grids. We have a 3 litre jug and a 5 litre jug, both empty, and an unlimited water supply. We want exactly 4 litres in one jug.

A state is `(a, b)`, the litres in each jug. The actions are: fill a jug, empty a jug, or pour one into the other until either the source is empty or the target is full.

```python
class WaterJugs(Problem):
    def __init__(self, cap_a=3, cap_b=5, target=4):
        super().__init__((0, 0))
        self.cap_a, self.cap_b, self.target = cap_a, cap_b, target

    def is_goal(self, state):
        return self.target in state

    def actions(self, state):
        return ["fill A", "fill B", "empty A", "empty B", "pour A->B", "pour B->A"]

    def result(self, state, action):
        a, b = state
        if action == "fill A":
            return (self.cap_a, b)
        if action == "fill B":
            return (a, self.cap_b)
        if action == "empty A":
            return (0, b)
        if action == "empty B":
            return (a, 0)
        if action == "pour A->B":
            amount = min(a, self.cap_b - b)
            return (a - amount, b + amount)
        if action == "pour B->A":
            amount = min(b, self.cap_a - a)
            return (a + amount, b - amount)


jugs = WaterJugs()
solution = bfs(jugs)
print(len(solution), "steps:")
state = jugs.initial
print(state)
for act in solution:
    state = jugs.result(state, act)
    print(f"{act:10s} -> {state}")
```

BFS returns the shortest solution, which is six steps for this puzzle. DFS also finds six steps here, which is a coincidence of this small puzzle. Try a bigger one, with jugs of 7 and 11 litres and a target of 6.

```python
big = WaterJugs(7, 11, 6)
print("BFS:", len(bfs(big)), "steps")
print("DFS:", len(dfs(big)), "steps")
```

BFS needs 10 steps and DFS returns a valid solution of 22 steps. Notice that we did not change either algorithm. We only wrote a new `Problem` subclass.

### 6.1 Counting expanded states

To compare algorithms fairly, count how many states each one expands. We add a counter by wrapping the problem.

```python
class Counted:
    """Wraps a problem and counts calls to actions(), i.e., expanded states."""
    def __init__(self, problem):
        self.p = problem
        self.expanded = 0
        self.initial = problem.initial

    def actions(self, s):
        self.expanded += 1
        return self.p.actions(s)

    def result(self, s, a):
        return self.p.result(s, a)

    def is_goal(self, s):
        return self.p.is_goal(s)


for name, algo in [("BFS", bfs), ("DFS", dfs)]:
    wrapped = Counted(MazeProblem(maze))
    p = algo(wrapped)
    print(f"{name}: path length {len(p)}, states expanded {wrapped.expanded}")
```

Week 6 reuses this idea to show how an informed search expands fewer states.

## 7. Hand Tracing

Tracing by hand is the best way to be sure you understand an algorithm, and it is often asked in examinations. Take this graph with five nodes. Edges are undirected, and neighbours are visited in alphabetical order.

```
A: B, C
B: A, D
C: A, D, E
D: B, C, E
E: C, D
```

Start at A and look for E.

BFS trace:

1. Frontier: [A]. Expand A, add B and C. Frontier: [B, C].
2. Expand B. Neighbours A (seen) and D (new). Frontier: [C, D].
3. Expand C. Neighbours A (seen), D (seen), E (new). Frontier: [D, E].
4. Expand D. Nothing new. Frontier: [E].
5. Take E, the goal. Path: A, C, E.

DFS trace (stack, last pushed is first popped):

1. Stack: [A]. Pop A, push B and C. Stack: [B, C].
2. Pop C (the last pushed). Push D and E. Stack: [B, D, E].
3. Pop E, the goal. Path: A, C, E.

Both reach E by the same path in this small case, but they got there in a different order, and DFS never touched B. Now confirm with code, using the `Graph` class idea from Week 2 wrapped as a problem.

```python
class GraphProblem(Problem):
    def __init__(self, adj, start, goal):
        super().__init__(start, goal)
        self.adj = adj

    def actions(self, state):
        return sorted(self.adj[state])      # an action is "go to that neighbour"

    def result(self, state, action):
        return action


adj = {
    "A": ["B", "C"],
    "B": ["A", "D"],
    "C": ["A", "D", "E"],
    "D": ["B", "C", "E"],
    "E": ["C", "D"],
}
gp = GraphProblem(adj, "A", "E")
print("BFS:", bfs(gp))
print("DFS:", dfs(gp))
```

The printed lists are the sequences of nodes visited after the start. The DFS version pushes neighbours in alphabetical order, so it pops the last one first. If your hand trace and the output differ, find where the order of your trace departed from the code.

## 8. In-Class Exercise

Trace BFS and DFS by hand on a different 5 node graph of your own, such as a path with one shortcut, and then check your answer with `GraphProblem`. Then answer these questions.

1. Can you build a graph where DFS finds a much longer path than BFS?
2. Remove the `visited` set from `dfs`, and run it on `GraphProblem`. What happens, and why?
3. In which situation would you prefer DFS despite its lack of optimality?

For the second question, add a safety limit so the program does not hang.

```python
def dfs_no_visited(problem, limit=10_000):
    frontier = [(problem.initial, [])]
    steps = 0
    while frontier and steps < limit:
        steps += 1
        state, path = frontier.pop()
        if problem.is_goal(state):
            return path, steps
        for action in problem.actions(state):
            frontier.append((problem.result(state, action), path + [action]))
    return None, steps

gp_b = GraphProblem(adj, "A", "B")
print(dfs_no_visited(gp_b))
```

The output is `(None, 10000)`: the search used up its whole step limit without finding B. B sits at the bottom of the stack, and the search keeps going around the cycle through C, D and E, so it never gets back to B. With the visited set, the same search ends quickly. The visited set is what makes DFS complete on graphs with cycles.

## 9. Common Mistakes

1. Marking states as visited when they are popped instead of when they are added. This is not wrong, but it lets duplicates sit in the frontier and wastes memory.
2. Using a list with `pop(0)` as a queue. It works, but it costs O(n) per pop. Use `deque`.
3. Using unhashable states, such as lists, in the visited set. Use tuples.
4. Mutating a state in `result` instead of returning a new one.
5. Forgetting that BFS optimality assumes equal step costs. With different costs we need uniform cost search or A*, which is next week.

## 10. Summary

A search problem is described by its states, actions, transitions, goal test and cost. Once a problem is written against that interface, algorithms can be swapped freely. BFS is complete and gives shortest paths for equal step costs, but it needs a lot of memory. DFS needs little memory but can return long paths. Next week we use knowledge about the problem, in the form of heuristics, to search more efficiently.

## 11. Practice Problems

1. Write a `EightPuzzle` problem class, and solve a simple scramble with BFS.
2. Modify `bfs` so that it returns the number of states it expanded as well as the path.
3. Implement depth-limited search, which stops going deeper than a given limit, and then iterative deepening, which calls it with limits 0, 1, 2 and so on. State why iterative deepening is optimal for equal costs while using DFS-like memory.
4. Change the maze so that no path exists, and check that both algorithms return `None`.

## 12. Suggested Reading

1. Russell and Norvig, Artificial Intelligence: A Modern Approach, the chapter on solving problems by searching.
2. Python documentation for `collections.deque`.
