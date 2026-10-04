# Lab Notes 10 — Autoencoders and Denoising Autoencoders

**Concept recap:** an autoencoder's loss compares its output against its *own* input (or, for a
denoising autoencoder, against the *clean* version of a corrupted input) — there are no external
labels involved.

**Common pitfalls:**
- Accidentally comparing the denoising autoencoder's reconstruction against the *noisy* input
  rather than the clean target, which trains the model to do nothing useful (approximate the
  identity on noisy data) instead of denoising.
- Using `nn.Sigmoid()` on the decoder's output while not scaling input pixel values to `[0, 1]`
  (or vice versa, using `nn.Tanh()` without scaling to `[-1, 1]`) — the output activation and the
  input data's value range must match.
- Forgetting `model.eval()` before extracting latent codes for Task C's visualization, which can
  introduce unnecessary dropout noise (if present in the architecture) into otherwise
  deterministic latent representations.
- In Task D, comparing the linear autoencoder's reconstruction error against PCA using a
  different number of components/latent dimensions on each side, invalidating the comparison.

**Debugging tip:** if reconstructions in Task A look like a uniform gray blur regardless of
input, check the loss function and output activation pairing first (Sigmoid output with
`MSELoss`/`BCELoss` against `[0,1]`-scaled targets) before suspecting the architecture itself.

**Instructor tip:** Task D often surprises students when the linear autoencoder's error is very
close to PCA's — use that result explicitly to reinforce the lecture's theoretical claim, rather
than letting it pass as an unremarkable coincidence.
