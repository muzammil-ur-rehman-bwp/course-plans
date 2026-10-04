# Week 10 Lecture Plan — Introduction to AI
## Topic: Uncertainty & Probability

**Duration:** 2 hours lecture + 3 hour lab

### Learning Objectives (Bloom's Level)
1. Explain why logic alone is insufficient for reasoning under uncertainty. (*Understand*)
2. State and apply joint, marginal, and conditional probability, and the product rule. (*Apply*)
3. Apply Bayes' rule to compute a posterior probability from a prior and likelihood. (*Apply*)

### Lecture Structure
| Time | Segment | Activity |
|---|---|---|
| 0:00–0:20 | Why uncertainty? | Limits of logic (e.g., "the car will start" is not certain); degrees of belief |
| 0:20–0:45 | Probability basics | Joint, marginal, conditional probability; the product rule |
| 0:45–1:00 | Break | — |
| 1:00–1:30 | Bayes' rule | Derivation from the product rule; prior, likelihood, posterior, normalization |
| 1:30–1:50 | Worked example | Diagnostic-test example: computing P(disease \| positive test) from sensitivity, specificity, prior prevalence |
| 1:50–2:00 | Independence | Conditional independence, and why it matters for compact models (preview of Bayesian networks) |

### Materials/Equipment
- Slides: Bayes' rule derivation, diagnostic-test worked numbers
- Starter notebook: Bayes'-rule calculator skeleton

### Formative Check (in-class)
Exercise: given a disease prevalence of 1%, a test sensitivity of 99%, and a false-positive rate
of 5%, compute P(disease | positive test) by hand, then verify with code.

### Link to Lab/Assessment
Lab 10: Implement Bayes' rule in Python for a medical-diagnostic-test example.
