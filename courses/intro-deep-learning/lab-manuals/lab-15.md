# Lab Manual 15 — Case Study: Bias Audit and Training Compute Cost

**Duration:** 3 hours | **Prerequisite:** Week 15 lecture

## Objectives
Analyze a provided dataset/model scenario for a subgroup performance gap; estimate relative
training compute cost for two example model configurations.

## Setup
Create `lab15.ipynb`. Use the provided `lab15_bias_case_study` materials (a dataset with a known
subgroup split and a pretrained model's predictions on it).

## Procedure
1. **Task A — Subgroup accuracy:** compute overall accuracy and per-subgroup accuracy for the
   provided model's predictions; report the gap, if any, between subgroups.
2. **Task B — Likely cause analysis:** inspect the provided dataset's subgroup composition
   (class balance, subgroup representation); in a markdown cell, connect any observed accuracy
   gap to a plausible data-composition cause from lecture (skewed representation or a proxy
   feature), rather than assuming an architecture flaw.
3. **Task C — Compute cost estimation:** using `relative_training_cost` from lecture, estimate
   and compare the relative training compute for two provided model configurations (parameter
   count, training steps, batch size); report the ratio.
4. **Task D — Written reflection:** in 4–6 sentences, discuss one mitigation you would recommend
   for Task B's finding, and one practical implication of Task C's compute-cost comparison for a
   resource-constrained team.

## Expected Output
A notebook with Tasks A–D, the subgroup accuracy table, the compute-cost ratio, and the written
reflection.

## Submission
Submit `lab15.ipynb` by the end of the lab session.
