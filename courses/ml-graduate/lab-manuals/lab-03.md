# Lab Manual 3 — Brute-Force Shattering and VC Dimension

**Duration:** 3 hours | **Prerequisite:** Week 3 lecture

## Objectives
Confirm, by brute-force search, the VC dimension of linear halfspace classifiers in
$\mathbb{R}^2$ and of intervals on the real line.

## Setup
Create `lab03.ipynb`. NumPy only.

## Procedure
1. **Task A — Interval shattering:** implement a shattering check for intervals $[a,b]$ on
   $\mathbb{R}$; verify a 2-point set can be shattered and find (or argue) that no 3-point set can.
2. **Task B — Halfplane shattering:** implement the brute-force halfplane shattering check from
   the lecture content; verify a 3-point (non-collinear) set in $\mathbb{R}^2$ is shattered.
3. **Task C — The 4-point case:** test the square-corners 4-point set from the lecture; identify
   and print the specific labeling that fails, and verify it is the "opposite corners same label"
   pattern.
4. **Task D — General position vs. not:** test a 4-point set where 3 points are collinear; discuss
   whether this changes your conclusion about $\mathrm{VCdim}$.
5. **Task E — Reflection:** state, in your own words, why proving $\mathrm{VCdim}(H)=d$ requires
   *both* a shattered $d$-point set and an impossibility argument for every $(d+1)$-point set, not
   just the one tested in code.

## Expected Output
A notebook with Tasks A–E, including the printed failing labeling for Task C.

## Submission
Submit `lab03.ipynb` by the end of the lab session.
