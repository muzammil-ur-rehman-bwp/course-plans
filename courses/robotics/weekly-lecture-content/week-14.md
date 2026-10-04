# Week 14 — Lecture Content: Path Planning II — Sampling-Based Planning

## 1. Limits of Grid Search
Grid-based A* works well in 2D, but its cost grows rapidly as the configuration space's
dimensionality increases (e.g., planning for a multi-joint arm, where C-space has one dimension
per joint) — exhaustively discretizing a high-dimensional space becomes computationally
infeasible. Sampling-based methods trade completeness guarantees for scalability.

## 2. Rapidly-Exploring Random Trees (RRT)
RRT grows a tree from the start configuration by repeatedly: (1) sampling a random point in the
space, (2) finding the nearest existing tree node, (3) extending the tree a small step toward
the sample, (4) adding the new node if the step doesn't hit an obstacle.
```python
import numpy as np

def rrt(start, goal, obstacle_check, bounds, step_size=0.5, max_iter=1000, goal_tol=0.5):
    tree = {start: None}  # node -> parent
    nodes = [start]

    for _ in range(max_iter):
        sample = (np.random.uniform(*bounds[0]), np.random.uniform(*bounds[1]))
        nearest = min(nodes, key=lambda n: np.hypot(n[0]-sample[0], n[1]-sample[1]))
        direction = np.array(sample) - np.array(nearest)
        length = np.linalg.norm(direction)
        if length == 0:
            continue
        new_node = tuple(np.array(nearest) + direction / length * step_size)

        if not obstacle_check(nearest, new_node):
            tree[new_node] = nearest
            nodes.append(new_node)
            if np.hypot(new_node[0]-goal[0], new_node[1]-goal[1]) < goal_tol:
                tree[goal] = new_node
                return reconstruct_path(tree, goal)
    return None  # no path found within max_iter

def reconstruct_path(tree, node):
    path = [node]
    while tree[node] is not None:
        node = tree[node]
        path.append(node)
    return path[::-1]
```
RRT paths are typically not optimal (unlike A* on a grid) but can be found quickly even in large
or high-dimensional continuous spaces.

## 3. Local Obstacle Avoidance (Conceptual)
Reactive methods like the **potential field** approach treat the goal as an attractive force and
obstacles as repulsive forces, summing them to decide instantaneous movement direction — simple
and fast, but prone to getting stuck at local minima (e.g., in U-shaped obstacles), analogous to
the local-search local-optima problem from classical AI.

## 4. Comparing RRT vs. A*
On the same obstacle map: A* (Week 13) finds a shorter, grid-optimal path but requires
discretizing the space; RRT finds a valid (not necessarily shortest) path faster and scales
better to larger/continuous/higher-dimensional spaces.

## 5. In-Class Exercise
Run the RRT implementation with increasing `max_iter`/decreasing `step_size` and observe the
tradeoff between path quality (closer to A*'s path) and computation time.
