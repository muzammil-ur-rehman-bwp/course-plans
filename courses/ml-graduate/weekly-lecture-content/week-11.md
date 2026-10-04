# Week 11 — Lecture Content: Structured Prediction — Conditional Random Fields

## 1. Structured Prediction
Many tasks require predicting a *whole structured output* $y=(y_1,\dots,y_T)$ jointly — e.g.,
part-of-speech tagging (a label sequence for a sentence) or named-entity recognition — rather than
a single independent label per instance. Predicting each $y_t$ independently ignores
correlations between adjacent labels (e.g., a verb is unlikely to directly follow a determiner);
structured-prediction models capture these dependencies explicitly.

## 2. Conditional Random Fields (CRFs)
A **linear-chain CRF** directly models the conditional distribution of the whole label sequence
given the whole input sequence:
$$
p(y\mid x) \;=\; \frac{1}{Z(x)} \exp\left(\sum_{t=1}^T \sum_k \theta_k\, f_k(y_{t-1},y_t,x,t)\right),
$$
where $f_k$ are hand-designed or learned **feature functions** (e.g., "current word is
capitalized and current label is PROPER-NOUN," or "previous label is DETERMINER and current label
is NOUN"), $\theta_k$ are learned weights, and
$$
Z(x) = \sum_{y'} \exp\left(\sum_t\sum_k \theta_k f_k(y'_{t-1},y'_t,x,t)\right)
$$
is the **partition function** — a normalizer summed over all possible label sequences $y'$,
computable efficiently for a linear chain via a forward-backward-style dynamic program (the same
recursion structure as the HMM forward algorithm, adapted to this model's potentials). Decoding
(finding $\arg\max_y p(y\mid x)$) uses a Viterbi-style dynamic program, again structurally
identical to HMM decoding.

## 3. Discriminative vs. Generative: the CRF/HMM Contrast
A **Hidden Markov Model (HMM)** is *generative*: it models the full joint
$p(x,y)=p(y_1)\prod_t p(y_t\mid y_{t-1})\cdot\prod_t p(x_t\mid y_t)$, via transition probabilities
$p(y_t\mid y_{t-1})$ and emission probabilities $p(x_t\mid y_t)$, then obtains $p(y\mid x)$ by
Bayes' rule at inference time. A CRF is **discriminative**: it models $p(y\mid x)$ directly, never
modeling $p(x)$ at all. This has two practical consequences: (1) a CRF's feature functions
$f_k(y_{t-1},y_t,x,t)$ may depend on $x$ in arbitrary, overlapping, non-independent ways (e.g.,
"the word is capitalized," "the word ends in *-ing*," "the previous word was 'the'," all at once)
without needing to model how those features of $x$ are jointly distributed — an HMM's emission
model $p(x_t\mid y_t)$ would need to represent (or simplify away) such dependencies explicitly;
(2) a CRF cannot generate new $x$ sequences (it is not a model of $p(x)$ at all), whereas an HMM
can. Parameter estimation also differs: CRF training maximizes the **conditional** log-likelihood
$\sum_i \log p(y^{(i)}\mid x^{(i)})$, a concave objective in $\theta$ solved by gradient-based
convex optimization (Week 6's machinery applies directly); HMM training typically maximizes the
**joint** log-likelihood, often in closed form by counting (fully observed case) or via
Expectation-Maximization (partially observed case). *(This contrast is intentionally brief — general
exact/approximate inference algorithms for probabilistic graphical models and their computational
complexity are covered in full in Artificial Intelligence, Graduate; this course's treatment here
is limited to the discriminative/generative distinction as it bears on structured prediction.)*

## 4. Why Discriminative Can Help
Because a CRF never has to model $p(x)$, it is free to use rich, redundant, overlapping features
of $x$ that would be difficult or impossible to model jointly and correctly in a generative
emission model — this is usually cited as the main practical advantage of CRFs over HMMs for
tasks like named-entity recognition, where many informative, non-independent surface features of
the input (capitalization, suffixes, surrounding words, gazetteer membership, etc.) are available.

## 5. Code: A Linear-Chain CRF for Sequence Labeling
```python
import sklearn_crfsuite  # pip install sklearn-crfsuite

def word_features(sent, i):
    word = sent[i]
    feats = {
        "word.lower": word.lower(),
        "word.isupper": word.isupper(),
        "word.istitle": word.istitle(),
        "word[-3:]": word[-3:],
        "BOS": i == 0,
        "EOS": i == len(sent) - 1,
    }
    if i > 0:
        feats["prev_word.lower"] = sent[i - 1].lower()
    return feats

def sent_features(sent):
    return [word_features(sent, i) for i in range(len(sent))]

train_sents = [
    ["The", "quick", "fox", "jumps"],
    ["A", "lazy", "dog", "sleeps"],
]
train_tags = [
    ["DET", "ADJ", "NOUN", "VERB"],
    ["DET", "ADJ", "NOUN", "VERB"],
]

X_train = [sent_features(s) for s in train_sents]
y_train = train_tags

crf = sklearn_crfsuite.CRF(algorithm="lbfgs", max_iterations=100)
crf.fit(X_train, y_train)

test_sent = ["The", "lazy", "fox", "sleeps"]
pred = crf.predict([sent_features(test_sent)])[0]
print(list(zip(test_sent, pred)))

# Inspect a few learned feature weights to see what the model relies on
top_features = sorted(crf.state_features_.items(), key=lambda kv: -abs(kv[1]))[:5]
print("Top-weighted state features:", top_features)
```
Expect the small CRF to recover the intended tag pattern (DET, ADJ, NOUN, VERB) on the held-out
sentence, given how similar the toy training sentences are to it; inspect the printed feature
weights to see which surface features (e.g., word identity, suffix, position) the model leaned on.

## 6. In-Class Exercise
Articulate, in one paragraph, the discriminative/generative distinction using the CRF/HMM pair as
the concrete example, and give one feature a CRF could use easily that would be awkward for an
HMM's emission distribution to represent (e.g., a feature that looks at both the previous and next
word simultaneously).
