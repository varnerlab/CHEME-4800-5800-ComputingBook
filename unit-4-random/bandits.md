# Bandit Problems and Reinforcement Learning

```{topic} Outline
In this lecture, we will delve into the problem, covering different algorithms and strategies for balancing exploration and exploitation, and explore the following topics:

* [Bandit problems](content:references:multiarm-bandit-problems) refer to a scenario where an agent must repeatedly choose between multiple actions, each with an unknown reward distribution. The goal of the agent is to maximize its total reward over time while learning the reward distributions of the different actions. This presents a tradeoff between exploration, where the agent chooses the action with the highest estimated reward so far, and exploitation, where it selects an action with an uncertain reward to learn more about its distribution. 

* [Reinforcement Learning (RL)](content:references:basic-concepts-reinforcement-learning) agents learn by performing actions in the world and then analyzing the rewards they receive. Reinforcement learning differs from other machine learning approaches, e.g., supervised learning, because labeled input/output pairs, e.g., what actions lead to positive rewards are not presented to the agent _a priori_. Reinforcement learning agents learn what is good or bad by trying different actions.
```


---
<!-- 
Reinforcement learning agents must balance exploration of the environment, e.g., taking random actions with exploiting knowledge already obtained through interacting with the world. Pure exploration allows agents to construct a comprehensive model of their environment, but likely at the expense of positive reward. On the other hand, pure exploitation has the agent continually choosing the action it thinks best to accumulate reward, but different, better actions could be taken. 

A classic approach to understanding the exploration and exploitation tradeoff, which itself has many real-world applications, is the [multi arm bandit problem](https://en.wikipedia.org/wiki/Multi-armed_bandit). -->

(content:references:multiarm-bandit-problems)=
## Bandit problems
The [multi-armed bandit problem](https://en.wikipedia.org/wiki/Multi-armed_bandit) is a fundamental problem in machine learning and decision-making. It refers to a scenario where an agent must repeatedly choose between multiple actions, each with an unknown reward distribution. Each action can be considered a slot machine, or a _one-armed bandit_, with potentially different payout distributions.

The goal of the agent is to maximize its total reward over time while learning the reward distributions of the different actions. This presents a tradeoff between exploitation, where the agent chooses the action with the highest estimated reward so far, and exploration, where it selects an action with an uncertain reward to learn more about its distribution. The multi-armed bandit problem has practical applications in various fields, such as clinical trials, online advertising, and recommender systems.

### Bandit problem formulation
A general bandit problem is a sequential game played between an agent and the environment. The game is played over $n$ rounds called the _horizon_. In each round the agent chooses an action $a\in\mathcal{A}$ and the environment then reveals a reward $r$. Let's look at a particular sub-type of multi-arm bandit problem, namely, the Bernoulli bandit problem and Thompson sampling, a particular solution approach to this class of problem {cite}`RUSSO-2017-TS`:

````{prf:definition} Binary Bernoulli bandit problem
:label: defn-multi-arm-bernoulli-bandit-problem
A Binary Bernoulli bandit problem is a type of multi-armed bandit problem where each action has a binary reward distribution with an unknown probability of success. In this problem:
* An agent has a choice between $K$ possible actions $\mathcal{A}=\left\{a_{1},a_{2},\dots,a_{K}\right\}$ where the success or failure of any action $a_{i}\in\mathcal{A}$ is governed by a Bernoulli random variable (see {prf:ref}`defn-pmf-bernouli-random-variable`). 
* The success probabilities of any action $p_{a_{i}}$, while static over the horizon of the game, are unknown to the agent, but they can be learned by experimentation. 
* The agent receives a reward of $R_{a_{i}} = 1$ (success) with probability $p_{a_{i}}$, and $R_{a_{i}} = 0$ (failure) otherwise. 

The agent’s objective is to maximize the cumulative number of successes over the game horizon, e.g., T-periods, where $T\gg{K}$. 
````

There are multiple heuristic strategies to solve the Binary Bernoulli bandit problem described in {prf:ref}`defn-multi-arm-bernoulli-bandit-problem`:

* [Thompson sampling](https://en.wikipedia.org/wiki/Thompson_sampling) which is shown in {prf:ref}`algo-thompson-sampling` consists of choosing the action that maximizes the expected reward with respect to a randomly drawn belief, where the belief distribution follows a [Beta distribution](https://en.wikipedia.org/wiki/Beta_distribution). 

```{prf:algorithm} Beta-Thompson-sampling
:class: dropdown
:label: algo-thompson-sampling

**Inputs**  number of time periods in the horizon $T$, the dimension of the action space $K = |\mathcal{A}|$, and
$\alpha_{1},\dots,\alpha_{K}$ and $\beta_{1},\dots,\beta_{K}$ the initial values (prior) for the parameters that appear in the 
Beta-distribution for each action 

**Outputs** posterior values for the parameters $\alpha_{1},\dots,\alpha_{K}$ and $\beta_{1},\dots,\beta_{K}$ that appear in the Beta-distributions for each action

**Initialize**
a Beta-distribution for each action using initial values $(\alpha,\beta)$.

**Main**
1. for t $\in$ 1 to T
    1. for k $\in$ 1 to K
        1. sample $\hat{\theta}_{k} \sim \beta(\alpha_{k}, \beta_{k})$
    

    1. Select action: $a_{t} = \text{arg}\max_{k}\hat{\theta}_{k}$
    1. Apply $a_{t}$ and observe $r_{t}$.
    1. Update distribution parameters:
        1. $(\alpha_{t},\beta_{t})\leftarrow\left(\alpha_{t}+r_{t},\beta_{t}+(1-r_{t})\right)$

**Return**
$(\alpha_{1},\dots,\alpha_{K})$ and $(\beta_{1},\dots,\beta_{K})$
```

* An obvious (but naive) alternative approach is to allocate a fixed fraction of the time steps in the time horizon, say $\epsilon$, to explore purely random actions. In contrast, in the remaining periods, only successful actions are chosen via [Thompson sampling](https://en.wikipedia.org/wiki/Thompson_sampling) or another exploitation approach. This strategy, called $\epsilon$-greedy exploration, is shown in {prf:ref}`algo-e-greedy` with a [Thompson sampling](https://en.wikipedia.org/wiki/Thompson_sampling) exploitation step:

```{prf:algorithm} $\epsilon$-greedy exploration
:class: dropdown
:label: algo-e-greedy

**Inputs**  the pure exploration parameter $\epsilon$, the number of time periods in the horizon $T$, the dimension of the action space $K = |\mathcal{A}|$, and
$\alpha_{1},\dots,\alpha_{K}$ and $\beta_{1},\dots,\beta_{K}$ the initial values (prior) for the parameters that appear in the 
Beta-distribution for each action 

**Outputs** posterior values for the parameters $\alpha_{1},\dots,\alpha_{K}$ and $\beta_{1},\dots,\beta_{K}$ that appear in the Beta-distributions for each action

**Initialize**
A Beta-distribution for each action using initial values $(\alpha,\beta)$.

**Main**
1. for t $\in$ 1 to T
    1. if rand() $<\epsilon$
        1. Select random action: $a_{t}\sim\text{Uniform}(\mathcal{A})$
    1. else
        1. for k $\in$ 1 to K
            1. sample $\hat{\theta}_{k} \sim \beta(\alpha_{k}, \beta_{k})$
    
        1. Select action: $a_{t} = \text{arg}\max_{k}\hat{\theta}_{k}$
    
    1. Apply $a_{t}$ and observe $r_{t}$.
    1. Update distribution parameters:
        1. $(\alpha_{t},\beta_{t})\leftarrow\left(\alpha_{t}+r_{t},\beta_{t}+(1-r_{t})\right)$

**Return**
$(\alpha_{1},\dots,\alpha_{K})$ and $(\beta_{1},\dots,\beta_{K})$
```

(content:references:basic-concepts-reinforcement-learning)=
## Basic concepts of reinforcement learning
In our discussion of [Markov decision process (MDPs)](content:references:markov-decision-procesess), we assumed that the transition and reward models were known precisely. However, these models may not be discovered in many actual problems. In these cases, the agent must learn to act through experience, e.g., by observing the outcomes of its actions. Then the agent chooses actions that maximize its long-term accumulation of reward. 

Several challenges must be addressed in cases of uncertain models. First, agents must balance between exploring the world and exploiting knowledge gained through experience. Second, rewards may be received long after decisions have been made. Finally, agents must generalize from limited experience. 


 ```{figure} ./figs/Fig-Schematic-RL.svg
---
height: 260px
name: fig-rl-schematic
---
Schematic of the reinforcement learning (RL) process. An agent takes action $a$ and then observes reward $r$ and changes in the state of the environment ($s\rightarrow{s^{\prime}}$) following the action $a$. 
```


[Reinforcement Learning (RL)](https://en.wikipedia.org/wiki/Reinforcement_learning) agents learn by performing actions in the world and then analyzing the rewards they receive ({numref}`fig-rl-schematic`). Thus, reinforcement learning differs from other machine learning approaches, e.g., [supervised learning](https://en.wikipedia.org/wiki/Supervised_learning) because labeled input/output pairs, e.g., what actions lead to positive rewards are not presented to the agent _a priori_.  Instead, reinforcement learning agents learn what is good or bad by trying different actions. 

Reinforcement learning agents learn optimal choices in different situations by balancing the exploration of their environment, e.g., by taking random actions and watching what happens, with the exploitation of the knowledge they have built up so far, i.e., choosing what the agent thinks is an optimal action based upon previous experience. The balance between exploration and exploitation is one of the critical concepts in reinforcement learning.

(content:references:model-free-reinforcement-learning)=
### Model-free reinforcement learning
Model-free reinforcement learning does not require transition or reward models. Instead, model-free methods, like bandit problems, iteratively construct a policy by interacting with the world. However, unlike bandit problems, model-free reinforcement learning builds the action value function $Q(s, a)$ directly, e.g., by incrementally estimating the action value function $Q(s, a)$ from samples using the [Q-learning approach](https://en.wikipedia.org/wiki/Q-learning). 

Model-free methods incrementally estimate the action value function $Q(s, a)$ by sampling the world. To understand the structure of the update procedures, which we'll discuss later, let's take a quick detour and look at how we incrementally estimate the mean of a single variable $X$. Suppose we are interested in computing the mean of $X$ from $m$ samples:

```{math}
:label: eqn-simple-mean
\hat{x}_{m} = \frac{1}{m}\sum_{i=1}^{m}x^{(i)}
```

where $x^{(i)}$ denotes the ith sample. However, model-free RL incrementally updates $Q(s,a)$; thus, we need to develop an incremental estimation calculation. 
Equation {eq}`eqn-simple-mean` can be re-written as:

```{math}
:label: eqn-simple-mean-1
\hat{x}_{m} = \frac{1}{m}\left(x^{(m)}+\sum_{i=1}^{m-1}x^{(i)}\right)
```

but the summation term is mean from $m-1$ samples or:

```{math}
:label: eqn-simple-mean-2
\hat{x}_{m} = \frac{1}{m}\left(x^{(m)}+(m-1)\hat{x}_{m-1}\right)
```

which can be rearranged to give the incremental update expression:

```{math}
:label: eqn-simple-mean-3
\hat{x}_{m} = \hat{x}_{m-1} + \frac{1}{m}\left(x^{(m)}-\hat{x}_{m-1}\right)
```

Equation {eq}`eqn-simple-mean-3` can be generalized to give the incremental update rule ({prf:ref}`defn-incremental-update-rule`):

````{prf:definition} Incremental update rule
:label: defn-incremental-update-rule

Let $\hat{x}_{m-1}$ denote the mean value computed from $m-1$ samples. The value of $\hat{x}_{m}$ given the the next sample $x^{(m)}$ can be written as:

```{math}
:label: eqn-next-sample-mean
\hat{x}_{m} = \hat{x}_{m-1} + \alpha\left(m\right)\left(x^{(m)}-\hat{x}_{m-1}\right)
```

where $\alpha\left(m\right)$ is the learning rate function. The learning rate can be any function of $m$; however, to ensure convergence 
$\alpha\left(m\right)$ must have the properties: the sum of $\alpha\left(m\right)$ as $m\rightarrow\infty$ can be unbounded, but the sum of $\alpha\left(m\right)^{2}$ as $m\rightarrow\infty$ is bounded. 
````

#### Q-learning
The Q-learning approach incrementally estimates the action value function $Q(s,a)$ using the action value form of the _Bellman expectation equation_.  From our discussion of [Markov decision process (MDPs)](content:references:markov-decision-procesess), we know that the action-value function (the $Q$-function) is defined as:

```{math}
Q(s,a) = R(s,a) + \gamma\sum_{s^{\prime}\in\mathcal{S}}T(s^{\prime} | s, a)U(s^{\prime})
```

However, we know that:

```{math}
U(s) = \max_{a} Q(s,a)
```

thus:

```{math}
:label: eqn-bellman-expectation-eqn
Q(s,a) = R(s,a) + \gamma\sum_{s^{\prime}\in\mathcal{S}}T(s^{\prime} | s, a)\left(\max_{a^{\prime}}Q(s^{\prime},a^{\prime})\right)
```

Here's the catch: in a model-free universe, we don't know the rewards or physics of the world, i.e., we don't know the rewards $R(s, a)$ or the probability array $T(s^{\prime} | s, a)$ that appear in Eqn. {eq}`eqn-bellman-expectation-eqn`. Instead of computing the rewards $r$ and transitions from a model, we must estimate them from samples received:

```{math}
Q(s,a) = \mathbb{E}_{r,s^{\prime}}\left[r+\gamma\cdot\max_{a^{\prime}}Q(s^{\prime},a^{\prime})\right]
```
We can use the incremental update rule shown in Eqn. {eq}`eqn-next-sample-mean` to develop an an expression that incrementally updates our estimate of $Q(s,a)$ as more data becomes available: 

```{math}
:label: eqn-Q-learning-update-rule
Q(s,a)\leftarrow{Q(s,a)}+\alpha\left(r+\gamma\max_{a^{\prime}}Q(s^{\prime},a^{\prime}) - Q(s,a)\right)
```

where $\alpha$ denotes a _learning rate hyperparameter_. Putting all these ideas together, gives a Q-learning definition ({prf:ref}`defn-q-learning-defn`):

````{prf:definition} Q-learning 
:label: defn-q-learning-defn

There exists states $s\in\mathcal{S}$ and actions $a\in\mathcal{A}$. The action value function $Q(s,a)$ can be iteratively estimated using the update rule:

```{math}
Q(s,a)\leftarrow{Q(s,a)}+\alpha\left(r+\gamma\max_{a^{\prime}}Q(s^{\prime},a^{\prime}) - Q(s,a)\right)
```

where $\alpha$ denotes the learning rate hyperparameter, and $\gamma$ denotes the discount rate. The policy $\pi(s)$ can be estimated from the action value function:

```{math}
\pi(s) = \text{arg}\max_{a}Q(s,a)
```
````

An algorithm to incrementally update the $Q$-function is given by ({prf:ref}`algo-e-greedy-q-learning`):

```{prf:algorithm} $\epsilon$-greedy Q-learning
:class: dropdown
:label: algo-e-greedy-q-learning

**Inputs**  the pure exploration parameter $\epsilon$, the number of time periods in the horizon $T$, the state space $\mathcal{S}$, the action space $\mathcal{A}$ and the initial state $s$.

**Outputs** the $Q(s,a)$ array

**Initialize**
1. set $Q(s,a)\leftarrow\text{zeros}(|\mathcal{S}|,|\mathcal{A}|)$

**Main**
1. for t $\in$ 1 to T
    1. if rand() $<\epsilon$
        1. Select random action: $a_{t}\sim\text{Uniform}(\mathcal{A})$
    1. else
        1. Select action: $a_{t}\leftarrow\text{arg}\max_{a}Q(s,a)$
    
    1. Apply $a_{t}$: observe the reward $r_{t}$ and the state $s^{\prime}$
    1. Update $Q(s,a)$:
        1. $Q(s,a)\leftarrow{Q(s,a)}+\alpha\left(r+\gamma\max_{a^{\prime}}Q(s^{\prime},a^{\prime}) - Q(s,a)\right)$
    1. Update state:
        1. set $s\leftarrow{s}^{\prime}$

**Return**
$Q(s,a)$
```


---

# Summary
In this lecture series, we explored bandit problems, and covered different algorithms and strategies for balancing exploration and exploitation:

* [Bandit problems](content:references:multiarm-bandit-problems) refer to a scenario where an agent must repeatedly choose between multiple actions, each with an unknown reward distribution. The goal of the agent is to maximize its total reward over time while learning the reward distributions of the different actions. This presents a tradeoff between exploration, where the agent chooses the action with the highest estimated reward so far, and exploitation, where it selects an action with an uncertain reward to learn more about its distribution.

* [Reinforcement Learning (RL)](content:references:basic-concepts-reinforcement-learning) agents learn by performing actions in the world and then analyzing the rewards they receive. Reinforcement learning differs from other machine learning approaches, e.g., supervised learning, because labeled input/output pairs, e.g., what actions lead to positive rewards are not presented to the agent _a priori_. Reinforcement learning agents learn what is good or bad by trying different actions.