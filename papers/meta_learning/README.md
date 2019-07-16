# Meta-Learning


## [Meta-Gradient Reinforcement Learning](https://arxiv.org/abs/1805.09801)

### Notes
- Learn meta-parameters of a return function
- Using a differentiable `meta-objective`
- The meta-parameters `eta` being optimized, may be viewed as gates that cause the return to 
  - terminate (γ= 0)
  - bootstrap (λ= 0),
  - or to continue onto the next step (γ= 1 and λ= 1).
