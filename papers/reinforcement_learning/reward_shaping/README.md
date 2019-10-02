# Reward Shaping

Overview: Provide the learner with supplemental rewards that encourages progress towards highly rewarding states in the environment.
If you design this arbitrarily, they can distract the learner from the intended goal.

## [Policy Invariance Under Reward Transformations: Theory and Application to Reward Shaping](https://people.eecs.berkeley.edu/~pabbeel/cs287-fa09/readings/NgHaradaRussell-shaping-ICML1999.pdf)

### Notes
- Policy invariance in shaping, where shaping must obey certain conditions to avoid converging to suboptimal policies.
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


## [Potential-Based Shaping and Q-Value Initialization are Equivalent](https://arxiv.org/pdf/1106.5267.pdf)

### Notes
- They show that the effects of potential-based shaping can be achieved by initializing a learner's Q-values with the state potential function.
- Let the initial values of learners $L$'s Q-table be $Q(s,a) = Q_0(s,a)$
- $L'$ will be initialized to $Q'_0(s,a) = Q_0(s,a) + \Phi(s)$
- Both learners' Q-values are updated based on an experience using the standard update rule for Q-learning.
- $Q(s,a) = Q(s,a) + \alpha(r + F(s,s') + \gamma max_{a'}Q(s',a') - Q(s,a))$
-$Q'(s,a) = Q'(s,a) + \alpha(r + \gamma max_{a'}Q'(s',a') - Q'(s,a))$
- They prove the theorem : Given the same sequence of experiences during learning, $\Delta Q(s,a)$ always equals $\Delta Q'(s,a)$
- Examine the update performed on $Q(s,a)$ in the presence of potential-based shaping reward:
- $\delta Q(s,a) = r + F(s,s') + \gamma max_{a'}Q(s',a') - Q(s,a)$
     $= r + \gamma \Phi(s') + \gamma max_{a'}(Q_0(s',a')+ \Delta Q(s',a')) - Q_0(s,a) - \Delta Q(s,a)$
- Now the update performed on $Q'$:
- $\delta Q'(s,a) = r + \gamma max_{a'}Q'(s',a') - Q'(s,a)$
- $= r + \gamma max_{a'}(Q'(s',a') - Q'(s,a)$
- $= r + \gamma max_{a'}(Q_0(s',a') + \Phi(s') + \Delta Q(s',a')) - Q_0(s,a)- \Phi(s) - \Delta Q(s,a)$
- $= r +  \gamma \Phi(s') - \Phi(s) + \gamma max_{a'}(Q_0(s',a') + \Delta Q(s',a')) - Q_0(s,a) - \Delta Q(s,a)$
- $\delta Q(s,a)$
- Both Q-tables are updated by equal values of $\Delta Q(.)$ and $\Delta Q'(.)$
- Think about it in terms of how a learner chooses actions.
- Most policies are defined in terms of the learner's Q-values.
- We define an *advantage based policy* as a policy that chooses an action in a given state with a probability that is determined by the differences of the Q-values for that state.
- So if some constant is added to all the Q-values, the probability distribution of the next action will not change.
- Theorem $2$ : If $L$ and $L'$ have learned on the same sequence of experiences and ue an advantage-based policy, they will have an identical probability distribution for their next action.
- Recall how Q-values are defined again:
- $Q(s,a) = Q_0(s,a) + \Delta Q(s,a)$
- $Q'(s,a) = Q_0(s,a) + \Phi(s) + \Delta Q'(s,a)$
- We already proved $\Delta Q(s,a)$ and $\Delta Q'(s,a)$ are equal if they have been updated with the same experience.
- So the only difference between the two Q-tables is the addition of the state potentials in $Q'$.
- Because this addition is *uniform* across the action in a given state, it does not affecf the policy.
- Final notes:
    - Initial Q-values have a large influence on the efficiency of RL for goal directed tasks
    - An agent must find a goal state at least once during exploration before an optimal policy is found
    - With Q-values initialized below their optimal value, an agent may require learning time exponential in the state action space
    - Because potential-based shaping is equivalent to Q-value initialization, care must be taken in choosing a potential function that does not lead to poor learning performance.







