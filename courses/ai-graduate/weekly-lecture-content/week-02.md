# Week 2 — Lecture Content: Advanced Search

## 1. Why A* Is Not Enough
A* is optimal and complete with an admissible heuristic, but it keeps every generated node in
memory (in its frontier and/or explored set). Worst-case node count is O(b^d) for branching
factor b and solution depth d — so A*'s **space** complexity, not its time complexity, is usually
what kills it first on large problems. The algorithms this week trade some time for much better
memory behavior, or trade search order for an exponential speedup.

## 2. Iterative Deepening A* (IDA*)
IDA* repeatedly runs a depth-first search bounded by an f-cost threshold, starting at
f(s₀) = h(s₀) and increasing the threshold on each iteration to the smallest f-value that
exceeded the previous threshold.

**Algorithm:**
```
IDA*(problem):
    threshold = h(s0)
    loop:
        result, next_threshold = DFS-contour(s0, g=0, threshold)
        if result is a solution: return result
        if next_threshold == infinity: return failure
        threshold = next_threshold

DFS-contour(s, g, threshold):
    f = g + h(s)
    if f > threshold: return failure, f
    if is_goal(s): return solution(s), threshold
    min_exceeded = infinity
    for each action a in actions(s):
        s2 = transition(s, a)
        result, t = DFS-contour(s2, g + cost(s,a,s2), threshold)
        if result is a solution: return result, threshold
        min_exceeded = min(min_exceeded, t)
    return failure, min_exceeded
```

**Correctness and complexity.** Each contour (iteration) is a complete depth-first search of
every node with f ≤ threshold, so IDA* is complete and — because the threshold only ever
increases to the minimal f-value that exceeded the previous bound, and depth-first search expands
nodes in a fixed order — the first solution found is optimal, exactly as for A*. Its **space**
complexity is O(d): depth-first search only needs to remember the current path, not every
generated node. Its time complexity can, in the worst case with many distinct f-values, be worse
than A*'s by a constant factor (nodes may be re-expanded across contours), but on problems with
integer costs and few distinct f-values this overhead is small. The practical tradeoff is exactly
"use IDA* when A*'s memory, not its time, is the bottleneck."

```python
def ida_star(start, is_goal, successors, h):
    threshold = h(start)
    while True:
        path = [start]
        result = _dfs_contour(path, 0, threshold, is_goal, successors, h)
        if isinstance(result, list):
            return result
        if result == float("inf"):
            return None
        threshold = result

def _dfs_contour(path, g, threshold, is_goal, successors, h):
    node = path[-1]
    f = g + h(node)
    if f > threshold:
        return f
    if is_goal(node):
        return list(path)
    min_exceeded = float("inf")
    for action, nxt, step_cost in successors(node):
        if nxt in path:
            continue
        path.append(nxt)
        result = _dfs_contour(path, g + step_cost, threshold, is_goal, successors, h)
        if isinstance(result, list):
            return result
        min_exceeded = min(min_exceeded, result)
        path.pop()
    return min_exceeded
```

## 3. Bidirectional Search
Bidirectional search runs two simultaneous searches: forward from s₀ and backward from the goal
(requires a goal state, or a small set of them, and an invertible/known-predecessor transition
model). The two frontiers are checked for intersection after each expansion step. If both
searches expand breadth-first to roughly depth d/2, the node count is O(2·b^(d/2)) instead of
O(b^d) — an exponential improvement, since b^(d/2) ≪ b^d for large d. The catch: backward search
requires the ability to compute predecessors, which is easy in an undirected graph but may be
hard or ill-defined in some problem formulations (e.g., when actions are not easily invertible).

## 4. Simplified Memory-Bounded A* (SMA*)
SMA* behaves like A* but operates under a fixed memory budget. When memory is full and a new node
must be generated, SMA* discards the frontier leaf with the *worst* (highest) f-value, first
backing up that value to its parent so the parent "remembers" that this branch was explored and
was not promising — if all of a parent's children have been forgotten, the parent's f-value is
set to the minimum of its children's backed-up values, which may later cause the parent itself to
be regenerated and re-expanded if it becomes the most promising remaining node. SMA* is complete
and optimal whenever the memory budget is enough to hold at least one solution path; it degrades
gracefully (never crashes from running out of memory) at the cost of potentially re-expanding
forgotten nodes.

## 5. Comparative Summary
| Algorithm | Space | Time (worst case) | Notes |
|---|---|---|---|
| A* | O(b^d) | O(b^d) | Optimal with admissible h; the memory baseline |
| IDA* | O(d) | O(b^d), possible re-expansion overhead | Best when memory, not time, is scarce |
| Bidirectional | O(b^(d/2)) | O(b^(d/2)) | Needs invertible/known-predecessor actions |
| SMA* | O(fixed budget) | Possible re-expansion overhead | Graceful degradation under hard memory limits |

## 6. In-Class/Lab Exercise
Implement IDA* for the 8-puzzle using the Manhattan-distance heuristic; compare its peak memory
usage against a plain A* implementation on the same instances, and report wall-clock time for
both.
