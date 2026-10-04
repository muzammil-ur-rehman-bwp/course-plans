# Lab Manual 1 — PyTorch Tensors, Autograd, and the Training Loop

**Duration:** 3 hours | **Prerequisite:** Week 1 lecture

## Objectives
Create and manipulate PyTorch tensors; use autograd to confirm it reproduces hand-derived
gradients from the prerequisite course; build an `nn.Module` and write the canonical training
loop; reproduce a known small-MLP result in PyTorch.

## Setup
Create `lab01.ipynb`. Verify `torch.__version__` and (if available) GPU access via
`torch.cuda.is_available()`.

## Procedure
1. **Task A — Tensors & autograd:** create tensors with `requires_grad=True`, build the small
   expression `z = x0*w0 + x1*w1 + b`, `y = sigmoid(z)`, `loss = (y - target)**2`; call
   `loss.backward()`; compare `.grad` values against a hand-derived gradient computed on paper
   first.
2. **Task B — `nn.Module`:** implement `SmallMLP` (as in lecture) with configurable
   `in_features`, `hidden`, `out_features`; verify output shape for a batch of inputs.
3. **Task C — Training loop:** using a small provided synthetic classification dataset
   (`lab01_data`), write the full `zero_grad → forward → loss → backward → step` loop; train for
   at least 20 epochs; plot the training loss curve.
4. **Task D — Cross-check against prior knowledge:** reimplement the same dataset/architecture's
   forward pass using NumPy (reusing code/logic from the prerequisite course if available), and
   confirm the PyTorch model's initial-step loss is consistent in magnitude with a matching NumPy
   computation using the same weights.

## Expected Output
A notebook with Tasks A–D, including the loss curve plot and the Task D cross-check.

## Submission
Submit `lab01.ipynb` by the end of the lab session.
