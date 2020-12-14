# Representation Learning for RL

## Overview
Summary of papers related to representation learning and state abstraction in RL.

## [Input Generalization in Delayed Reinforcement Learning](https://www.ijcai.org/Proceedings/91-2/Papers/018.pdf)
- Input generalization problem, the system must generalize to produce similar actions in similar situations.
- G algorithm -> Based on recursive splitting of the state space based on statistical measures of differences in reinforcements received? Does this mean reward?
- What is connectionist backpropagation? They seem to be comparing their method to backpropagation?
- When the environment an RL agent was trained in changes slightly, these methods fail gracelessly.
- What makes it hard?
  - Some states are rare its difficult to gain enough experience with them
  - Somehow a learning algorithm must guess the value of states based on experience with similar states
  - Generalization is a large field of study, but RL puts special constraints on the problem
- The G algorithm
  - Addresses the problem of input generalization in Q-learning
  - Collapse the exponential sized Q-table where in most domains, large chunks of the table should have identical entries, because many of the bits will be irrelevant for acting under certain situations.
  - Incrementally builds a tree-structured Q-table. Begin by assuming all input bits are irrelevant, collapsing the entire table into a single block.
  - Collect Q-values within this block
  - Collect statistical evidence for the relevance of individual bits.
  - When it sees a relevant bit, it splits the state space into the two subspaces corresponding to the relevant bit being on and off. These blocks are recursively split giving rise to a tree structured Q-table.
  - The system then acts on the basis of the Q statistics in the leaf node corresponding to an input.
  - Think of tree as a boolean input classification network.
- This only works in an environment where the relevance of bits is apparent in isolation.
- This fails if groups of bits are collectively relevant but individuall irrelevant.
- This relates to other inducing decision tree methods such as ID3 and CART [see paper]
- This method applies to input generalization, but what about the output side?
  - If the number of actions is very large, may not be possible to try them all in every state.
- They also have this statistical mechanism for separating the rewards. They call this D-learning and say its better because it keeps track of more distinctions.
- How do they do the bit relevance test?
  - Students t-test to determine when a bit is relevant
  - The t test tells you guven two sets of data, how probable it is that distinct distributions gave rise to them
  - Two sorts of relevance
    - a bit may be relevant because it affects the value of a state or because it affects the way the system should act in that state.
    - Two sorts of statistics are used to determine value, corresponding to the mean immediate value of the state and its mean discounted future value.
    - Both are needed, immediate value to bootstrap the process by recognizing the states that themselves give large reinforcements & discounted value used to find states that lie on a path toward externally reinforced states.
    - For each bit in each state block, G keeps track of the immediate and discounted values of the state block subdivided by the bit being on and off and compares these values with the t test.
  - A bit may be relevant because it affects how the agent should act.
  - To discover relevabce, G keeps track for each action in each block state, of the discounted value of taking the action in that state block when the bit is on versus off and compares with the t test.
  - When a bit is shown to be relevant in a block, that block is split on the bit. When a block is split, all discounted statistics are reinitialized to zero.
  - Note something: Before the split high-valued subblock is invisible and the estimated values of all states that can transition to that subblock will be low. Throwing away all experiences accumulated is drastic. They are exploring ways to avoid this. I didn't understand what is being thrown away here?
- The t test depends on the assumption that items sampled are distributed normally. Of course in real life this is violated because again some interesting events occur rarely (like killing a ghost)
- Central limit theorem: States that the sum of a set of values from an arbitrary distribution will approach normality as the number of samples increases.
- Low frequency noise?
- It lerans many times slower than Q because it has to find the right splits before learning a policy.
- Other methods for input generalization in RL:
  - Watkins [1989] used the CMAC algorithm
- They also talk about ``backpropagation is ill-characterized; it is impossible to know how the algorithm will perform when presented with a complex problem because it often converges to bad local minima.`` What do they mean?
  - ``perceptron convergence theorem, there should be no local minima to fall into``

- Cross references:
  - Watkins [1989] used the CMAC algorithm

## [Feudal Reinforcement Learning](http://www.cs.toronto.edu/~fritz/absps/dh93.pdf)
- Speed up RL to enable learning to happen simultaneously at multiple resolutions in space and time.
- Q-learning managerial heirarchy in which high level managers learn how to set tasks to their sub-managers who learn how to satisfy them.
- The system learns to explore more efficiently than standard flat Q-learning and builds a more comprehensive map.
- Some bottlenecks are
  - temporal resolution
  - Finding smaller subtasks that are easier to solve
  - Exploration
  - Structural generalization
- An appropriate heirarchy can address some of these bottlenecks, higher level managers should sustain a larger grain of temporal resolution
- Exploration for actions leading to rewards can be more efficient since it can be done non-uniformly. Higher level managers can decide that reward is best found in some other region of the state space and send the agent there directly
- Two principles:
  - Reward Hiding: Managers must reward sub-managers for doing their bidding
    - Sub-managers should learn to obey their managers and leave it up to them to determine what is best to do
  - Information Hiding: Info hidden both downwards- sub-managers dont know the task he super-manager has set  for the manager. And upwards- a super-manager doesnt know what choices its manager has made to satisfy commands.
- Each manager maintains Q-values over the actions it instructs its sub-managers to perform based on the location of the agent at the subordinate level of detail and the command it has received from above.
- When the agent starts, actions at successively lower levels are selected using Q-learning softmax method and the agent moves according to the finest grain action (talking about levels here).
- Typical pattern that emerges, low level Q values embody an implicit knowledge of how to get around the maze, so the feudal system can explore efficiently once it (slowly) learns not to search in the original place.

- Cross references:
  - RL & backgammon (Tesauro, 1992)
  - Singh’s (1992b) variable temporal resolution system

## [Reinforcement Learning with Selective Perception and Hidden State (Chapter 4)](https://web.media.mit.edu/~tristan/Classes/MAS.945/Papers/Contextual/McCallum_Thesis.pdf)
[Chapter 4]
- Recall the definition of ``Markov with respect to reward:`` a state is Markov with respect to reward if knowledge of past states does not help predict future reward from that state.
- On the other hand, if there is statistically significant difference in future discounted reward between statistics depending on where the agent came from, then knowledge of which state the agent came from does help predict reward, and the state should be split.
- What are state distinctions necessary for calculating the optimal policy?
- Maze Example
- Distinguishing between states that have different policy actions is necessary, but the example proof shows its not sufficient. The agent must also distinguish states with different utilities (means reward right?)
- The utile distinction test distinguishes states that have different policy actions or different utilities, and merges states that have the same policy action and same utility.
- When the agent is not given a model of the environment, it will have to estimate a model using its experience. A statistical test will be needed to determine if the data is sufficient to claim statistical significance in utility differences.
- TODO: UDM Algorithm


- Cross references:
  - Kaelbling [1995]
  - Baum-Welch

## [Chattering in SARSA(lambda)](http://citeseerx.ist.psu.edu/viewdoc/download?doi=10.1.1.35.325&rep=rep1&type=pdf)
- They demonstrate the `chattering' phenomenon which happens with SARSA in the case where Q-function is approximated by state aggregation. The example MDP problem is as follows: From the starting state, the agent picks either upper or lower arc to traverse through, but from then on its decisions are forced to a final absorbing state. Along the way of going through each arc, the agent faces 0 cost, but from upper arc to the final state a cost of 2 occurs and from the lower arc to the final state a cost of 1 occurs.
- The agent is following a $$\epsilon$$-greedy policy with constant value of $\epsilon=10\%$ of picking suboptimal actions. When, the agent picks the upper arc, the cost-2 arc, i.e. upper arc into the goal will be followed more often and the learned Q values will increase. On the other hand, when the lower path appears more desirable, then the cost-1 arc will be weighted more and Q values will decrease. And this cycle repeats, where the learned Q values oscillate forever.
- This failure occurs in this case due to the usage of state aggregation and therefore one can conclude that SARSA with function approximation are prone to this type of failure modes.
If we use a powerful function approximation to represent every Q function exactly then, it would overcome this issue.
- And the reason that $Boltzmann$ exploration can help is because, it uses knowledge from the Q function, given an appropriate temperature parameter $\beta$ is selected, it has a smoothing effect over the action selection probability since it takes into account weighting of all actions according to their values.
Also with $\epsilon$-greedy style of policy the probability of visiting a given state changes discontinuously when the Q function fluctuates, but a $Boltzmann$ policy is a continuous function w.r.t. the Q function.

## [Predictive representations of state](https://web.eecs.umich.edu/~baveja/Papers/psr.pdf)
- States of a dynamical system can be represented by multi-step, action-conditional predictions of future observations.
- k-order Markov models?
  - History-based approach, uses simple functions of past observations as states
- The model's internal state gives it temporally unlimited memory-the ability to remember an event that happened arbitrarily long ago.
- State representation can be a relatively simple record of the stream of past actions and observations.
- You could say, POMDP learning algorithms encounter many local minima and sanddle points because all their states are equipotential.
- PSR is like the generative-model approach in that it updates the state representation recursively.
- A history based representation looks to the past and records what did happen, PSR looks to the future and represents what will happen.

- Cross references:
  - If important aspects of the dynamics are genuinely unknown, then these methods are rarely effective (e.g., Shatkay & Kaelbling, 1997).
  - McCallum (1995) has shown in a number of examples that sophisticated history-based methods

## [MAXQ-Q learning](https://papers.nips.cc/paper/1999/file/e5a4d6bf330f23a8707bb0d6001dfbe8-Paper.pdf)
- See physical notes. Also related [Hierarchical Reinforcement Learning with the MAXQ Value Function Decomposition](https://arxiv.org/pdf/cs/9905014.pdf)

## [Towards a unified theory of state abstraction for MDPs](http://anytime.cs.umass.edu/aimath06/proceedings/P21.pdf)
- State abstraction (or state aggregation): instead of working in the ground state space, the agent finds solutions in the abstract state space much faster by treating groups of states as unit by ignoring irrelevant state information.
- Abstractions allow the agent to distinguish relevant information from irrelevant information.
- Different types of state abstraction
  - Bisimulation
  - Homomorphism
  - Utile Distinction
  - Policy irrelevance
- How does a solution to the abstract MDP relate to the ground MDP? What guarantees can be made?
- What information is lost when an abstraction is applied?
- Bisimulation, where states with the same transition and reward functions are aggregated. Given a flat representation of an MDP, this partitioning can be accomplished in polynomial time.
- Several algorithms can be used to aggregate states between iterations of model-based planning algorithms like value iteration or policy iteration.
- MAXQ heirarchy aggregates states within a subtask only if their reward and transition functions are the same for any policy consistent with the heirarchy.
- Using statistics to do the abstraction online was a feature of G-algorithm, which aggregates states with the same reward and Q-values for each action.
- Abstraction is in general a mapping from one problem representation to a new representation, while preserving some properties. So preservation of properties that are needed for an agent to make decisions that lead to optimal behavior.
  - For example, bisimulation preserves  the one-step model of an MDP (i.e. the transition and reward functions) while policy-irrelevance types of abstraction preserve the optimal actions.
- Five types of abstractions (there are many ways to partition the state space)
  - Em see the paper? :P
- Lots of proof sketches in this paper.

- Cross references:
  - Ravindran et al. examined state aggregation based on homomorphisms of the model, rather than strict action matching.

## [State representation learning for control](https://arxiv.org/pdf/1802.04181.pdf)
- Representation learning algorithms are designed to learn abstract features that characterize data.
- SRL focuses on learning features in low dimension, evolving through time and influenced by actions of agent.
- Such representation is learned to capture the variation in the environment generated by the agent's actions which is particularly suitable for robotics and control.
- The low dimension characteristic help to overcome the curse of dimensionality
- This paper reviews SRL methods that involve interaction with the environment
- How generic learning objectives are differently exploited in these scenarios.
- In robotics and control, we care about compact and expressive representations of sensor data.
- Deep learning now provides a general framework to learn such representations from data.
- This is particularly interesting in robotics where multiple sensors can provide high dimensional as well as low dimensional data.
- The low dimensional representation is called the state of the system 
  - Such state representation is at the basis of the classical RL framework.
- Deep RL has shown that it is posible to learn controllers directly from high-dimensional observations.
- Set generic constraints on what a good state representation should be.
- Feature learning is a wide domain which aims at decomposing data into different features that can faithfully characterize it.
- The objective of SRL is to take advantage of time steps, actions, maybe rewards, to transform observations into states: a vector of a reduced set of the most representative features that is sufficient for efficient policy learning.
- Distinguish between passive feature learning, learning on data that is only observed vs active feature learning where the agent has the opportunity to exploit more possibilities to learn better representation by balancing between exploration and exploitation.
- Mapping $\phi$ of the history of observation to the current state, where mapping is learned through proxy objectives without access to the true state. 
- General idea of state representation, encoding essential information while discarding irrelevant aspects of the original data.
- What is a good state representation?
  - Markovian : it summarizes all the necessary information to be able to choose an action by looking at only the current state
  - Able to represent the true value of the current state well enough for policy improvement
  - Generalize to learned value-function to unseen states with similar futures
  - Low dimensional for efficient estimation.
- State representation learning can also be linked with the idea of learning disentangled representations that clearly separate the different factors of variation with different semantics.
- Good rep -> Sufficient, efficient, easy to work with (e.g. factorizing the data-generating factors), minimalistic.
  - minimal assumption comparable to the simplicity prior (Occam's razor)
- Ideas relating to autoencoders with a siamese encoder projecting sequences of images into a state representation space S with constraints on the transition between $s_t$ and $s_{t+1}$ to be linear.
  - Using AE to learm a projection into a state space where transitions are assumed to be linear etc
  - Embed to Control (E2C) deep generative model that learns to generate image trajectories from a linear latent space.
- Deep Variational Bayes Filters (DVBF) are an extension of Kalman filters which learn to reconstruct the observation based on a nonlinear state space using variational inference
- Temporal dynamics: Force states to efficiently encode the info necessary to predict the next state
- So when they predict future observations to learn representation they belong to the family of forward models.
- Intrinsic curiosity model (ICM) as an intrinsic reward signal computed from the forward models' loss function where the forward model is learned by an encoder.
- Forward models are able to learn representations of controllable factors: in order to predict next state, the model must understand the object being controlled.
- Rewards are helpful to disentangle meaningful information from a noisy or distracting one, and to tie the representation to a particular task.
- Example priors:
  - Slowness principle - interesting features fluctuate slowly and continuously
  - Variability - 

- Cross references:
  - [Böhmer et al., 2015] survey approaches that learn low dimensional Markovian representations without
direct supervision.
  - reward weighted principal component analysis (rw-PCA) [Parisi et al.,2017]

## [A geometric per-spective on optimal representations for reinforcement learning](https://arxiv.org/pdf/1901.11530.pdf)
- Representation learning based on geometric properties of the space of value functions.
- Value functions as auxiliary tasks. Linear approximation of the value function of all stationary policies for a given env.
- Special class of value functions adversarial value functions (AVFs).
- Most deep RL methods augment their main objective with additional losses called auxiliary tasks.
  - With the aim of facilitating and regularizing the representation learning process.
- Formulate an optimization problem whose solution is a form of optimal representation.
- State representation from which one can best approximate the value function of any stationary policy
- Using predictions as auxiliary tasks can be interpreted as a relaxation of optimization

## [State Abstraction Discovery from Irrelevant State Variables](https://www.cs.utexas.edu/~pstone/Papers/bib2html-links/IJCAI05.pdf)

## [SMDP Homomorphisms: An Algebraic Approach to Abstraction in Semi-Markov Decision Processes](https://www.cse.iitm.ac.in/~ravi/papers/IJCAI03.pdf)

## [Learning State Abstractions for Transfer in Continuous Control](https://arxiv.org/pdf/2002.05518.pdf)

## [Improving Generalisation for Temporal Difference Learning: The Successor Representation](http://www.gatsby.ucl.ac.uk/~dayan/papers/d93b.pdf)


## [A neurally plausible model learns successor representations in partially observable environments]()


## [Successor Features for Transfer in Reinforcement Learning](https://arxiv.org/pdf/1606.05312.pdf)

## [Advantages and Limitations of using Successor Features for Transfer in Reinforcement Learning](https://arxiv.org/pdf/1708.00102.pdf)