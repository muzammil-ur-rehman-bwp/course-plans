# Lab Manual 15 — Vision & Robotics Survey + Ethics Reflection

**Duration:** 3 hours | **Prerequisite:** Week 15 lecture

## Objectives
Implement a simple image-thresholding/edge-detection demo and a simple reactive-agent grid-world
simulation; complete a short ethics case-study reflection.

## Setup
Create `lab15.ipynb`; use the `simple_edge_detect_1d` function and `reactive_grid_agent`
function from the lecture content as starting points.

## Procedure
1. **Task A — Edge detection (1-D):** implement/paste `simple_edge_detect_1d(row)`; run it on 3
   different synthetic pixel rows (as plain Python lists) and identify which index the largest
   edge occurs at for each.
2. **Task B — Edge detection (2-D, optional extension):** extend Task A to a small 2-D list of
   lists (a tiny synthetic image) by applying the 1-D operator along each row and each column
   separately, then combining (e.g., taking the larger of the two at each pixel).
3. **Task C — Reactive grid agent:** implement/paste `reactive_grid_agent`; run it on a provided
   small grid with obstacles (`"#"`) from a start position to a goal position, printing the
   agent's position at each step until it reaches the goal (or gets stuck).
4. **Task D — Ethics reflection:** write short (3–5 sentence) answers to: (1) which ethical
   concern (bias, safety, privacy, societal impact) is most relevant to the instructor-provided
   case study, and why; (2) one concrete mitigation a development team could have applied.

## Expected Output
A notebook with Tasks A–C; a short written reflection (markdown cells or separate file) for
Task D.

## Submission
Submit `lab15.ipynb` (including the Task D reflection) by the end of the lab session.
