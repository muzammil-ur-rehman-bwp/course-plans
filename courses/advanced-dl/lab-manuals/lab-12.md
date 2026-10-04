# Lab Manual 12 — Critiquing a Current Fast-Sampler Paper

**Duration:** 3 hours | **Prerequisite:** Week 12 lecture

## Objectives
Critically evaluate a current consistency-model-style or step-distillation paper's reported
quality/latency tradeoff; optionally extend the Week 2 toy sampler with a simple consistency-
style self-distillation loss.

## Setup
1. Obtain the instructor-provided current paper (refreshed each offering) from the course
   reading list.
2. Create `lab12-critique.md` (required) and, optionally, `lab12.ipynb` (extension).

## Procedure
1. **Task A — Claim extraction:** state the paper's central quality/latency claim precisely,
   including the exact step counts and benchmark(s) compared.
2. **Task B — Baseline fairness:** assess whether the paper's baseline (the many-step sampler it
   compares against) is current and fairly matched (same model size/training data where
   applicable), or whether the comparison could be inflated by an undertuned baseline.
3. **Task C — Caution flag:** identify one specific aspect of the reported result that should be
   read with appropriate caution (a single benchmark, a specific guidance-scale regime, a
   specific hardware configuration, etc.), per the Week 12 Section 5 checklist.
4. **Task D (optional, bonus up to +2):** extend your Week 2 toy 2-D score model with a simple
   consistency-style self-distillation loss (train a second network to map two adjacent
   trajectory points from the Week 2 sampler to the same final clean sample) and compare its
   one-step sample quality against the Week 2 many-step sampler.

## Expected Output
A written critique (`lab12-critique.md`) covering Tasks A–C, and optionally a notebook for Task D.

## Submission
Submit `lab12-critique.md` (and `lab12.ipynb` if attempted) via the course submission system by
the end of the lab session.
