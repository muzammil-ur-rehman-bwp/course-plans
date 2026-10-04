# Lab Manual 3 — Building a Reverse-Mode Autodiff Engine

**Duration:** 3 hours | **Prerequisite:** Week 3 lecture

## Objectives
Implement a minimal scalar-valued reverse-mode automatic differentiation engine from scratch, and
verify it reproduces hand-derived and PyTorch-computed gradients.

## Setup
Create `lab03.ipynb`. Start from the lecture's `Value` class.

## Procedure
1. **Task A — Extend the engine:** add `__sub__`, `__truediv__`, `__pow__` (integer exponents),
   and `relu` methods to the lecture's `Value` class, each with a correct `_backward` closure.
2. **Task B — Shared-node check:** build $f(x) = x \cdot x + x$ (note $x$ is used **twice** —
   once in the product, once added directly) and confirm `.backward()` gives $df/dx = 2x+1$ at
   $x=3$ (i.e., $7$), not an incorrect value from only partially accumulating $x$'s gradient.
3. **Task C — Reproduce Week 1's network:** rebuild the Lab 1 two-input, 3-hidden, 1-output
   network using only your `Value`-based engine (no NumPy matrix ops — build it unit by unit with
   scalars), and confirm its backward pass matches Lab 1's analytic gradients.
4. **Task D — Cross-check against PyTorch:** reimplement the same small computation using
   `torch.Tensor` with `requires_grad=True` and confirm `.grad` matches your engine's output.

## Expected Output
A notebook with Tasks A–D; Task B's result must be printed explicitly with the expected value
shown alongside it.

## Submission
Submit `lab03.ipynb` by the end of the lab session.
