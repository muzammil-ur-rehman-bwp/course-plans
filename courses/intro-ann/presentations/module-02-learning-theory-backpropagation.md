# Presentation: Module 2 — Learning Theory: Loss, Gradient Descent, Backpropagation (Weeks 5–8)

**Format:** Slide deck outline for lecture delivery.

1. **Title slide** — Module 2: Learning Theory: Loss, Gradient Descent, Backpropagation
2. **Why a loss function** — turning a prediction and a label into one number to minimize
3. **MSE, binary cross-entropy, categorical cross-entropy** — formulas side by side
4. **Loss/activation pairing** — the MSE-vs-cross-entropy gradient-at-saturation derivation
5. **The gradient & gradient descent** — $\theta \leftarrow \theta - \eta\nabla L(\theta)$,
   steepest-descent intuition
6. **Learning rate effects** — convergence/divergence plot on a 1D quadratic
7. **Batch vs. stochastic vs. mini-batch** — trade-off table; why shuffling matters
8. **The chain rule, reviewed** — scalar composition before vectors/matrices
9. **Backpropagation derivation** — $\delta^{(2)}=\hat y-y$; the recursive
   $\delta^{(l)}=(W^{(l+1)\top}\delta^{(l+1)})\odot g'(z^{(l)})$ rule
10. **Worked numeric example** — the full forward/backward pass on the lecture's 2-layer network
11. **From derivation to code** — the `NeuralNetwork` class (`forward`/`backward`/`update`)
12. **XOR, solved by training** — the from-scratch network's loss curve converging to zero
13. **Module recap** — every gradient this semester traces back to this one recursive rule

**Speaker notes:** slide 10 (worked numeric example) should be presented with every intermediate
number visible, not condensed — this is the slide that converts "I can recite the formula" into
"I can compute the formula."
