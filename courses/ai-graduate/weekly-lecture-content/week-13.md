# Week 13 — Lecture Content: Research Methods in AI

## 1. How to Read a Research Paper Efficiently
Graduate AI research moves fast, and most papers are not read linearly front-to-back on a first
pass. A standard efficient strategy:
1. **Abstract and title** — what is the claim, in one or two sentences?
2. **Figures and results tables** — what does the evidence actually look like, before reading
   any explanation of it?
3. **Methods** — how was the result produced? Is it a small, understandable modification of a
   known technique, or a substantially new one?
4. **Related work** — how does this position itself relative to prior work, and does that
   framing look honest?
5. **Full read** — only after the above, for papers that pass the first four checks and are
   directly relevant to your work (e.g., a capstone literature-review candidate).

## 2. How to Critique a Paper
A structured critique separates **claim**, **evidence**, and **method** and asks pointed
questions of each:
- **Claim:** What exactly is being claimed? Is it precisely stated, or vague/overreaching
  relative to the evidence?
- **Evidence:** Do the reported results actually support the claim? Are the metrics appropriate
  for the problem?
- **Baselines:** Are the baselines the paper compares against still representative/competitive,
  or outdated/weak strawmen that make the proposed method look better than it would against a
  fair comparison?
- **Ablations:** Does the paper isolate *which* part of its proposed method is responsible for
  the improvement, or does it only report one end-to-end number?
- **Reproducibility:** Could an independent researcher re-run this and get a comparable result?

## 3. Reproducibility Concerns in AI Research
Common, well-documented reproducibility failures:
- **Missing hyperparameters** — learning rate, exploration schedule, regularization strength, or
  other settings not fully reported, making exact replication impossible.
- **Cherry-picked seeds** — reporting the best of several random seeds without disclosing the
  spread, which overstates the method's reliability.
- **Undisclosed compute** — not reporting how much compute/search/tuning was spent on the
  proposed method vs. the baselines, which can make an unfair comparison look fair.
- **Non-public code/data** — without released code or data, even a well-described method can be
  practically impossible to reproduce exactly.
A critique should check for each of these explicitly, not just react to whether the reported
numbers "look good."

## 4. Experimental Design
**Benchmarks.** A good benchmark is a standard, shared problem/dataset that lets different
methods be compared on equal footing; critique whether a paper's benchmark is standard,
appropriately difficult, and not cherry-picked to favor the proposed method.

**Baselines.** The strength of a result is only as meaningful as the baseline it beats. A method
that beats a weak or outdated baseline by a wide margin is far less impressive than one that
beats a strong, current baseline by a small but consistent margin.

**Ablation studies.** An ablation study removes (or varies) one component of a proposed method at
a time, holding everything else fixed, to isolate that component's actual contribution to the
overall result. For example, if a new planning heuristic combines idea A and idea B, an ablation
would separately test "A only," "B only," and "A and B together" against the baseline, to
determine whether both ideas are actually necessary or if most of the benefit comes from one.

**Statistical significance.** A single run's numbers are not evidence of a real difference
between two methods — stochastic algorithms (e.g., Q-learning with ε-greedy exploration,
simulated annealing, genetic algorithms — all covered this semester) can vary run to run purely
from randomness. The standard remedy is to run each method multiple times with different random
seeds, report a measure of spread (e.g., mean ± standard deviation, or a confidence interval)
alongside the mean, and — ideally — perform a statistical test (e.g., a t-test or a non-parametric
test appropriate to the sample size) before claiming one method is genuinely better than another.
This course does not require a deep statistics background; the learning outcome is the
*judgment* to recognize when a single-run comparison is insufficient evidence, which applies
directly to the research capstone's own experiment.

```python
import statistics

def summarize_runs(results):
    """results: list of a metric's value across multiple random-seed runs of one method."""
    mean = statistics.mean(results)
    stdev = statistics.stdev(results) if len(results) > 1 else 0.0
    return {"mean": mean, "stdev": stdev, "n_runs": len(results)}

def naive_significance_gap(results_a, results_b):
    """A simple, non-rigorous check: do the two methods' mean±stdev ranges overlap?
    This is a teaching heuristic, not a substitute for a proper statistical test."""
    a, b = summarize_runs(results_a), summarize_runs(results_b)
    a_low, a_high = a["mean"] - a["stdev"], a["mean"] + a["stdev"]
    b_low, b_high = b["mean"] - b["stdev"], b["mean"] + b["stdev"]
    overlaps = a_low <= b_high and b_low <= a_high
    return {"a": a, "b": b, "ranges_overlap": overlaps}
```

## 5. In-Class/Lab Exercise
Using the structured critique worksheet, critique a short paper excerpt provided by the
instructor: identify the claim, evidence, baseline adequacy, and at least one reproducibility
concern; then design (on paper, not implemented) an ablation study for a hypothetical AI system
combining two components, specifying what each ablation condition would isolate.
