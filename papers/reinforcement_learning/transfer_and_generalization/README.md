# Generalization, Transfer, Domain Rand

## [Sim-to-Real Transfer with Neural-Augmented Robot Simulation](http://proceedings.mlr.press/v87/golemo18a.html)

### Notes
- Use data collected from a real robot to train an RNN to predict the discrepancies between simulation and the real world.
- Since data is collected using non-task-specific policy, it can be used to learn policies related to different tasks. In other words, at this stage, we only care about using the data to learn discrepancy between simulated and the real world.
- Once you have a model that provides you with corrections between real and sim states, it can be combined to learn a policy.
- To learn the polic now, at each time step the current state transition in the source domain is passed to LSTM model, lets call it $\phi$ to compute an estimate of the state in the target env, an action is then chosen according to this estimated state.

## [Robust Domain Randomization For Reinforcement Learning](https://openreview.net/pdf?id=H1xSOTVtvH)
- Domain adaptation techniques aim to update the data distribution in simulation to match the real distribution through some canonical mapping or using regularization methods.
- Propose a method for learning policies that are robust to changes in the randomization space, producing agents that ignore irrelevant aspects of the environment.
- This works combines aspects from both domain adaptation and domain randomization
- Lipschitz constant of the agent's policy over the randomization parameters provides an upper bound on the agent's robustness to variations in the env
- The agent is trained on one variation of the env but its learned representations are regularlized so that the Lipschitz constant is minimized.
- Lets distinguish between two types of domain randomization: visual randomization, and dynamics randomization. This paper focuses on visual
    - In the context of domain rand, when the randomization parameters affect the transitions in the MDP, we call it dynamics randomization.
    - And when the randomization parameters affect only the states, we refer to visual randomization.
- Previous work proposed to add constraints to encourage networks to learn similar embeddings for samples from both a simulated and a target domain
    - Or adversarial loss to train RL agents in such a way that similar policies are learned in both a simulated domain and the target domain.
