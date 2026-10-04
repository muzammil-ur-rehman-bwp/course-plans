# Lab Notes 11 — A Linear-Chain CRF for Sequence Labeling

**Concept recap:** a linear-chain CRF models $p(y\mid x)$ directly using arbitrary, overlapping
feature functions of $x$; it does not model $p(x)$ at all, unlike a generative HMM.

**Common pitfalls:**
- **Confusing correlation with causation** is not this lab's risk, but **confusing
  discriminative with generative** is its direct analogue — students sometimes assume a CRF's
  feature weights have the same "transition probability" interpretation an HMM's do; they do not,
  since CRF weights are free real numbers (not constrained to sum to 1 like a conditional
  probability table) and are learned jointly to maximize conditional likelihood.
- Training on too little data (e.g., the 2-sentence toy corpus from the lecture) and expecting
  the CRF to generalize to structurally different test sentences — it will not, and this is
  expected, not a bug; a real evaluation needs a larger, more varied corpus (Task D's comparison
  should use whatever corpus the instructor provides, not the 2-sentence toy example alone).
- Treating a feature's large learned weight magnitude as proof of its "importance" without
  checking how often that feature actually fires in the data — a rarely-firing feature can have a
  large weight with little practical effect.

**Debugging tip:** if `crf.predict` raises an error about mismatched feature dictionaries, check
that `sent_features` is applied identically (same feature keys) to both training and test
sentences — a stray extra or missing key is the most common cause.

**Instructor tip:** use Task E to make the discriminative/generative distinction concrete and
memorable — ask students to try adding a "next word" look-ahead feature to the CRF (trivial) and
then ask them to describe, out loud, what modeling change an HMM would need to use the same
information (a much harder, awkward change to its emission model).
