# Dynamic programming and Heuristic Optimization

```{topic} Overview
Dynamic programming, heuristic optimization, and combinatorial optimization are all techniques used to solve real-world optimization problems. In this lecture, we'll introduce the following, along with their practical applications:

* {ref}`content:references:dynamic-programming` solves optimization problems by breaking them into smaller subproblems, solving each subproblem once, and storing the solutions in a table or array. It is typically used for problems that can be divided into similar subproblems, and the optimal solution can be constructed from optimal solutions to the subproblems.

* {ref}`content:references:heuristic-optimization` is a class of algorithms used to solve nonlinear optimization problems inspired by natural phenomena and human behavior. These algorithms are beneficial when the objective function or constraints are non-convex, non-differentiable, or expensive to evaluate. Heuristic optimization algorithms use rules of thumb, intuition, and trial-and-error methods to explore the search space and find near-optimal solutions.
```

---

(content:references:dynamic-programming)=
## Dynamic programming
Dynamic programming is an approach developed by [Richard Bellman in the 1950s](https://en.wikipedia.org/wiki/Richard_E._Bellman) that solves problems by breaking them down into smaller subproblems and storing the solutions to these subproblems. This allows the program to avoid recomputing the solution to the same subproblem multiple times and instead reuse the stored solutions, significantly improving the algorithm’s efficiency.

Dynamic programming is typically used for problems that can be divided into [overlapping subproblems](https://en.wikipedia.org/wiki/Overlapping_subproblems), i.e., the solution to one subproblem can be used to solve other subproblems. This is often the case with optimization problems, where the goal is to find the optimal solution among possible solutions. It is also true for _sequential decision problems_.

### Dynamic decision problems
Imagine we have a decision-making agent in some state $x_{t}$, at time $t$. At time $t$, the agent takes an action $a_{t}$ from a set of possible actions $a_{t}\in\mathcal{A}\left(x_{t}\right)$ that leads to a new state $x_{t+1} = T(x_{t},a_{t})$ and a payoff $F(x_{t},a_{t})$. In this situation, an infinite-horizon decision problem takes the form:

$$
\begin{eqnarray}
V(x_{\circ}) & = & \max_{a\in\mathcal{A}} \sum_{t=0}^{\infty}
\beta^{t}F(x_{t},a_{t}) \\
\text{subject to}&~& a_{t}\in\mathcal{A}(x_{t}) \\
\text{and} &~& x_{t+1} = T(x_{t},a_{t})
\end{eqnarray}
$$

where the function $V(x_{\circ})$ is the _value function_, $x_{\circ}$ is the initial state of the agent and $0\leq\beta\leq{1}$ denotes the discount factor. 
Dynamic programming breaks this decision problem into smaller subproblems using Bellman's principle of optimality ({prf:ref}`obs-bellman-principle-of-optimality`):

````{prf:observation} Bellman's principle of optimality
:label: obs-bellman-principle-of-optimality

__Principle of optimality__: An optimal policy has the property that whatever the initial state and initial decision are, the remaining decisions must constitute an optimal policy with regard to the state resulting from the first decision.
````

The optimality principle suggests that we can pull the first decision from the sum, which gives a value function of the form:

$$
\begin{eqnarray}
V(x_{\circ}) & = & \max_{a_{\circ}\in\mathcal{A}} \left\{F(x_{\circ},a_{\circ}) + \beta\cdot\left[\max_{a_{t}\mathcal{A}}\sum_{t=1}^{\infty}\beta^{t-1}F(x_{t},a_{t})\right]\right\}\\
\text{subject to}&~& a_{t}\in\mathcal{A}(x_{t}) \\
\text{and} &~& x_{t+1} = T(x_{t},a_{t})
\end{eqnarray}
$$

However, the quantity inside the brackets of the $V(x_{\circ})$ expression is just $V(x_{1})$, i.e., the decision problem starting at time step `1` instead of time step `0`. Thus, we can re-write the expression above as:

$$
\begin{eqnarray}
V(x_{\circ}) & = & \max_{a_{\circ}\in\mathcal{A}} \left\{F(x_{\circ},a_{\circ}) + \beta\cdot{V(x_{1})}\right\}\\
\text{subject to}&~& a_{t}\in\mathcal{A}(x_{t}) \\
\text{and} &~& x_{t+1} = T(x_{t},a_{t})
\end{eqnarray}
$$

Dropping the subscripts gives rise to a [Bellman equation](https://en.wikipedia.org/wiki/Bellman_equation) of the form shown in {prf:ref}`defn-bellman-decision-eqn`:

````{prf:definition} Bellman equation
:label: defn-bellman-decision-eqn

The optimal value of a state $x$, denoted by $V(x)$, is governed by a Bellman equation of the form:

```{math}
:label: eqn-bellman-decision-eqn
V(x) = \max_{a\in\mathcal{A}} \left\{F(x,a) + \beta\cdot{V(T(x,a))}\right\}
```

where $\mathcal{A}$ denotes the set of possible actions open to the decision maker and $0\leq\beta\leq{1}$ represents the discount factor. 
````

Equation {eq}`eqn-bellman-decision-eqn` is a [functional equation](https://en.wikipedia.org/wiki/Functional_equation), i.e., an equation in which functions appear as unknowns. Thus, solving Eqn. {eq}`eqn-bellman-decision-eqn` gives the unknown value function $V(x)$. Further, by calculating the value function, we will find the _policy function_ $a(x)$ that describes the optimal action as a function of the system state $x$.


(content:references:heuristic-optimization)=
## Heuristic optimization
Suppose we want to maximize or minimize a nonlinear objective function $f(x)$ subject to some (potentially nonlinear) constraints $g(x)$. One of the first things we might try is the [method of Lagrange multipliers](https://en.wikipedia.org/wiki/Lagrange_multiplier):


````{prf:definition} Method of Lagrange multipliers
:label: defn-method-of-lagrange-multipliers
 
To find the optimum of a function $f(x)$ subject to the equality constraint $g(x)$, 
we form the Lagrangian $\mathcal{L}(x,\lambda)$:

$$
\begin{equation}
\mathcal{L}(x,\lambda) = f(x) + \lambda\cdot{g}(x)
\end{equation}
$$

where $\lambda$ is the Lagrange multiplier for constraint $g(x)$. At a critical point (maximum or minimum), the partial derivatives of the Lagrangian with respect to $x$ and $\lambda$ vanish:

$$
\begin{eqnarray}
\frac{\partial\mathcal{L}}{\partial{x}} & = & f^{\prime}(x) + \lambda\cdot{g^{\prime}(x)} = 0\\
\frac{\partial\mathcal{L}}{\partial{\lambda}} & = & g(x) = 0
\end{eqnarray}
$$

This system of equations, which represents first-order optimality conditions, can be solved for the critical points and, 
thus, the maximum or minimum value of the objective function.
````

However, numerically solving the system first-order optimality equations directly can be challenging, especially when the objective function $f(x)$ or constraints $g(x)$ are non-convex, or expensive to evaluate. Alternatively, we could use a numerical optimization algorithm to find the optimal solution for unconstrained problems, such as [gradient descent](https://en.wikipedia.org/wiki/Gradient_descent). Gradient descent is an iterative optimization algorithm that uses the gradient of the objective function to update the solution in the direction of the steepest descent:

$$
\begin{equation}
x_{i+1} = x_{i} - \alpha\cdot\nabla{f}(x_{i})
\end{equation}
$$

where $x_{i}$ is the current solution, $\alpha$ is the step size, and $\nabla{f}(x_{i})$ is the gradient of the objective function at $x_{i}$. If we have constraints, we can use a [penalty function](https://en.wikipedia.org/wiki/Penalty_method) approach to enforce the constraints, i.e., we add a penalty term to the objective function that penalizes violations of the constraints.

However, [gradient descent](https://en.wikipedia.org/wiki/Gradient_descent) may not always work, especially when the objective function $f(x)$ or constraints $g(x)$ have some edge-case properties, i.e., these functions are non-differentiable. Moreover, maybe we don't have a mathematical model for the objective function, e.g., it relies on a solution to another problem, or the derivative is too expensive to compute. Finally, the objective function may have many local optima, and gradient descent may get stuck in a local minimum. Thus, [gradient descent](https://en.wikipedia.org/wiki/Gradient_descent)  is a hassle to implement and may not always work (`IMHO!`).

We can use heuristic optimization algorithms to find near-optimal solutions in such cases. Heuristic optimization is a family of algorithms inspired by natural phenomena and human behavior. Heuristic methods are often used to solve complex, real-world problems where exact solutions are difficult to obtain or may not even exist. Unlike traditional optimization methods, heuristic approaches use rules of thumb, intuition, and trial-and-error to explore the search space and find the best solution. They use values of the objective function to guide the search, and they may not always find the optimal solution, but they can find a good solution in a reasonable amount of time.

There are several types of heuristic optimization algorithms, including:

* [Simulated Annealing (SA)](https://en.wikipedia.org/wiki/Simulated_annealing) is inspired by the process of annealing in metallurgy. It starts with an initial solution and gradually changes it by randomly perturbing it and accepting or rejecting the new solution based on a probability function. SA is commonly used for problems that involve finding the global minimum of a non-convex function.
* [Genetic Algorithms (GA)](https://en.wikipedia.org/wiki/Genetic_algorithm) are inspired by the process of natural selection in biology. It starts with a population of potential solutions represented as chromosomes, which evolve through selection, crossover, and mutation operations to create a new generation of solutions. GA is commonly used for problems that involve a large number of variables and constraints.
* [Particle Swarm Optimization (PSO)](https://en.wikipedia.org/wiki/Particle_swarm_optimization) simulates the behavior of a swarm of particles moving in a multi-dimensional search space. Each particle represents a potential solution to the problem, and it adjusts its position and velocity based on the best solution found by the swarm. PSO is commonly used for problems that require continuous optimization.
* [Ant colony optimization (ACO)](https://en.wikipedia.org/wiki/Ant_colony_optimization_algorithms) is inspired by the behavior of ant colonies. It involves a set of artificial ants that communicate with each other using pheromone trails to find the best path between a start and end point. ACO is commonly used for problems that find the shortest path in a graph.
* [Artificial Bee Colony (ABC)](https://en.wikipedia.org/wiki/Artificial_bee_colony_algorithm) simulates the behavior of a colony of artificial honey bees. ABC is commonly used for problems that involve finding the optimal solution in a continuous search space. Each bee represents a potential solution to the problem and adjusts its position based on the quality of the nectar source it has found.
* [Tabu Search (TS)](https://en.wikipedia.org/wiki/Tabu_search) uses a memory-based search strategy to avoid revisiting previously explored solutions. TS is commonly used for problems that involve finding the optimal solution in a combinatorial search space. It maintains a list of tabu moves, which are forbidden moves, to ensure that the search space is explored efficiently.

### Simulated annealing
Simulated annealing (SA) is a heuristic optimization algorithm inspired by the annealing process in metallurgy {cite}`SIMAN1983`. Simulated annealing is especially good at finding near-optimal solutions to problems with many local optima.

Simulated annealing solves complex optimization problems by randomly selecting a candidate solution and evaluating the fitness of this candidate compared to the current best solution. The candidate solution is accepted or rejected based on a probability function; candidate solutions far away from the best solution are less likely to be selected. However, the willingness of the simulated annealing algorithm to choose a candidate that is far from the best solution changes over time; in the beginning, the SA algorithm is more willing to take a chance. However, as time progresses, the SA algorithm only bets on sure things, i.e., new solutions that are strictly better than the best solution found so far.

A pseudo-code implementation for a simulated annealing routine is given in {prf:ref}`algo-simulated-annealing`:

````{prf:algorithm} Simulated Annealing
:label: algo-simulated-annealing
:class: dropdown

**Inputs** The objective function $f$, temperature, neighbor, and accept functions, initial guess $x_{\circ}$, initial temperature $T$, and max iterations $\mathcal{M}_{\infty}$

**Outputs** the $\min_{x}f(x)$ and $\arg\min_{x}f(x)$.

**Initialize** 
1. Set best solution $\hat{x}\leftarrow{x}_{\circ}$
1. Set initial temperature $T_{0}\leftarrow{T}$


**Main**
1. for $i\in\left\{1,\dots,\mathcal{M}_{\infty}\right\}$
    1. update temperature $T_{i}\leftarrow\text{temperature}(T_{i-1},i)$
    1. generate new solution $x^{\prime}\leftarrow\text{neighbor}(\hat{x})$
    1. compute objective function difference $\Delta_{i} = f(x^{\prime}) - f(\hat{x})$
    
    1. if $\text{accept}(\Delta_{i},T_{i}) = \text{true}$
        1. update the current best solution ${\hat{x}}\leftarrow{x}^{\prime}$

**Return**
the tuple $f(\hat{x})$ and $\hat{x}$.
````

The performance of simulated annealing depends upon the choice of the temperature, neighbor, and accept functions. Example pseudo code implementations for these functions is shown in {prf:ref}`algo-simulated-annealing-other-functions`:

````{prf:algorithm} Temperature, neighbor, and accept functions
:label: algo-simulated-annealing-other-functions
:class: dropdown

**Temperature function**
1. function temperature($T$, $i$):
    1. return $T\leftarrow\alpha\times{T}~$ where $\alpha<1$

**Accept function**
1. function accept($\Delta$, $T$):
    1. if $\Delta < 0$:
        1. return true
    1. if random(0, 1) < $\exp\left(-\Delta/T\right)$:
        1. return true
1. return false

**Neighbor function**
1. function neighbor(solution):
    1. set new_solution $\leftarrow$ copy(solution)
    1. select random move
    1. perform move on new_solution
1. return new_solution

````

Let's explore the performance of a simulated annealing implementation in {prf:ref}`example-sa-impl-spehere-function`:

````{prf:example} Minimization of the Sphere function
:label: example-sa-impl-spehere-function
:class: dropdown

Minimize the d-dimensional sphere function $f(x)$:

```{math}
f(x) = \sum_{i=1}^{d}x_{i}^{2}
```

subject to the bounds constraints $x_{i}\in\left[-5.12,5.12\right]$ using simulated annealing. 
The solution code for this example can be found [here](https://github.com/varnerlab/CHEME-1800-4800-Course-Repository-S23/tree/main/examples/unit-3-examples/simulated_annealing).
````

---

## Summary
Dynamic programming, heuristic optimization, and other combinatorial optimization techniques can all be used to solve optimization problems. In this lecture we introduced several optimization approaches:

<!-- 
Dynamic programming is a method that solves problems by breaking them down into smaller subproblems and solving them independently, then combining the solutions to the subproblems to solve the original problem. On the other hand, heuristic optimization algorithms are a family of algorithms inspired by natural phenomena and human behavior. They explore the search space using rules of thumb, intuition, and trial-and-error methods to find the best solution to a problem. Finally, combinatorial optimization is a field that deals with discrete optimization problems, where the search space consists of discrete objects or structures. There are many different approaches to solving these optimization problems, including exact algorithms, approximation algorithms, and heuristics.

In this lecture we introduced several optimization approaches: -->

* {ref}`content:references:dynamic-programming` solves optimization problems by breaking them down into smaller subproblems, solving each subproblem once, and storing the solutions in a table or array. It is typically used for problems that can be divided into similar subproblems and for which the optimal solution can be constructed from optimal solutions to the subproblems.

* {ref}`content:references:heuristic-optimization` is a class of algorithms used to solve nonlinear optimization problems, inspired by natural phenomena and human behavior. These algorithms are particularly useful when the objective function or constraints are non-convex, non-differentiable, or expensive to evaluate. Heuristic optimization algorithms use rules of thumb, intuition, and trial-and-error methods to explore the search space and find near-optimal solutions.

<!-- * {ref}`content:references:branch-and-bound` is an algorithmic technique for solving combinatorial optimization problems. It involves dividing the search space into smaller subproblems and then using bounds on the optimal solutions to prune the search space and avoid considering suboptimal solutions. -->
