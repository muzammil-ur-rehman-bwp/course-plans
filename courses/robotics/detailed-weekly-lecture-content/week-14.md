# Week 14: Path Planning II, Sampling-Based Planning

## Learning Objectives

By the end of this lecture, you should be able to:

1. Explain why grid search becomes impractical as the dimension of the configuration space grows.
2. Implement RRT, and say what is and is not guaranteed about the paths it finds.
3. Improve a raw RRT path with goal bias and shortcut smoothing.
4. Describe the potential field method, and show how it gets stuck in a local minimum.
5. Compare RRT with grid-based A* on the same map, using path length and running time.

## 1. Limits of Grid Search

Grid based A* works very well for a mobile robot in a plane, where the configuration space has two dimensions. Its cost depends on the number of cells, and that number grows exponentially with the number of dimensions. Suppose we divide each dimension into 100 steps. A planar position needs 100 times 100, which is 10,000 cells. A 6-joint arm needs 100 to the power 6, a million million cells, which is impossible to store, let alone search.

```python
cells_per_dim = 100
for dims in [2, 3, 4, 6, 7]:
    print(f"{dims} dimensions: {cells_per_dim ** dims:.2e} cells")
```

Sampling-based planners avoid building the grid. They sample random configurations, test them for collisions, and connect them into a graph or tree that grows through the free space. They give up two guarantees that grid search had. They are not optimal, since the path found depends on the random samples, and they are only probabilistically complete: if a path exists, the chance of finding it tends to one as the number of samples grows, but there is no way to show that no path exists. In exchange they scale to many dimensions and to continuous space.

## 2. Rapidly-Exploring Random Trees

RRT grows a tree from the start configuration. Each iteration does four things.

1. Sample a random point in the space.
2. Find the node of the tree that is nearest to the sample.
3. Extend from that node a small step toward the sample.
4. If the step does not hit an obstacle, add the new point to the tree, with the nearest node as its parent.

When a new node comes within a tolerance of the goal, the path is read off by following parents back to the start.

The tree has a useful bias: nodes at the edge of the explored region are the nearest to samples that fall outside it, and so they are the ones that get extended. The tree rushes toward unexplored space. That is what "rapidly exploring" means.

### 2.1 The test environment

We need a world to plan in. This one is a square room of 10 by 10 metres, with a dividing wall that has a gap, and two smaller obstacles. The obstacles are rectangles. A point is blocked if it lies in a rectangle or outside the room. A motion between two points is checked for collision by testing points along the segment at a fine spacing.

```python
import math
import time
import numpy as np

rects = [(4.5, 0.0, 5.5, 7.0),      # lower part of a dividing wall
         (4.5, 8.5, 5.5, 10.0),     # upper part, leaving a gap between y = 7 and y = 8.5
         (1.5, 3.0, 3.0, 4.0),
         (6.5, 5.0, 8.5, 6.0)]

def point_blocked(x, y):
    if not (0 <= x <= 10 and 0 <= y <= 10):
        return True
    return any(x0 <= x <= x1 and y0 <= y <= y1 for x0, y0, x1, y1 in rects)

def obstacle_check(a, b, spacing=0.05):
    """True if the straight motion from a to b hits an obstacle."""
    d = math.hypot(b[0] - a[0], b[1] - a[1])
    n = max(1, int(d / spacing))
    for i in range(n + 1):
        t = i / n
        if point_blocked(a[0] + t * (b[0] - a[0]), a[1] + t * (b[1] - a[1])):
            return True
    return False

start, goal = (1.0, 1.0), (9.0, 9.0)
print("straight line blocked:", obstacle_check(start, goal))
print("start free:", not point_blocked(*start), "  goal free:", not point_blocked(*goal))
```

Checking points along a segment at a spacing is simple, and it can miss an obstacle thinner than the spacing. Real planners use exact geometric tests. Here our obstacles are 1 m thick, so the spacing of 5 cm is safe.

### 2.2 The algorithm

The original lecture code is clear, but it searches the whole list of nodes linearly for each sample, and it has a few traps, which we fix in the version below. It stores the nodes in a NumPy array so the nearest node can be found quickly. It takes a random generator for repeatable experiments. It stops the extension at the sample if the sample is closer than one step. It checks that the final connection to the goal is collision free. It avoids a subtle bug in which the goal could become its own parent, and path reconstruction would loop for ever.

```python
def reconstruct_path(tree, node):
    path = [node]
    while tree[node] is not None:
        node = tree[node]
        path.append(node)
    return path[::-1]

def rrt(start, goal, obstacle_check, bounds, step_size=0.5, max_iter=1000, goal_tol=0.5,
        goal_bias=0.0, rng=None):
    rng = rng or np.random.default_rng()
    tree = {start: None}                      # node -> parent
    keys = [start]
    pts = np.zeros((max_iter + 2, 2))
    pts[0] = start
    n = 1
    for iteration in range(max_iter):
        if rng.random() < goal_bias:
            sample = np.array(goal, dtype=float)
        else:
            sample = np.array([rng.uniform(*bounds[0]), rng.uniform(*bounds[1])])
        d2 = ((pts[:n] - sample) ** 2).sum(axis=1)
        i = int(d2.argmin())
        nearest = keys[i]
        length = math.sqrt(d2[i])
        if length == 0:
            continue
        new_node = tuple(pts[i] + (sample - pts[i]) / length * min(step_size, length))

        if not obstacle_check(nearest, new_node):
            tree[new_node] = nearest
            keys.append(new_node)
            pts[n] = new_node
            n += 1
            if (math.hypot(new_node[0] - goal[0], new_node[1] - goal[1]) < goal_tol
                    and not obstacle_check(new_node, goal)):
                if new_node != goal:
                    tree[goal] = new_node
                return reconstruct_path(tree, goal), iteration + 1, n
    return None, max_iter, n                  # no path found within max_iter

def path_length(path):
    return sum(math.hypot(a[0] - b[0], a[1] - b[1]) for a, b in zip(path, path[1:]))

rng = np.random.default_rng(1)
path, iterations, nodes = rrt(start, goal, obstacle_check, ((0, 10), (0, 10)),
                              step_size=0.5, max_iter=3000, rng=rng)
print("found a path:", path is not None)
print(f"iterations used: {iterations}, tree nodes: {nodes}, path points: {len(path)}, path length: {path_length(path):.2f} m")
print(f"straight-line distance would be {math.hypot(8, 8):.2f} m")
```

Each time you change the seed, you get a different tree and a different path. That is the first thing to notice about RRT: it is random.

Also notice the `bounds` argument. The sampler needs to know where to sample. The nearest-node step is the main cost for large trees, and real implementations use special data structures, such as k-d trees, to make it fast.

### 2.3 Looking at the path

```python
import matplotlib.pyplot as plt
from matplotlib.patches import Rectangle

def draw_world(ax):
    for x0, y0, x1, y1 in rects:
        ax.add_patch(Rectangle((x0, y0), x1 - x0, y1 - y0, color="gray"))
    ax.set_xlim(0, 10)
    ax.set_ylim(0, 10)
    ax.set_aspect("equal")

fig, ax = plt.subplots()
draw_world(ax)
xs, ys = zip(*path)
ax.plot(xs, ys, color="black", linewidth=2)
ax.scatter(*start, color="black", marker="o")
ax.scatter(*goal, color="black", marker="*", s=100)
ax.set_title("A path found by RRT")
plt.show()
```

The path found is jagged and wanders. It is valid, but it is not what a person would draw, and it is much longer than needed. This is typical of raw RRT paths, and we deal with it in section 4.

## 3. How Much Does It Cost?

We run the planner for 20 different seeds with several settings, and record the success rate and the path length. The success rate depends on the iteration limit, and the step size has a strong influence.

```python
def experiment(step_size, max_iter, runs=20):
    lengths, iterations = [], []
    for seed in range(runs):
        p, it, _ = rrt(start, goal, obstacle_check, ((0, 10), (0, 10)),
                       step_size=step_size, max_iter=max_iter, rng=np.random.default_rng(seed))
        if p is not None:
            lengths.append(path_length(p))
            iterations.append(it)
    return len(lengths), (np.mean(lengths) if lengths else float("nan")), (np.mean(iterations) if iterations else float("nan"))

print(f"{'step':>5} {'max_iter':>9} {'success':>8} {'mean length':>12} {'mean iterations':>16}")
for step_size in [1.0, 0.5, 0.25]:
    for max_iter in [200, 500, 3000]:
        ok, mean_len, mean_it = experiment(step_size, max_iter)
        print(f"{step_size:5.2f} {max_iter:9d} {ok:5d}/20 {mean_len:12.2f} {mean_it:16.1f}")
```

Some things to read from the table.

1. With a small iteration budget, many runs fail. For `step = 0.25` and 200 iterations, none succeed, since the tree cannot even reach across the room with steps of a quarter metre in 200 iterations.
2. Success goes up with the budget, and for large budgets it approaches 100 percent. That is probabilistic completeness in action.
3. The mean path length hardly changes with the budget. RRT stops at the first path it finds, so additional iterations do not make that path better. They only make it more likely that a path is found. This is an important point for the in-class exercise.
4. Smaller steps need more iterations to cover the same distance, but each iteration is cheap.

## 4. Making the Path Better

### 4.1 Goal bias

A pure random sampler spends most of its effort exploring parts of the space that are far from the goal. In goal biasing, a small fraction of the samples, 5 to 20 percent, are the goal itself. The tree then makes frequent attempts to reach out toward it.

```python
print(f"{'goal bias':>9}   mean iterations to find a path (30 seeds)")
for bias in [0.0, 0.05, 0.2, 0.5]:
    its = []
    for seed in range(30):
        p, it, _ = rrt(start, goal, obstacle_check, ((0, 10), (0, 10)), 0.5, 3000,
                       goal_bias=bias, rng=np.random.default_rng(seed))
        its.append(it if p is not None else 3000)
    print(f"{bias:9.2f}   {np.mean(its):8.1f}")
```

A little bias helps, since it reduces the iterations needed, from about 276 with none to about 150 with 20 percent. Too much bias, 50 percent, helps less, because the tree keeps butting against the dividing wall instead of exploring toward the gap. A pure goal-seeker would get stuck behind the wall, which is the problem of local minima that we meet again in section 5.

### 4.2 Shortcut smoothing

The raw path has many unnecessary turns. A very effective and simple improvement is shortcutting. Pick two random points on the path. If the straight segment between them is free of obstacles, replace everything between them by that segment. Repeat many times.

```python
def shortcut(path, check, tries=300, rng=None):
    rng = rng or np.random.default_rng(0)
    path = list(path)
    for _ in range(tries):
        if len(path) < 3:
            break
        i, j = sorted(rng.choice(len(path), size=2, replace=False))
        if j - i < 2:
            continue
        if not check(path[i], path[j]):
            path = path[:i + 1] + path[j:]
    return path

raw_lengths, smooth_lengths = [], []
for seed in range(20):
    p, _, _ = rrt(start, goal, obstacle_check, ((0, 10), (0, 10)), 0.5, 3000, rng=np.random.default_rng(seed))
    raw_lengths.append(path_length(p))
    smooth_lengths.append(path_length(shortcut(p, obstacle_check, 300, np.random.default_rng(seed))))
print(f"mean raw path length:       {np.mean(raw_lengths):.2f} m")
print(f"mean after shortcutting:    {np.mean(smooth_lengths):.2f} m")
```

Shortcutting cuts the paths by about a fifth, from around 16.2 metres to around 12.9. It costs almost nothing, and it is used in almost every practical sampling-based planner. More sophisticated variants, such as RRT*, keep improving the path as more samples come in, and converge to the optimal path, at a higher cost per iteration. We do not implement them here.

## 5. Local Obstacle Avoidance: Potential Fields

Sampling planners think globally. A much simpler idea works locally, and is used for fast reactive behaviour. In the potential field method the goal attracts the robot, as if with a spring, and each obstacle repels it, with a force that grows very strongly as the robot gets close. The robot moves in the direction of the total force.

The attraction is `F_att = ka * (goal - position)`. The repulsion, for an obstacle at distance `rho` from the robot's surface, acts only within a range `rho0`, and pushes away from the obstacle with a magnitude of `kr * (1/rho - 1/rho0) / rho^2`.

```python
def potential_field(start, goal, circles, steps=3000, ka=1.0, kr=2.0, rho0=1.5, lr=0.05):
    p = np.array(start, dtype=float)
    goal = np.array(goal, dtype=float)
    for k in range(steps):
        force = ka * (goal - p)
        for cx, cy, r in circles:
            away = p - np.array([cx, cy])
            dist_centre = np.linalg.norm(away)
            rho = dist_centre - r
            if rho < rho0:
                rho = max(rho, 1e-3)
                force += kr * (1 / rho - 1 / rho0) / rho ** 2 * away / dist_centre
        if np.linalg.norm(force) < 1e-3:
            return p, k, "stuck at a point where the forces cancel"
        p = p + lr * force / max(1.0, np.linalg.norm(force))
        if np.linalg.norm(goal - p) < 0.1:
            return p, k, "reached the goal"
    return p, steps, "ran out of steps"

# a single round obstacle between the start and the goal
obstacle = [(5.0, 5.0, 1.0)]
for y0 in [5.0, 5.4]:
    pos, steps, outcome = potential_field((1.0, y0), (9.0, 5.0), obstacle)
    print(f"start at y = {y0}: {outcome}, after {steps} steps, at ({pos[0]:.2f}, {pos[1]:.2f})")
```

When the start is exactly in line with the obstacle and the goal, the attractive and repulsive forces point along the same line and balance at a point in front of the obstacle. The robot stops there, at about `(3.4, 5.0)`, without ever reaching the goal. This is a local minimum of the potential, a place where the net force is zero though it is not the goal. A start that is only slightly off the line, 0.4 m, slides around the side and arrives.

A concave obstacle makes the problem much worse. The U-shaped trap is the classic.

```python
u_trap = ([(5.0, y, 0.4) for y in np.arange(3.0, 7.1, 0.4)]
          + [(5.0 - x, 3.0, 0.4) for x in np.arange(0.4, 2.5, 0.4)]
          + [(5.0 - x, 7.0, 0.4) for x in np.arange(0.4, 2.5, 0.4)])
pos, steps, outcome = potential_field((4.0, 5.0), (9.0, 5.0), u_trap)
print(f"inside a U-shaped trap: {outcome}, at ({pos[0]:.2f}, {pos[1]:.2f}) after {steps} steps")
```

The robot enters the U, is pulled toward the goal on the other side of the closed back of the U, and stops against the back of it. It cannot back out, because backing out would mean moving away from the goal. This is analogous to local optima in hill climbing, which you met in the classical AI course. Potential fields are quick and simple, and useful for local avoidance, but they should not be the only planner. They are better combined with a global planner, which supplies waypoints that guide the robot out of such traps.

## 6. Comparing RRT and A*

To compare them fairly, we rasterize the same room into a grid with 10 cm cells, and run A* with eight-connected moves on it.

```python
import heapq

def a_star_cost(grid, start_cell, goal_cell):
    rows, cols = grid.shape
    moves = [(-1, 0), (1, 0), (0, -1), (0, 1), (-1, -1), (-1, 1), (1, -1), (1, 1)]
    def h(a):
        dx, dy = abs(a[0] - goal_cell[0]), abs(a[1] - goal_cell[1])
        return (dx + dy) + (math.sqrt(2) - 2) * min(dx, dy)
    best = {start_cell: 0.0}
    frontier = [(h(start_cell), 0.0, start_cell)]
    while frontier:
        f, g, cur = heapq.heappop(frontier)
        if g > best[cur]:
            continue
        if cur == goal_cell:
            return g
        for dr, dc in moves:
            nxt = (cur[0] + dr, cur[1] + dc)
            if not (0 <= nxt[0] < rows and 0 <= nxt[1] < cols) or grid[nxt]:
                continue
            new_g = g + math.hypot(dr, dc)
            if new_g < best.get(nxt, float("inf")):
                best[nxt] = new_g
                heapq.heappush(frontier, (new_g + h(nxt), new_g, nxt))
    return None

res = 0.1
grid = np.array([[1 if point_blocked((c + 0.5) * res, (r + 0.5) * res) else 0 for c in range(100)] for r in range(100)])

t0 = time.perf_counter()
astar_len = a_star_cost(grid, (10, 10), (90, 90)) * res
astar_time = time.perf_counter() - t0

t0 = time.perf_counter()
p, _, _ = rrt(start, goal, obstacle_check, ((0, 10), (0, 10)), 0.5, 3000, rng=np.random.default_rng(0))
rrt_time = time.perf_counter() - t0
smooth = shortcut(p, obstacle_check, 300, np.random.default_rng(0))

print(f"A* on a 100 x 100 grid:   length {astar_len:5.2f} m   time {astar_time * 1000:6.1f} ms")
print(f"RRT (raw):                length {path_length(p):5.2f} m   time {rrt_time * 1000:6.1f} ms")
print(f"RRT with shortcutting:    length {path_length(smooth):5.2f} m")
```

On this small two-dimensional problem, A* finds the shortest path, about 12.8 metres, quickly. The raw RRT path is about a fifth longer for this seed, and over many seeds about a quarter longer, and shortcutting almost closes the gap. So on a planar map A* is a perfectly good choice, and RRT has no advantage. The picture changes with the dimension. For a 6-joint arm, the grid has a million million cells, and a planner like RRT is the only practical option. The aim of this comparison is not that one wins, but that they suit different problems.

A summary of the comparison:

1. Space. A* works on a discretised space, and RRT works in the continuous space.
2. Optimality. A* is optimal for the grid, and RRT is not optimal.
3. Completeness. A* is complete for the grid, and RRT is only probabilistically complete.
4. Growth with dimension. The cost of A* grows exponentially, and the cost of RRT grows only mildly.
5. Path quality. A* gives the shortest path on the grid, and RRT gives a jagged path until it is smoothed.

## 7. In-Class Exercise

Run the RRT implementation with an increasing `max_iter` and a decreasing `step_size`, and observe the tradeoff between path quality, closer to the path of A*, and computation time.

Use the experiment function above, and extend it with the time taken. Then add shortcutting, and see how much of the gap to A* it closes.

```python
print(f"{'step':>5} {'max_iter':>9} {'success':>8} {'raw length':>11} {'smoothed':>9} {'ms per run':>11}")
for step_size in [1.0, 0.5, 0.25]:
    for max_iter in [500, 3000]:
        raw, smooth, ok = [], [], 0
        t0 = time.perf_counter()
        for seed in range(15):
            p, _, _ = rrt(start, goal, obstacle_check, ((0, 10), (0, 10)), step_size, max_iter,
                          rng=np.random.default_rng(seed))
            if p is not None:
                ok += 1
                raw.append(path_length(p))
                smooth.append(path_length(shortcut(p, obstacle_check, 300, np.random.default_rng(seed))))
        ms = (time.perf_counter() - t0) / 15 * 1000
        print(f"{step_size:5.2f} {max_iter:9d} {ok:5d}/15 {np.mean(raw) if raw else float('nan'):11.2f} "
              f"{np.mean(smooth) if smooth else float('nan'):9.2f} {ms:11.1f}")
print(f"A* reference length: {astar_len:.2f} m")
```

Write a short conclusion that answers these questions.

1. Did a larger `max_iter` improve the quality of the path, or only the success rate? Why?
2. What does a smaller step size buy, and what does it cost?
3. How close does shortcutting get to the A* length, and at what cost?

Further questions:

1. Why does RRT's lack of a guarantee matter for a robot that must be safe?
2. How would the planner change if the robot were a rectangle that could rotate? (What is the dimension of the configuration space then?)
3. How might you use RRT and potential fields together?

## 8. Common Mistakes

1. Treating the first RRT path as final, without smoothing.
2. Using a step size that is larger than the thinnest obstacle or the narrowest gap.
3. Checking only the new node for collisions, and not the whole segment from its parent.
4. Having `start` or `goal` inside an obstacle.
5. Forgetting that RRT is random, and reporting a single run instead of the average over several seeds.
6. Reading "no path found" as "no path exists".
7. Relying on potential fields alone in a cluttered space.
8. Letting path reconstruction loop for ever through a parent cycle, for example by adding the goal to the tree twice.

## 9. Summary

Grid search cannot cope with high-dimensional configuration spaces, because the grid grows exponentially. RRT grows a tree toward randomly sampled points, and finds paths in continuous spaces without building a grid, but it gives paths that are neither optimal nor smooth, and it can only promise to find a path if given enough samples. Goal bias and shortcutting are simple, effective improvements. Potential fields are a quick local method that can be trapped by local minima. For a planar robot, A* remains an excellent choice, and the sampling methods earn their place in higher dimensions. Next week we bring the planner, the controller and the estimator together.

## 10. Practice Problems

1. Add a second gap to the dividing wall, and measure how often RRT chooses each gap over 50 seeds.
2. Implement RRT-Connect, which grows two trees, one from the start and one from the goal, and tries to join them. Compare the number of iterations with plain RRT.
3. Plan for a 2-link arm in joint space, with the obstacles checked in the workspace by forward kinematics from Week 3. How many dimensions is the search space?
4. Combine RRT and the PID controller of Week 10: let the simulated differential-drive robot of Week 3 follow the waypoints of a smoothed RRT path.

## 11. Suggested Reading

1. LaValle, "Rapidly-Exploring Random Trees: A New Tool for Path Planning", 1998, and his book Planning Algorithms, free online.
2. Karaman and Frazzoli, "Sampling-based algorithms for optimal motion planning", 2011, which introduces RRT*.
3. Khatib, "Real-Time Obstacle Avoidance for Manipulators and Mobile Robots", 1986, for potential fields.
