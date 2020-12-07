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

## [Feudal Reinforcement Learning](http://www.cs.toronto.edu/~fritz/absps/dh93.pdf)
- Speed up RL to enable learning to happen simultaneously at multiple resolutions in space and time.
- Q-learning managerial heirarchy in which high level managers learn how to set tasks to their sub-managers who learn how to satisfy them.
- The system learns to explore more efficiently than standard flat Q-learning and builds a more comprehensive map.
- Some bottlenecks are
  - temporal resolution
  - Finding smaller subtasks that are easier to solve
  - Exploration
  - Structural generalization




- Other references:
  - RL & backgammon (Tesauro, 1992)




