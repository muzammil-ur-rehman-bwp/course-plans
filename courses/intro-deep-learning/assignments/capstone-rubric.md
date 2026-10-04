# Capstone Project — Grading Rubric

**Due:** Week 16 | **Weight:** 20% of course grade total (includes proposal, implementation, presentation)

| Component | Weight (of 20%) | Criteria |
|---|---|---|
| Proposal | 2% | Clear problem statement, finalized dataset, appropriate architecture, well-defined design trade-off, sound evaluation plan |
| Implementation — correctness | 6% | Code runs; model is built and trained correctly with PyTorch; data is handled appropriately (train/val/test split, normalization/augmentation as relevant) |
| Implementation — evaluation & trade-off | 5% | The required evaluation is correctly computed and reported; the proposed design trade-off (e.g., transfer learning vs. from-scratch, optimizer comparison, LSTM vs. GRU, VAE vs. GAN) is implemented and compared fairly |
| Report | 3% | Concise written report: problem, architecture, training setup, evaluation results, at least one limitation/next step |
| Presentation | 4% | Clear 5–7 min talk; training/evaluation results and the design trade-off's results shown; answers Q&A questions accurately |

## Grading Notes
- A project that honestly reports a design trade-off that did not help (e.g., "fine-tuning did
  not outperform feature extraction on this small dataset"), with sound analysis of why, can score
  as well as a project with a clearly beneficial result but weak analysis — the evaluation and
  understanding matter as much as raw performance.
- The design-trade-off requirement is not optional: a technically correct model trained once, with
  no comparison or evaluation beyond a single accuracy number, cannot receive full credit on the
  "evaluation & trade-off" row above.
- Pairs must clearly attribute each member's contribution in the report; grading can differ
  between partners if contributions were significantly uneven.
