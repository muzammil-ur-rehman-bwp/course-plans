# Week 15 Summary — Evaluating and Debugging Neural Networks

**Key takeaways:**
- Four reference loss-curve patterns — healthy, underfitting, overfitting, broken — summarize the
  diagnostic space for any network trained this semester, regardless of architecture.
- A broken training loop is debugged cheapest-check-first: labels, normalization, learning rate,
  loss/activation pairing, then a gradient check for from-scratch components.
- Systematic hyperparameter tuning (e.g., grid search over learning rate, width, regularization
  strength) selects configurations by *validation* performance, never the test set.
- This evaluation lens applies uniformly across every architecture covered this semester (MLP,
  CNN, RNN), unifying Weeks 1–14 into one practical diagnostic skill.

**You should now be able to:** diagnose a training run's loss curve into one of four categories
and propose the correct fix; run a systematic hyperparameter sweep and select by validation
performance.

**Next week:** capstone project presentations and a course-wide review, connecting every topic
from the McCulloch-Pitts neuron through CNNs and RNNs into one coherent map.
