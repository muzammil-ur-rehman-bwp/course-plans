# Week 10 — Lecture Content: Uncertainty & Probability

## 1. Why Logic Alone Is Not Enough
Logic (Weeks 6–8) requires sentences to be true or false with certainty. Real agents must act
under **uncertainty**: a diagnostic test is not perfectly accurate, sensors are noisy, and
outcomes of actions are not guaranteed. Probability theory lets an agent represent degrees of
belief and update them as evidence arrives.

## 2. Probability Basics
- **Joint probability** `P(A, B)`: the probability that both `A` and `B` hold.
- **Marginal probability** `P(A)`: the probability of `A` regardless of `B`, obtained by summing
  the joint over all values of `B`.
- **Conditional probability** `P(A | B) = P(A, B) / P(B)` (defined when `P(B) > 0`): the
  probability of `A` given that `B` is known to hold.
- **The product rule**: `P(A, B) = P(A | B) P(B) = P(B | A) P(A)`.

## 3. Bayes' Rule
Rearranging the product rule gives Bayes' rule:

```
P(A | B) = P(B | A) * P(A) / P(B)
```

In diagnostic terms: `P(disease | positive test) = P(positive test | disease) * P(disease) / P(positive test)`,
where `P(disease)` is the **prior**, `P(positive test | disease)` is the **likelihood**
(related to the test's sensitivity), and `P(positive test)` is a normalizing constant.

## 4. Worked Example: A Diagnostic Test
Suppose a disease has prevalence `P(D) = 0.01`, the test's sensitivity is `P(+|D) = 0.99`
(true positive rate), and its false-positive rate is `P(+|¬D) = 0.05`. What is `P(D|+)`?

```python
def bayes_diagnostic(prior, sensitivity, false_positive_rate):
    p_d = prior
    p_not_d = 1 - prior
    p_pos_given_d = sensitivity
    p_pos_given_not_d = false_positive_rate

    # P(+) by the law of total probability
    p_pos = p_pos_given_d * p_d + p_pos_given_not_d * p_not_d
    p_d_given_pos = (p_pos_given_d * p_d) / p_pos
    return p_d_given_pos

result = bayes_diagnostic(prior=0.01, sensitivity=0.99, false_positive_rate=0.05)
print(f"P(disease | positive test) = {result:.4f}")  # approx 0.1667
```

This is the classic, counter-intuitive result: even with a 99%-sensitive test, a positive result
only raises the probability of disease to about 17%, because the disease is rare and false
positives from the large healthy population outnumber true positives from the small sick
population. This is exactly why **the prior matters** — ignoring it (a common human reasoning
error called base-rate neglect) leads to badly miscalibrated beliefs.

## 5. Independence and Conditional Independence
Two events `A` and `B` are **independent** if `P(A, B) = P(A) P(B)` (equivalently,
`P(A | B) = P(A)` — knowing `B` tells you nothing about `A`). They are **conditionally
independent given C** if `P(A, B | C) = P(A | C) P(B | C)`. Conditional independence is the
assumption that lets us represent large joint distributions compactly — exactly the idea behind
Bayesian networks, next week.

```python
def are_independent(p_a, p_b, p_a_and_b, tolerance=1e-9):
    return abs(p_a_and_b - p_a * p_b) < tolerance
```

## 6. In-Class Exercise
Given a disease prevalence of 1%, a test sensitivity of 99%, and a false-positive rate of 5%,
compute `P(disease | positive test)` by hand using Bayes' rule, then verify with
`bayes_diagnostic`. Then try a prevalence of 50% and discuss why the posterior changes so much.
