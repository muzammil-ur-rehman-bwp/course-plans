# Presentation: Module 3 — Training in Practice: Optimizers, Regularization, Frameworks (Weeks 9–12)

**Format:** Slide deck outline for lecture delivery.

1. **Title slide** — Module 3: Training in Practice: Optimizers, Regularization, Frameworks
2. **Why zero initialization fails** — the symmetry argument
3. **Xavier/Glorot vs. He initialization** — formulas, matched to activation choice
4. **Vanishing/exploding gradients** — the product-of-factors argument across depth
5. **Momentum** — velocity term; ball-rolling-downhill intuition
6. **RMSProp** — per-parameter adaptive learning rate via running squared-gradient average
7. **Adam** — combining both, with bias-corrected moment estimates (derived)
8. **Optimizer convergence comparison** — SGD vs. Momentum vs. Adam on an elongated loss bowl
9. **Overfitting & L1/L2 regularization** — weight decay derivation
10. **Dropout** — training-time masking + inverted scaling; test-time behavior
11. **Early stopping** — using the validation curve directly as a stopping signal
12. **Autograd** — framework automatic differentiation as a generalization of Module 2's
    backpropagation
13. **The framework training loop** — `zero_grad`/`forward`/`backward`/`step`, mapped line by
    line to concepts from Weeks 8–10
14. **Module recap** — from a hand-derived gradient to a one-line `optimizer.step()` call,
    nothing was magic

**Speaker notes:** slide 13 is the module's payoff — spend time explicitly mapping each framework
line back to a hand-built equivalent from Weeks 8–10, so the framework is understood, not just
used.
