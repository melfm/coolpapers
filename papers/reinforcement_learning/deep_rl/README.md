# Deep RL

## [Off-Policy Deep Reinforcement Learning without Exploration](https://arxiv.org/pdf/1812.02900.pdf)
- Most immitation learning methods fail when exposed to suboptimal trajectories
- Their results show that off-policy agents perform much worse than behavioral agent when train with the same algorithm on the same data : Q: What does this mean??
- Extrapolation error: unseen state-action pairs are erroneously estimated to have unrealistic values
    - It can be attributed to a mismatch in the distribution of data induced by the policy and the distribution of data contained in the batch.
- Batch-constrained Q-learning: uses a state-conditional generative model to produce only previously seen actions. This generative model is combined with a Q-network, to select the highest valued action which is similar to the data in the batch.

## [Distributed Distributional Deterministic Policy Gradients]
- N-step returns and prioritized experience replay
- Lets get back to the problem of DQN - the policy is implicitly defined in terms of its value function and actions are selected by maximizing this function.
    - In continuous control of course this would require a costly optimization step or discretization of the action space.
    - What we end up doing is, parameterize the policy *explicitly* and directly optimize the long term value of following this policy.
- Distributional version of the critic update - Such distributions model the randomness due to intrinsic factors.
- Because DDPG is off-policy, its nice because you can run many actors in parallel all feeding into the same buffer.
- What's interesting about Deterministic Policy Gradient is that, the form of gradient, does not require integrating over the action space and so requires less samples to learn.
- Recall that by letting the behaviour policy different from $\pi$ we are able to empirically evaluate the gradient of the objective (Think about the parameterized Q and taking gradient w.r.t. to $\pi$) - And remember that we also parameterize the Critic
- Distributional critic was introduced by Bellemare (2017).
    - To define this update, lets revisit our expectation thingy expression in terms of a random variable $z_{\pi}$, such that $Q_{\pi}(x,a) = \mathbb{E}z_{\pi}(x,a)$
        - $(\tau_{\pi} Z)(x,a) = r(x,a) + \gamma \mathbb{E} [Z(x', \pi(x'))) | x,a]]$
    - This equality is w.r.t. the probability of the random variables and the expectation is taken w.r.t. to distribution $Z$ as well as the **transition dynamics**.
    - As usual we also parameterize this distribution so now we will have $Z_{\omega}$
    - We also need to define some sort of loss, so before we used L2 norm (remember our TD update stuff) - now we take some distance metric which is a measure between two distributions.
    - You are gonna end up replacing the Q with an expectation over this distribution and of course the outer expectation will be a sampled based approximation.
- Now $N-$step return : So instead of one step you do n-step update, thats it
- For prioritized replay, you can use absolute TD-error to sample from replay as means of measuring priority? So I dont know you take something with higher error ?
- Correlated noise drawn from Ornstein-Uhlenbeck (whats that?) - It was found to be unneccessary
- 
