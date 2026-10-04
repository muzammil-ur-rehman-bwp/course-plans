# Lab Notes 12 — Machine Learning Survey: A Tiny Decision Tree

**Concept recap:** entropy measures label impurity in a set of examples; information gain
measures how much an attribute split reduces entropy; ID3 recursively splits on the
highest-information-gain attribute until a stopping condition (pure subset, or no attributes
left) is reached.

**Common pitfalls:**
- Computing entropy with `p = 0` or `p = 1` and calling `math.log2(0)`, which raises an error —
  always special-case pure sets to return entropy 0 (as shown in lecture).
- Re-using an already-split attribute further down the tree in a basic ID3 implementation —
  decide explicitly (and document) whether your version removes used attributes from the
  candidate list at each recursive call.
- Building a tree that overfits perfectly to the tiny training set and mistaking 100% training
  accuracy for evidence the tree "works well" — the lecture explicitly flags this as expected
  and not informative about generalization.

**Debugging tip:** print the information gain for every candidate attribute at each recursive
call before committing to a split — an attribute with unexpectedly low or zero gain often
indicates a data-encoding bug (e.g., a typo'd attribute value that creates a spurious extra
category).

**Instructor tip:** ask students to predict, by eye, which attribute should split first on the
toy dataset before computing information gain — most will guess correctly on an obvious
dataset, which makes the few counter-intuitive cases (where the "obvious" attribute is not
actually the highest-gain one) land harder.
