# Reward Shaping


## [Policy Invariance Under Reward Transformations: Theory and Application to Reward Shaping](https://people.eecs.berkeley.edu/~pabbeel/cs287-fa09/readings/NgHaradaRussell-shaping-ICML1999.pdf)

### Notes
- Policy invariance in shaping, where shaping must obey certain conditions to avoid convering to suboptimal policies.
- Reward for executing a transition between two states is the difference in the value of a `potential function` applied to each state.

## [Reinforcement Learning from Demonstration through Shaping](https://www.ijcai.org/Proceedings/15/Papers/472.pdf)

### Notes
- Sparse reward makes learning slow - instead incorporate demonstrations in the learning process as a potential-based reward shaping function.
- Define potential as a function of state-action pairs `(s, a)`. Such potential gives a high value when action `a` was demonstrated in a agent-state `similar` to that of the demonstrator state, and the potential is low when this action was not demonstrated within the `neighbourhood` of `s`.
- So need a similarity metric for state-action pairs.
- They use non-normalized multivariate Gaussian to calculate similarity.
- Assuming discrete actions, if two state-action pairs differ in the action, their similarity is zero.
- To calculate the potential, search through a set of demonstrations, find the sample with the same action that yields the highest similarity.

## [Principled Methods for Advising Reinforcement Learning Agents](http://cseweb.ucsd.edu/~ewiewior/03principled.pdf)

### Notes

## [Reward Shaping in Episodic Reinforcement Learning](https://pdfs.semanticscholar.org/41c5/0331ed5d3ffa51d879cc2e1a675c99445bc3.pdf)

### Notes

## [Reward Shaping via Meta-Learning](https://arxiv.org/pdf/1901.09330.pdf)

### Notes






