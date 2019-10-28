# Generalization & Transfer

## [Sim-to-Real Transfer with Neural-Augmented Robot Simulation](http://proceedings.mlr.press/v87/golemo18a.html)

### Notes
- Use data collected from a real robot to train an RNN to predict the discrepancies between simulation and the real world.
- Since data is collected using non-task-specific policy, it can be used to learn policies related to different tasks. In other words, at this stage, we only care about using the data to learn discrepancy between simulated and the real world.
- Once you have a model that provides you with corrections between real and sim states, it can be combined to learn a policy.
- To learn the polic now, at each time step the current state transition in the source domain is passed to LSTM model, lets call it $\phi$ to compute an estimate of the state in the target env, an action is then chosen according to this estimated state.