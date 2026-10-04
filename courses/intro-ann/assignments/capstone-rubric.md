# Capstone Project — Grading Rubric

**Due:** Week 16 | **Weight:** 20% of course grade total (includes proposal, implementation, presentation)

| Component | Weight (of 20%) | Criteria |
|---|---|---|
| Proposal | 2% | Clear problem statement, finalized dataset, appropriate architecture, well-defined ablation/comparison, sound evaluation plan |
| Implementation — correctness | 6% | Code runs; model is built and trained correctly with the chosen framework; data is handled appropriately (train/val/test split, normalization) |
| Implementation — ablation/comparison | 5% | The required ablation (e.g., with/without dropout, optimizer A vs. B) is implemented correctly, with both conditions held fair except the one isolated variable |
| Report | 3% | Concise written report: problem, architecture, training setup, ablation results, 1+ limitation/next step |
| Presentation | 4% | Clear 5–7 min talk; training/validation curves and ablation results shown; answers Q&A questions accurately |

## Grading Notes
- A project that honestly reports an ablation result that did not help (e.g., "dropout did not
  improve validation loss on this dataset"), with sound analysis of why, can score as well as a
  project with a clearly beneficial ablation result but weak analysis — the evaluation and
  understanding matter as much as raw performance.
- The ablation/comparison requirement is not optional: a technically correct model trained once,
  with no isolated comparison, cannot receive full credit on the "ablation/comparison" row above.
- Pairs must clearly attribute each member's contribution in the report; grading can differ
  between partners if contributions were significantly uneven.
