# Week 6 Summary — The Bradley-Terry Model and the RLHF Pipeline

**Key takeaways:**
- The Bradley-Terry model derives $P(y_1\succ y_2\mid x)=\sigma(r(x,y_1)-r(x,y_2))$ from an
  odds-ratio assumption; fitting $r_\phi$ by maximum likelihood is exactly a logistic/cross-
  entropy loss over preference pairs.
- The RLHF pipeline has three stages: SFT (produces $\pi_{\mathrm{ref}}$), reward-model training
  (Bradley-Terry loss on preference pairs), and KL-regularized RL fine-tuning against the reward
  model.
- The KL penalty's coefficient $\beta$ trades off policy improvement against reward
  over-optimization ("reward hacking") of the imperfect, learned reward-model proxy; $\beta=0$
  removes the safeguard entirely.

**You should now be able to:** derive the Bradley-Terry formula and the reward-model loss;
state the three-stage RLHF pipeline and write the KL-regularized objective; explain concretely
why the KL term is necessary.

**Next week:** Preference-based alignment II — PPO for RLHF fine-tuning, built on the graduate
course's policy-gradient/actor-critic foundation, and Direct Preference Optimization derived as a
reparameterization that avoids training an explicit reward model.
