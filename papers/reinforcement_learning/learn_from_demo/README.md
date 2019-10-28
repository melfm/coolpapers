# Learning From Demonstration

## [Deep Q-learning from Demonstrations](https://arxiv.org/pdf/1704.03732.pdf)

### Notes
- DQfD : Prioritized replay queue, combines temporal difference updates with supervised classification of demonstrator's actions
- Outperforms Prioritized Dueling Double Deep Q-nets (PDD DQN)
- When you separate these value functions for these variables, i.e. current state and next state, while acting greedily w.r.t. action your main network is telling you to do, evaluated using your target network, essentially reduces the upward bias (whatever that means? ) that is created with regular Q-learning updates. 
- **Prioritized Experience Replay** modifies the DQN agent to sample more important transitions from its replay buffer more frequently. The priority is calculated from the last TD error for the transition. Remember, to account for the change in the distribution, updates to the network are weighted with importance sampling.
- Recall, DAGGER (iteratively produce new policies based on polling the expert outside the original state space - requires the expert to be available during training.), Deeply AggreVaTeD (extends DAGGER with DNN and continuous action space), zero-sum game (learner chooses a policy and the adversary chooses a reward function.)
- DQfD combines TD and classification losses in a batch algorithm in a model-free setting
- Their agent is also pre-trained on the demonstration data and the batch of self-generated data grows over time and is stored in the experience replay.
- **AlphaGo** also first trains a policy network from a dataset of 30 million expert actions. It then uses this as a starting point to apply policy gradient updates during self-play, combined with planning rollouts.
- **Human Experience Replay (HER)** agent samples from a replay buffer that is mixed between agent and demonstration data.

#### Deep Q-learning from Demo
- Pre-train your agent guy to learn to imitate the demonstrator with a value function that satisfies the Bellman equation so that it can be updated with TD updates once he startes interacting with the environment
- Network is updated by applying *4* losses :
  - 1-step double Q-learning loss
  - $n-$ step double Q-learning loss
  - supervised large margin classification loss
  - $L2$ regularization loss on the network weights and biases
- Again, the supervised loss is for clasification of the expert actions, Q-learning loss ensures the network satisfies the Bellman equation and can be used as a starting point for TD learning.
- Now lets think about this - The demo data covers a narrow part of the state space, and its not even taking all possible actions. So many state-actions have never been taken, so no data to ground them to realistic values.
  - If you were to pre-train the network with only Q-learning updates, towards the max value of the next state, the network would update towards the highest of these **ungrounded variables**.
So they add a large margin classification loss:
  - J(Q) = max[Q(s,a) + l(a_e, a)] - Q(s, a_e)   where a_e is the actin the expert took in state $s$ and l(a_e, a) is a margin function, so its 0 when a=a_e, and positive otherwise.
  - This forces the values of the other actions to be at least a margin lower than the value of the demonstrator's action.
  - Think of this as grounding the values of the unseen actions to reasonable values.
 - So now if you were to only use this loss, there would be nothing constraining the values between consecutive states and the Q-network would not satify Bellman eq.
- So adding $n-$step returns helps propagate the values of the expert's trajectory to all the earlier states. This helps you with pre-training. Now visualize the n-step return (forward view).
- Finally, $L2$ regularization helps with overfitting to the demo data.


## [Overcoming Exploration in Reinforcement Learning with Demonstrations](https://arxiv.org/pdf/1709.10089.pdf)


## [Leveraging Demonstrations for Deep Reinforcement Learning on Robotics Problems with Sparse Rewards](https://arxiv.org/pdf/1707.08817.pdf)



## [Reinforcement Learning from Demonstration through Shaping](https://www.ijcai.org/Proceedings/15/Papers/472.pdf)
- Incorporate demonstration data as a shaping potential
- Using a multivariate Gaussian to calculate similarity between state-action pairs.
- Originated from HAT.


## [Principled Methods for Advising Reinforcement Learning Agents](http://cseweb.ucsd.edu/~ewiewior/03principled.pdf)
- Proof of state-action potential shaping function for MDP.


## [Reinforcement and Imitation Learning for Diverse Visuomotor Skills](https://arxiv.org/abs/1802.09564)
- The policy takes both an RGB camera observation and a  proprioceptive feature vector that describes the joint  positions and angular velocities.
- You then throw CNN, MLP, LSTM, GAIL and PPO into the soup!
- Solved simple block stacking task etc. Stacking is only one level though.






