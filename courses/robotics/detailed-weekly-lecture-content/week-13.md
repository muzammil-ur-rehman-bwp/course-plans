# Week 13: Path Planning I, Grid-Based Planning

## Learning Objectives

By the end of this lecture, you should be able to:

1. Explain configuration space, and why we can shrink the robot to a point by growing the obstacles.
2. Convert between world coordinates and grid cells.
3. Implement A* on an occupancy grid, with a heuristic that is admissible for the allowed moves.
4. Compare heuristics, and four-connected with eight-connected movement, by path length and cells expanded.
5. Visualise a planned path, and check a planner against a simpler method that is known to be correct.

## 1. Configuration Space

A planner has to decide where the robot can go without hitting anything. The robot has a size and a shape, but it is easier to plan for a point. The configuration space, or C-space, makes this possible. It is the space of all configurations of the robot, that is all the parameters needed to describe where it is. For a mobile robot in the plane, the simplest configuration is the position `(x, y)`, and sometimes also the heading. For a 2-link arm it is the pair of joint angles. Obstacles in the physical world become forbidden regions in C-space.

For a round robot of radius `r`, there is an easy way of building C-space. The robot collides with an obstacle whenever its centre comes within `r` of it. So we grow every obstacle by `r`, and then the robot can be treated as a single point. This operation is called inflating the obstacles.

In this course we approximate C-space by an occupancy grid, built from sensors in Week 9, and treat free cells as the space where the point robot may move.

```python
import numpy as np
from scipy.ndimage import binary_dilation

def inflate(grid, radius_cells):
    """Grow every obstacle (value 1) by radius_cells, using a round structuring element."""
    r = int(radius_cells)
    yy, xx = np.mgrid[-r:r + 1, -r:r + 1]
    disc = (xx ** 2 + yy ** 2) <= r ** 2
    return binary_dilation(grid == 1, structure=disc).astype(int)

grid = np.zeros((12, 12), dtype=int)
grid[5, 3:9] = 1                       # a wall
inflated = inflate(grid, 1)
print("original obstacle cells:", int(grid.sum()), "  after inflating by one cell:", int(inflated.sum()))
print(inflated[3:8, 2:10])
```

The inflated wall is three cells thick, and no path for a point can pass closer than one cell to the real wall.

### 1.1 Grid and world coordinates

A planner works in cell indices, and a robot works in metres. We need two small conversion functions, and a convention. Here the world origin is at the corner of the grid, the cell `(row, col)` covers world `y` in `[row * res, (row + 1) * res)` and `x` in `[col * res, (col + 1) * res)`.

```python
def world_to_cell(x, y, resolution):
    return int(y // resolution), int(x // resolution)          # (row, col)

def cell_to_world(row, col, resolution):
    return (col + 0.5) * resolution, (row + 0.5) * resolution  # the centre of the cell

res = 0.1
print(world_to_cell(0.37, 0.52, res))        # (5, 3)
print(tuple(round(v, 2) for v in cell_to_world(5, 3, res)))   # (0.35, 0.55)
```

Notice that a round trip does not return the original point, since the cell stores the position only to within one cell. Also notice the order: a cell is `(row, col)`, which is `(y, x)`. Mixing the two orders is the commonest bug in grid planners.

## 2. Grid-Based Search for Navigation

This is the A* of the classical AI course, now applied to navigation. Each free cell is a state, moving to an adjacent free cell is an action, and the cost is the distance travelled. The original lecture code is a compact start.

```python
import heapq

def a_star_grid(grid, start, goal):
    def heuristic(a, b):
        return abs(a[0]-b[0]) + abs(a[1]-b[1])  # Manhattan distance

    neighbors = [(-1,0), (1,0), (0,-1), (0,1)]
    frontier = [(heuristic(start, goal), 0, start, [start])]
    visited = {start}

    while frontier:
        f, g, current, path = heapq.heappop(frontier)
        if current == goal:
            return path
        for dx, dy in neighbors:
            nxt = (current[0]+dx, current[1]+dy)
            if (0 <= nxt[0] < grid.shape[0] and 0 <= nxt[1] < grid.shape[1]
                    and grid[nxt] == 0 and nxt not in visited):
                visited.add(nxt)
                new_g = g + 1
                heapq.heappush(frontier, (new_g + heuristic(nxt, goal), new_g, nxt, path + [nxt]))
    return None
```

This version has a subtle weakness. It marks a cell visited at the moment it is first reached, and never reconsiders it. In A* a cell can be reached first by a worse route, and a better route found later would be ignored, so the answer may not be optimal. It also stores a whole path inside every frontier entry, which is wasteful. A more careful version records the best known cost to each cell, and stores parents, from which the path is rebuilt at the end. It also accepts the heuristic and the type of movement as arguments, which we need to compare them.

```python
import math

MOVES_4 = [(-1, 0), (1, 0), (0, -1), (0, 1)]
MOVES_8 = MOVES_4 + [(-1, -1), (-1, 1), (1, -1), (1, 1)]

def manhattan(a, b):
    return abs(a[0] - b[0]) + abs(a[1] - b[1])

def euclidean(a, b):
    return math.hypot(a[0] - b[0], a[1] - b[1])

def octile(a, b):
    dx, dy = abs(a[0] - b[0]), abs(a[1] - b[1])
    return (dx + dy) + (math.sqrt(2) - 2) * min(dx, dy)

def a_star(grid, start, goal, heuristic=manhattan, moves=MOVES_4):
    rows, cols = grid.shape
    best_g = {start: 0.0}
    parent = {start: None}
    frontier = [(heuristic(start, goal), 0.0, start)]
    expanded = 0
    while frontier:
        f, g, current = heapq.heappop(frontier)
        if g > best_g.get(current, float("inf")):
            continue                                     # a stale entry: a cheaper route was found
        expanded += 1
        if current == goal:
            path = []
            while current is not None:
                path.append(current)
                current = parent[current]
            return path[::-1], g, expanded
        for dr, dc in moves:
            nxt = (current[0] + dr, current[1] + dc)
            if not (0 <= nxt[0] < rows and 0 <= nxt[1] < cols) or grid[nxt] != 0:
                continue
            if dr != 0 and dc != 0 and (grid[current[0] + dr, current[1]] != 0 or grid[current[0], current[1] + dc] != 0):
                continue                                 # do not cut the corner of an obstacle
            new_g = g + math.hypot(dr, dc)
            if new_g < best_g.get(nxt, float("inf")):
                best_g[nxt] = new_g
                parent[nxt] = current
                heapq.heappush(frontier, (new_g + heuristic(nxt, goal), new_g, nxt))
    return None, float("inf"), expanded
```

The cost of a diagonal move is the square root of 2, the real distance, and the code refuses to cut a corner through the gap between two diagonally touching obstacles, which a physical robot could not do. The function returns the path, its cost and the number of cells expanded, so that we can compare.

## 3. Path Cost and Heuristic

`g(n)` is the cost travelled so far, the number of cells or the distance, and `h(n)` is an estimate of the cost to the goal. As in the classical formulation, `h` must be admissible, which means never overestimating the true remaining cost, for the path to be optimal. Admissible depends on the moves that are allowed.

1. With four-connected moves and unit cost, Manhattan distance is the exact cost on an empty grid, so it is admissible and the strongest of the simple choices.
2. Euclidean distance is also admissible, since a straight line is never longer than any path, but it is weaker, because it underestimates more, so A* expands more cells.
3. With eight-connected moves, diagonal steps cost 1.41 for a gain of 1 in both coordinates. Manhattan distance then overestimates, since it counts a diagonal as two, so it is not admissible. The octile distance, which uses diagonals when it can, is exact on an empty grid.
4. The zero heuristic is admissible, and turns A* into Dijkstra's algorithm.

We check these claims on a grid with some obstacles. To trust the planner, we also compare it with a breadth-first search, which is known to be optimal for unit costs on a four-connected grid.

```python
from collections import deque

def bfs_length(grid, start, goal):
    rows, cols = grid.shape
    dist = {start: 0}
    queue = deque([start])
    while queue:
        cur = queue.popleft()
        if cur == goal:
            return dist[cur]
        for dr, dc in MOVES_4:
            nxt = (cur[0] + dr, cur[1] + dc)
            if 0 <= nxt[0] < rows and 0 <= nxt[1] < cols and grid[nxt] == 0 and nxt not in dist:
                dist[nxt] = dist[cur] + 1
                queue.append(nxt)
    return None

rng = np.random.default_rng(0)
mismatches = 0
tested = 0
for _ in range(300):
    g = (rng.random((20, 20)) < 0.25).astype(int)
    g[0, 0] = g[19, 19] = 0
    expected = bfs_length(g, (0, 0), (19, 19))
    path, cost, _ = a_star(g, (0, 0), (19, 19), manhattan, MOVES_4)
    if expected is None:
        if path is not None:
            mismatches += 1
        continue
    tested += 1
    if path is None or abs(cost - expected) > 1e-9:
        mismatches += 1
print(f"checked {tested} random solvable grids, mismatches with BFS: {mismatches}")
```

The count of mismatches should be zero. This kind of test, comparing a clever algorithm with a simple one on many random inputs, is a good habit. It catches the subtle faults that a single example misses.

## 4. Comparing Heuristics

Now a fixed map, so that the comparison is repeatable.

```python
MAP = [
    "....................",
    "....................",
    "...########.........",
    "...#................",
    "...#.......#####....",
    "...#.......#........",
    "...#...#####........",
    "...#........########",
    "...##########.......",
    "....................",
    "....................",
    "....................",
]
grid = np.array([[1 if ch == "#" else 0 for ch in row] for row in MAP])
start, goal = (0, 0), (11, 19)

print(f"{'moves':8s} {'heuristic':12s} {'path cost':>9s} {'expanded':>9s}")
for moves_name, moves, heuristics in [
    ("4-conn", MOVES_4, [("zero", lambda a, b: 0.0), ("euclidean", euclidean), ("manhattan", manhattan)]),
    ("8-conn", MOVES_8, [("zero", lambda a, b: 0.0), ("euclidean", euclidean), ("manhattan", manhattan), ("octile", octile)]),
]:
    for h_name, h in heuristics:
        path, cost, expanded = a_star(grid, start, goal, h, moves)
        print(f"{moves_name:8s} {h_name:12s} {cost:9.2f} {expanded:9d}")
```

Read the table. With four-connected moves, all three heuristics return the same cost, since they are all admissible, and the number of cells expanded goes down as the heuristic gets stronger: zero, then Euclidean, then Manhattan. With eight-connected moves, the zero, Euclidean and octile heuristics agree on the cost, and the octile one expands the fewest of the three admissible choices. Manhattan expands far fewer cells than any of them, only 27, but it is not admissible here, and on some maps it returns a longer path than necessary. This map does not show that, since Manhattan happens to find the optimal path here, so look at the experiment below, which uses random maps.

```python
rng = np.random.default_rng(5)
worse = 0
total = 0
for _ in range(200):
    g = (rng.random((25, 25)) < 0.2).astype(int)
    g[0, 0] = g[24, 24] = 0
    p1, c_oct, _ = a_star(g, (0, 0), (24, 24), octile, MOVES_8)
    p2, c_man, _ = a_star(g, (0, 0), (24, 24), manhattan, MOVES_8)
    if p1 is None:
        continue
    total += 1
    if c_man > c_oct + 1e-9:
        worse += 1
print(f"on {total} random maps with 8-connected moves, Manhattan returned a longer path than octile on {worse}")
```

The count is far from zero. On the 170 maps that had a solution, Manhattan returned a longer path than the octile heuristic on 97 of them, which is more than half. Using an inadmissible heuristic really does cost path quality, as the theory says, and the speed gained is not worth it unless an approximately good path will do.

## 5. Visualising the Planned Path

```python
import matplotlib.pyplot as plt

def show(grid, path, title, start, goal):
    plt.imshow(grid, cmap="gray_r")
    rows, cols = zip(*path)
    plt.plot(cols, rows, color="black", linewidth=1.5)
    plt.scatter([start[1]], [start[0]], marker="o", color="gray", s=60, label="start")
    plt.scatter([goal[1]], [goal[0]], marker="*", color="gray", s=100, label="goal")
    plt.title(title)
    plt.legend(loc="lower left")
    plt.show()

path4, cost4, _ = a_star(grid, start, goal, manhattan, MOVES_4)
path8, cost8, _ = a_star(grid, start, goal, octile, MOVES_8)
show(grid, path4, f"4-connected A*, cost {cost4:.1f}", start, goal)
show(grid, path8, f"8-connected A*, cost {cost8:.1f}", start, goal)
print("4-connected path cost:", round(cost4, 2), " 8-connected path cost:", round(cost8, 2))
```

The `imshow` function draws the array with row 0 at the top, so the picture matches the printed map. The plotted path uses columns as x, and rows as y. The eight-connected path is shorter, as diagonals are allowed, and it also looks more natural. Neither is smooth, and a real robot has to convert the cell by cell path into smooth motion, typically by simplifying it to a few waypoints and following them with the controller of Week 10.

### 5.1 Inflation matters

The planned path hugs obstacles, since the shortest path usually touches corners. A real robot with a size would collide. We plan on the inflated grid instead.

```python
robot_radius_cells = 1
safe_grid = inflate(grid, robot_radius_cells)
safe_grid[start] = safe_grid[goal] = 0

plain_path, plain_cost, _ = a_star(grid, start, goal, octile, MOVES_8)
safe_path, safe_cost, _ = a_star(safe_grid, start, goal, octile, MOVES_8)
print("without inflation: cost", round(plain_cost, 2) if plain_path else None)
print("with inflation:    cost", round(safe_cost, 2) if safe_path else "no path")

def min_clearance(grid, path):
    obstacles = np.argwhere(grid == 1)
    pts = np.array(path)
    return float(np.min(np.linalg.norm(pts[:, None, :] - obstacles[None, :, :], axis=2)))

print("closest approach to an obstacle (cells), without inflation:", round(min_clearance(grid, plain_path), 2))
print("closest approach to an obstacle (cells), with inflation:   ", round(min_clearance(grid, safe_path), 2))
```

The inflated plan is a little longer, 28.24 against 27.66, but it keeps the robot's centre at least two cells from every obstacle, where the plain plan passed right next to them, at a distance of one cell. In a narrow corridor the inflation may close the passage, and then no path is found. That is the correct result for a robot that is too big to pass.

### 5.2 Turning a path into waypoints

A cell by cell path has more points than a controller needs. We keep only the cells at which the direction changes.

```python
def waypoints(path):
    if len(path) < 3:
        return list(path)
    keep = [path[0]]
    for prev, cur, nxt in zip(path, path[1:], path[2:]):
        d1 = (cur[0] - prev[0], cur[1] - prev[1])
        d2 = (nxt[0] - cur[0], nxt[1] - cur[1])
        if d1 != d2:
            keep.append(cur)
    keep.append(path[-1])
    return keep

wp = waypoints(path8)
print(f"{len(path8)} cells reduced to {len(wp)} waypoints:", wp)
```

## 6. In-Class Exercise

Plan a path between two given points on a provided occupancy grid using the A* function, and rerun with Euclidean distance instead of Manhattan distance as the heuristic, comparing the paths.

Use the map from section 4, with the start `(0, 0)` and the goal `(11, 19)`. Run both heuristics with four-connected moves.

```python
for name, h in [("manhattan", manhattan), ("euclidean", euclidean)]:
    path, cost, expanded = a_star(grid, start, goal, h, MOVES_4)
    print(f"{name:10s} cost {cost:5.1f}   cells expanded {expanded:3d}   same path as manhattan: {path == a_star(grid, start, goal, manhattan, MOVES_4)[0]}")
```

Compare the printed lengths, which should be equal, with the number of cells expanded. The paths may differ in the details when there are several shortest paths, since the heuristics break ties differently. Write down which explored fewer cells, and why: the Manhattan heuristic is closer to the true cost, and so it guides the search more strongly.

Questions:

1. Is Euclidean distance admissible for four-connected moves? Is the path it returns optimal?
2. Why do the two heuristics find paths of the same cost but a different number of expansions?
3. Multiply the Manhattan heuristic by 3 and plot the result. What do you lose, and what do you gain?

## 7. Common Mistakes

1. Mixing up `(row, col)` and `(x, y)`, and indexing the grid with the wrong one.
2. Planning for the point robot on an uninflated grid.
3. Using Manhattan distance with diagonal moves, which is not admissible.
4. Using a visited set that blocks cheaper routes, in a planner with non-uniform costs.
5. Allowing diagonal moves between two touching obstacles.
6. Forgetting that the grid is a snapshot, so the plan must be reconsidered when the map changes.
7. Giving the planner a start or goal inside an obstacle.

## 8. Summary

C-space turns obstacle avoidance into a search in which the robot is a point, and inflating obstacles accounts for the robot's size. An occupancy grid gives a discrete version of that space, on which A* applies unchanged from the classical AI course. The heuristic must be admissible for the moves allowed. Stronger admissible heuristics expand fewer cells without changing the answer, and an inadmissible one trades quality for speed. Testing against a simpler, trusted algorithm is a cheap and effective check. Next week we look at what to do when grids become impractical.

## 9. Practice Problems

1. Add a cost map, so that some cells are expensive, such as rough ground, and check that A* avoids them when it is worth it.
2. Implement Dijkstra's algorithm by calling `a_star` with the zero heuristic, and verify that it expands the most cells.
3. Change `a_star` to keep a count of how many times each cell is expanded, and show that with a consistent heuristic no cell is expanded twice.
4. Extend the planner so that the robot may only turn by at most 45 degrees between steps, which needs the heading as part of the state.

## 10. Suggested Reading

1. Russell and Norvig, Artificial Intelligence: A Modern Approach, the chapter on informed search.
2. Amit Patel, "Introduction to A*", Red Blob Games, an interactive tutorial.
3. LaValle, Planning Algorithms, the chapter on the configuration space, free online.
