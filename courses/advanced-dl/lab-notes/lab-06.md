# Lab Notes 6 — Bradley-Terry Reward Model

**Concept recap:** Bradley-Terry derives $P(y_1\succ y_2\mid x)=\sigma(r(x,y_1)-r(x,y_2))$ from an
odds-ratio assumption; fitting $r_\phi$ by maximum likelihood on preference pairs is exactly a
logistic/cross-entropy loss.

**Common pitfalls:**
- Generating preference labels deterministically (always label the higher-$r^*$ item as
  preferred) rather than via the Bradley-Terry sampling distribution — this produces unrealistically
  clean data and can mask bugs that would surface with properly noisy labels.
- Forgetting that only the *difference* $r(x,y_1)-r(x,y_2)$ is identified by preference data —
  the fitted $r_\phi$'s absolute scale/offset is arbitrary; comparing raw fitted values across
  different training runs without accounting for this is a common source of confusion when
  debugging Task C.
- Using Pearson correlation instead of Spearman in Task C — Bradley-Terry only identifies $r$ up
  to a monotonic-preserving transformation in practice (strictly, up to an additive constant, but
  finite-sample fitting noise makes rank-based comparison the more robust check), so a rank
  correlation is the more appropriate verification than a linear one.
- In Task D, writing a vague answer ("the policy might do something bad") rather than a specific,
  constructed example showing exactly which reward-model blind spot an unconstrained ($\beta=0$)
  optimizer would exploit in your own toy setup.

**Debugging tip:** before trusting Task B's training, verify on a tiny 2-item toy case (known
$r^*$, several pairs) that the reward model's loss decreases and that it eventually ranks the two
items correctly.

**Instructor tip:** ask students to predict, before running Task C, roughly how many preference
pairs their synthetic dataset needs for the Spearman correlation to exceed 0.9 — this connects
the lab directly to the broader point that reward-model quality is bounded by preference-data
coverage, previewed again in Week 14's DPO-robustness open question.
