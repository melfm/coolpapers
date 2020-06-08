# Generalization, Transfer, Domain Rand

## [Invariant Causal Prediction for Block MDPs](https://arxiv.org/abs/2003.06016)
- Block MDPs : Families of environments with a shared latent state space and dynamics structure over the latent space, but varying observations. There exists some latent causal structure that is shared among all of the envs and the sources of variability between envs do not affect the reward, this family of envs is called a *Block MDP*.
- Model-irrelevance state abstractions (MISA) : generalizes to novel observations
- Using bisimulation and the minimal causal set of variables found by the algorithm, provide bounds on the model error and sample complexity

### Background
- Bisimulation relations are a type of state abstraction that offers a mathematically precise definition of what it means for two envs to share the same structure
    - Two states are bisimilat if they share the same expected reward and equivalent distributions over the next bisimilar states
- ICP (Invariant Causal Prediction) to find the causal feature set, the minimal set of featues which are causal predictors of a target variable
    - ICP removes irrelevant variables from the input, just as state abstractions remove irrelevant information from the envs observations
- IRM extends ICP by augmenting empirical risk minimization to learn a data representation free of spurious correlations
    - IRM learns a representation \phi for which the optimal linear classifier $w$ is invariant across $e$. This objective is a constrained optimization problem

- Relaxed block MDP: spurious variables may have different transition dynamics across the different envs so long as these correlations do not affect the expected reward
- Invariant causal prediction aims to identify a set S of causal variables such that a linear predictor with support on S will attain consistent performance over all envs

- Normal PAC generalization bounds require a much larger number of envs that one could expect to obtain in the RL setting.
- Instead of assuming access to a bisimilar MDP M', just provide discrepancy bounds for an MDP M^ produced by a learned state representation \phi(x), dynamics function f and rewward function $R$.

### Methods
- 1) ICP to select the causal variables in the state in the setting where variables are given - This corresponds to direct feature selection which with high probability
returns the minimal causal feature set.
- 2) Gradient-based similar to IRM objective, with no assumption of a linear causal relationship and a learned causal invariant representation - impose an additional
invariance constraint.

- Variable Selection for linear predictors : Take your replay buffer, tag the envs from where they come from. Apply ICP to find all causal ancestors of the reward iteratively.
- Learning model-irrelevance State Abstraction : Design an objective to learn a dynamics preserving state abstraction Z, where the similaity of the model is bounded by the model error
in the env.
- This requires disentangling the state space into a minimal representation that causes reward $s_t=\phi(x_t)$ and everything else $n_t=\varphi(x_t)$
- To incorporate a meaningful objective and ground the learned representation, a decoder is used

    
## [Invariant Policy Optimization: Towards Stronger Generalization in Reinforcement Learning](https://arxiv.org/pdf/2006.01096.pdf)

- Find a representation such that there exists an action-predictor built on top of this representation that is simultaneously optimal across all training domains
    - Find causes of successful actions
- A policy will generalize well if it exploits invariances resulting from causal relationships present across domains
- The PAC-Bayes Control, provides a way to make provable generalization guarantees under distributional shifts
    - They require an a prior bound on how much the test domain differs from the training domain in terms of f-divergence
 - Find features that are causally linked to a target variable by exploiting the invariance of causal relationships
- IRM Invariant risk minimization formualtes the problem in terms of finding a representation such that the optimal classifier built on top of this representation is invariance across domains
- This classifier ignores the spurious correlations, and requires a bilevel optimization problem and tackled via a regularizer that approximates its solution
- Treat each env corresponding to different interventions on the data-generation process that do not intervene on the target variable
    - there exists a classifier that is simultaneousl optimal for all training domains
 
## [Learning to See before Learning to Act](https://ai.googleblog.com/2020/03/visual-transfer-learning-for-robotic.html)

### Notes
- Model is trained on passive vision task and then adapted to perform manipulation tasks
- It is important to carefully select which parts of the pre-trained model to transfer
- Passive here means the data distribution is independent of the agent's decisions
- Make use of "affordance maps"
- Affordance of manipulation ?
    - TODO
- Very bad definitions of "backbone" and "heads"
    - backbone : Heavy backbone model for extracting visual features
    - head : lightweight vision head consisting of one or a few conv layers
- Input representation
    - RGB-D heightmap
- Visual prediction is a 2D heatmap that can represent a wide variety of vision tasks.
- Various vision tasks such as edge detection, segmentation etc are used
- They actually collect all this data through random exploring agent and label it using detectors ?
- They also try models trained with ImageNet and COCO
- Learning manipulation affordance
    - ConvNet and a motion primitive
    - Predicts a heatmap encoding probability of picking success at each pixel location
    - The motion primitive is a pre-defined function which controls the robot arm to perform manipulation task from a fixed initial arm position.
    - Motion primitive is open-loop with motion planning executed using a stable, collision free IK solver.
- I don't get these primitives?
- No RL here, just grabbing stuff using a planner.

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
