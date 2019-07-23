# Meta-Learning

## [Model-Agnostic Meta-Learning for Fast Adaptation of Deep Networks](https://arxiv.org/abs/1703.03400)

### Notes
- The model parameters are explicitly trained such that a small number of gradient steps with a small amount of training data from a new task will give good generalization.
- Building internal representation that is broadly suitable for many tasks, fine-tuning the parameters slightly, gives good performance.
- Maximizing the sensitivity of the loss functions of new tasks with respect to the parameters. When the sensitivity is high, small local changes to the parameters can lead to large improvements.
- Consider a distribution over tasks $p(\tau)$, you train from samples drawn from $\tau_i$ but you also test on new samples - the test error serves as the training error of the meta-learning process.
- Meta-optimization is performed over the model parameters $\theta$, whereas the objective is computed using the updated model parameters $\theta'$.
- The meta-gradient update involves a gradient through a gradient. Computationally, this requires an additional backward pass through $f$ to compute the Hessian-vector products.

## [Meta-Gradient Reinforcement Learning](https://arxiv.org/abs/1805.09801)

### Notes
- The discount factor $\lambda$ determines the time-scale of the return. $\lambda=1$ provides a long-sighted goal that accumulates rewards far into the future, $\lambda=0$ provides a short-sighted goal prioritising short-term rewards.
- The return may also be bootstrapped at different time horizons. An $n$ step return accumulates rewards over $n$ time-steps and then adds the value function at the $n$th returns.
- Together these two trade-off bias and variance.
- Learn meta-parameters of a return function
- Using a differentiable `meta-objective`
- The meta-parameters `eta` being optimized, may be viewed as gates that cause the return to 
  - terminate (γ= 0)
  - bootstrap (λ= 0),
  - or to continue onto the next step (γ= 1 and λ= 1).
