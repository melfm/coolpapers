# Model-Based RL

## [Temporal Difference Variational Auto-Encoder](https://arxiv.org/pdf/1806.03107.pdf)

### Notes
- An RL agent needs to build a representation of the uncertainty about the world, computed from the data gathered so far.
- To plan in a model-based fashion, agent needs to imagine distant futures which are consistent with his past. But planning step by step is too much.
- So we want:
    - Learn an abstract state representation, capable of making predictions at the state level, NOT just observation level.
    - Learn a belief state which contains all the information an agent has about the state of the world.
    - Model should exhibit temporal abstraction, by making 'jumpy' predictions, and learning from temporarly separated time points.
- They talk about the modelling options for construction of a latent state-space model such as autoregressive models and state-space models.
- Incorporate belief state which has often been restricted to the tabular case in RL.
- Belief-state-based ELBO
    - Sequential model that constructs a latent state-space and creates an online belief state.
- Jumpy state modeling
    - Look for models that can imagine future states directly, without going through all the intermediate states.
    - Lets call these temporal jumps.
    

