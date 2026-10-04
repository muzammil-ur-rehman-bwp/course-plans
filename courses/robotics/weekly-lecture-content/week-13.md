# Week 13 — Lecture Content: Path Planning I — Grid-Based Planning

## 1. Configuration Space
The **configuration space** (C-space) represents all possible robot configurations (for a
mobile robot in 2D, simply its `(x, y)` position, possibly plus heading). Obstacles in the
physical world become "forbidden regions" in C-space. For this course, we approximate C-space
with the occupancy grid built in Week 9.

## 2. Grid-Based Search for Navigation
This directly reframes the search algorithms from classical AI (equivalent to a search-course's
A* on graphs) onto a navigation grid: each free grid cell is a "state," and moving to an
adjacent free cell is an "action."
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

## 3. Path Cost vs. Heuristic Cost in Navigation
`g(n)` is typically the number of grid cells traveled (or physical distance); `h(n)` is a
distance estimate to the goal (Manhattan or Euclidean distance on the grid) — must remain
admissible (never overestimate) for the planned path to be optimal, exactly as in the classical
A* formulation.

## 4. Visualizing the Planned Path
```python
import matplotlib.pyplot as plt

plt.imshow(grid, cmap='gray_r')
path_arr = list(zip(*path))
plt.plot(path_arr[1], path_arr[0], color='red')
```

## 5. In-Class Exercise
Plan a path between two given points on a provided occupancy grid using `a_star_grid`; re-run
with Euclidean distance instead of Manhattan distance as the heuristic and compare the resulting
paths.
