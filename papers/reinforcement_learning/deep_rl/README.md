# Deep RL

## [Off-Policy Deep Reinforcement Learning without Exploration](https://arxiv.org/pdf/1812.02900.pdf)
- Most immitation learning methods fail when exposed to suboptimal trajectories
- Their results show that off-policy agents perform much worse than behavioral agent when train with the same algorithm on the same data : Q: What does this mean??
- Extrapolation error: unseen state-action pairs are erroneously estimated to have unrealistic values
    - It can be attributed to a mismatch in the distribution of data induced by the policy and the distribution of data contained in the batch.
- Batch-constrained Q-learning: uses a state-conditional generative model to produce only previously seen actions. This generative model is combined with a Q-network, to select the highest valued action which is similar to the data in the batch.

