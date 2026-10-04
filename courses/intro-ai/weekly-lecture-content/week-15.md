# Week 15 — Lecture Content: Computer Vision & Robotics Overview + AI Ethics

## 1. Computer Vision: The Core Problem
An image is just an array of pixel intensities — the "meaning" (that this is a cat, that this
is a stop sign) is not explicit in the data at all. Computer vision is the problem of recovering
structure and meaning from pixel arrays. Modern vision systems are overwhelmingly deep-learning-
based (Week 13's survey), but simple, classical operations remain foundational and illustrate
the core challenge concretely.

### Edge Detection: A Simple, Concrete Operation
Edges (abrupt intensity changes) often correspond to object boundaries. A minimal 1-D gradient
operator illustrates the idea without any image library:

```python
def simple_edge_detect_1d(row):
    """Returns the absolute difference between each pixel and its right neighbor."""
    return [abs(row[i + 1] - row[i]) for i in range(len(row) - 1)]

row = [10, 12, 11, 200, 205, 190, 12, 11]
edges = simple_edge_detect_1d(row)
print(edges)  # a clear spike where intensity jumps (the object boundary)
```
A real 2-D edge detector (e.g., a Sobel filter) applies a similar idea in both dimensions over a
full image; the 1-D version above keeps the underlying idea — "look for large local intensity
differences" — visible without requiring an image-processing library.

## 2. Robotics: The Core Problem
A robot must sense, plan, and act in a physical world that is dynamic, only partially observable,
and continuous (recall Week 2's environment properties) — a much harder environment than chess
or Tic-Tac-Toe.

### The Sense-Plan-Act Cycle
1. **Sense**: gather sensor data (cameras, lidar, encoders) — the robot's percepts, per Week 1's
   PEAS framework.
2. **Plan**: decide what to do, often via search/planning (Weeks 3–4, 9) over a map or
   configuration space — e.g., path planning is a direct application of A*.
3. **Act**: execute the chosen action through actuators, then sense again (the cycle repeats).

### A Minimal Reactive-Agent Illustration
```python
def reactive_grid_agent(position, grid, goal):
    """A simple reflex-style controller (Week 2) that greedily reduces Manhattan distance
       to the goal, stepping around obstacles by preferring the first open, improving move."""
    row, col = position
    moves = [(row - 1, col), (row + 1, col), (row, col - 1), (row, col + 1)]

    def manhattan(p):
        return abs(p[0] - goal[0]) + abs(p[1] - goal[1])

    valid_moves = [m for m in moves if 0 <= m[0] < len(grid) and 0 <= m[1] < len(grid[0])
                   and grid[m[0]][m[1]] != "#"]
    if not valid_moves:
        return position
    return min(valid_moves, key=manhattan)
```
This reactive controller is deliberately simple (no planning, no memory) — a reminder that real
robotics applications typically combine reactive control for fast, local decisions with planning
(Weeks 3–4, 9) for longer-horizon decisions.

## 3. AI Ethics
As AI systems (classical and learned) are deployed more widely, four recurring concerns arise:

- **Bias and fairness**: a system trained or designed using unrepresentative data or rules can
  systematically disadvantage some group (e.g., a hiring tool trained on historically biased
  hiring data perpetuates that bias).
- **Safety**: an agent should behave predictably and avoid harmful actions, especially in
  high-stakes environments (medical diagnosis tools, autonomous vehicles) — connecting back to
  Week 1's performance measure: *what* an agent optimizes for matters enormously.
- **Privacy**: AI systems often rely on personal data; collecting, storing, and using it raises
  consent and security questions.
- **Societal impact**: automation changes labor markets, and increasingly capable AI systems
  raise accountability questions — when an AI-assisted decision causes harm, who is responsible?

These are not abstract concerns: the Bayesian diagnostic tool from Week 11, the decision tree
from Week 12, and the bag-of-words classifier from Week 14 could all, in a real deployment,
reflect biased data or be misapplied outside the narrow conditions they were validated for.

## 4. Case Discussion
Small groups discuss a short case study (e.g., a resume-screening tool that systematically
down-ranks candidates from a particular background) and identify which ethical concern (bias,
safety, privacy, societal impact) is most directly implicated, and what a responsible team
could have done differently.

## 5. In-Class Exercise
For the case-study scenario, each group identifies the most relevant ethical concern and
proposes one concrete mitigation; report back to the class.
