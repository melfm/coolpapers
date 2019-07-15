# Distribution Similarity, Similarity Metric

Many problems in testing and learning require evaluating distribution similarity in high dimensions.
The analysis of distributions is fundamental. Many algorithms rely on information theoretic approaches such as entropy, mutual information, or Kullback–Leibler divergence. However, to estimate these quantities,one must first either perform density estimation, or employ sophisticated space-partitioning/bias-correction strategies which are typically infeasible for high-dimensional data.

## [Masked Autoregressive Flow for Density Estimation](https://arxiv.org/pdf/1705.07057.pdf)

## [Neural Autoregressive Flows](https://arxiv.org/pdf/1804.00779.pdf)

## [Minimax Estimation of Maximum Mean Discrepancywith Radial Kernels](https://papers.nips.cc/paper/6483-minimax-estimation-of-maximum-mean-discrepancy-with-radial-kernels.pdf)

### Notes
- A family of divergences are the integral probability metrics, they find a witness function to distinguish samples from `P` and `Q`.
- The distance MMD measures is based on the notion of embedding probabilities in a reproducing kernel Hilbert space.
- Instead of density estimators, use means directly
-


## [Generative Models and Model Criticism via Optimized Maximum Mean Discrepancy](https://arxiv.org/abs/1611.04488)

### Notes


### Umbrella Techniques
- Jensen-Shannon divergence measure
- Kernel embedding of distributions

## [Metric Learning for Reinforcement Learning Agents](https://pdfs.semanticscholar.org/f3b5/689d80cf849e94bb41e9e7b05f2f552390ab.pdf)

### Notes
- Distance metric learning algorithm, the agent learns a metric that should generalize across the entire state space, NOT JUST the region explored.
- Goal : Scale and select state variables automatically from data gathered via agent experience.
- Three main steps :
    - Collect data while agent explores
    - Decide which states are `more similar` based on the transitions
    - Use state relatedness to calculate a distance metric, i.e. states which have similar transitions should be closer
    to states with dissimilar transitions.
#### Transition Similarity
- Learn a Mahalanobis distance function, parameterized by a positive semi-definite matrix `W` : `d(v, w) = (v - w)^T W(v - w)`
- Learning the distance function is learning the matrix `W`, and because its positive semi-definite, `W = G^T G` for some matrix `G`, and so the Mahalanobix distance function `d` would be the squared Euclidean distance after applying the transformation `G` to the data points.
