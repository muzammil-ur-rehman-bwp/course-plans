# Lab Manual 12 — Generative Adversarial Networks

**Duration:** 3 hours | **Prerequisite:** Week 12 lecture

## Objectives
Build and train a simple GAN on MNIST/Fashion-MNIST; track generator/discriminator losses and
generated-sample quality across training; identify any failure modes observed.

## Setup
Create `lab12.ipynb`.

## Procedure
1. **Task A — Generator and discriminator:** implement `Generator` and `Discriminator` as in
   lecture; verify output shapes.
2. **Task B — Adversarial training loop:** implement the alternating training loop as in
   lecture, remembering to `.detach()` generated images when training the discriminator; train for
   a provided number of epochs.
3. **Task C — Monitoring training:** plot generator and discriminator losses across training on
   one chart; save and display a grid of generated samples at several checkpoints (e.g., epoch 1,
   epoch 10, final epoch) to visually track sample quality over time.
4. **Task D — Failure-mode check:** inspect the final grid of generated samples for signs of mode
   collapse (very similar-looking outputs); if observed, note it; if not, explain what evidence in
   the samples/losses supports that training was reasonably healthy.

## Expected Output
A notebook with Tasks A–D, the loss plot, the sample grids at multiple checkpoints, and the
written failure-mode assessment.

## Submission
Submit `lab12.ipynb` by the end of the lab session.
