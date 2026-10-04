# Lab Notes 12 — Generative Adversarial Networks

**Concept recap:** the discriminator is trained to distinguish real from generated samples; the
generator is trained to fool the discriminator; `.detach()` prevents the discriminator's update
from backpropagating into the generator's parameters.

**Common pitfalls:**
- Forgetting `.detach()` on `fake_images` when computing the discriminator's loss on generated
  samples — without it, the discriminator's `.backward()` call also computes (unused, wasted, and
  potentially incorrect if gradients are reused) gradients through the generator.
- GAN training instability/mode collapse mistaken for "my code is broken" — unlike supervised
  training, oscillating losses are common even in a correctly-implemented GAN; the Week 12
  failure-mode discussion exists precisely because this is expected behavior to recognize, not
  always a bug.
- Using too high a learning rate for both networks simultaneously, which tends to cause the kind
  of training instability described in lecture faster than a mismatched learning-rate balance
  would; the lecture's suggested optimizer settings (`lr=2e-4, betas=(0.5, 0.999)`) are a
  reasonable, commonly-used starting point, not an arbitrary choice.
- Evaluating generator quality purely from the final loss value — as emphasized in lecture, GAN
  losses are not a reliable progress signal; Task C's sample grids at multiple checkpoints are the
  intended evaluation method.

**Debugging tip:** if the discriminator's loss collapses to near zero almost immediately and
stays there, the discriminator has likely become too strong for the generator to ever get a
useful learning signal — try reducing the discriminator's learning rate relative to the
generator's, or training the generator for more steps per discriminator step.

**Instructor tip:** show a known "mode collapse" sample grid from a reference run (if available)
alongside a healthier one, so students have a visual anchor for what Task D's failure-mode check
should actually look like.
