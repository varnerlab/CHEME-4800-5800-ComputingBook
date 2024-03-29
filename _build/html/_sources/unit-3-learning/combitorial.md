# Dynamic Progamming and Heuristic Optimization

## Introduction
Dynamic programming, heuristic optimization, and combinatorial optimization are all techniques used to solve optimization problems. In this lecture, we'll introduce the following:

<!-- Dynamic programming is a method that solves problems by breaking them down into smaller subproblems and solving them independently, then combining the solutions to the subproblems to solve the original problem. On the other hand, heuristic optimization algorithms are a family of algorithms inspired by natural phenomena and human behavior. They explore the search space using rules of thumb, intuition, and trial-and-error methods to find the best solution to a problem. Finally, combinatorial optimization is a field that deals with discrete optimization problems, where the search space consists of discrete objects or structures. There are many different approaches to solving these optimization problems, including exact algorithms, approximation algorithms, and heuristics. -->

* {ref}`content:references:dynamic-programming` solves optimization problems by breaking them down into smaller subproblems, solving each subproblem once, and storing the solutions in a table or array. It is typically used for problems that can be divided into similar subproblems and for which the optimal solution can be constructed from optimal solutions to the subproblems.

* {ref}`content:references:heuristic-optimization` is a class of algorithms used to solve complex optimization problems inspired by natural phenomena, or human, insect or animal behavior. 

<!-- * {ref}`content:references:branch-and-bound` is an algorithmic technique for solving combinatorial optimization problems. It involves dividing the search space into smaller subproblems and then using bounds on the optimal solutions to prune the search space and avoid considering suboptimal solutions. -->

---

(content:references:dynamic-programming)=

## Dynamic programming

Dynamic programming is an approach developed by [Richard Bellman in the 1950s](https://en.wikipedia.org/wiki/Richard_E._Bellman) that solves problems by breaking them down into smaller subproblems and storing the solutions to these subproblems. This allows the program to avoid recomputing the solution to the same subproblem multiple times and instead reuse the stored solutions, which can significantly improve the algorithm’s efficiency.

Dynamic programming is typically used for problems that can be divided into [overlapping subproblems](https://en.wikipedia.org/wiki/Overlapping_subproblems), i.e., the solution to one subproblem can be used to solve other subproblems. This is often the case with optimization problems, where the goal is to find the optimal solution among a set of possible solutions. It is also true for _sequential decision problems_.

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

Equation {eq}`eqn-bellman-decision-eqn` is a [functional equation](https://en.wikipedia.org/wiki/Functional_equation), i.e., an equation in which functions appear as unknowns. Thus, solving Eqn. {eq}`eqn-bellman-decision-eqn` gives the unknown value function $V(x)$. Further, by calculating the value function, we will also find the _policy function_ $a(x)$ that describes the optimal action as a function of the system state $x$.


(content:references:heuristic-optimization)=
## Heuristic optimization

Heuristic optimization is a family of algorithms inspired by natural phenomena and human behavior. Heuristic methods are often used to solve complex, real-world problems where exact solutions are difficult to obtain or may not even exist. Unlike traditional optimization methods, heuristic approaches use rules of thumb, intuition, and trial-and-error to explore the search space and find the best solution.

There are several types of heuristic optimization algorithms, including:

* [Simulated Annealing (SA)](https://en.wikipedia.org/wiki/Simulated_annealing) is inspired by the process of annealing in metallurgy. It starts with an initial solution and gradually changes it by randomly perturbing it and accepting or rejecting the new solution based on a probability function. SA is commonly used for problems that involve finding the global minimum of a non-convex function.
* [Genetic Algorithms (GA)](https://en.wikipedia.org/wiki/Genetic_algorithm) are inspired by the process of natural selection in biology. It starts with a population of potential solutions represented as chromosomes, which evolve through a series of selection, crossover, and mutation operations to create a new generation of solutions. GA is commonly used for problems that involve a large number of variables and constraints.
* [Particle Swarm Optimization (PSO)](https://en.wikipedia.org/wiki/Particle_swarm_optimization) simulates the behavior of a swarm of particles moving in a multi-dimensional search space. Each particle represents a potential solution to the problem, and it adjusts its position and velocity based on the best solution found by the swarm. PSO is commonly used for problems that require continuous optimization.
* [Ant Colony Optimization (ACO)](https://en.wikipedia.org/wiki/Ant_colony_optimization_algorithms) is inspired by the behavior of ant colonies. It involves a set of artificial ants that communicate with each other using pheromone trails to find the best path between a start and end point. ACO is commonly used for problems that find the shortest path in a graph.
* [Artificial Bee Colony (ABC)](https://en.wikipedia.org/wiki/Artificial_bee_colony_algorithm) simulates the behavior of a colony of artificial honey bees. ABC is commonly used for problems that involve finding the optimal solution in a continuous search space. Each bee represents a potential solution to the problem and adjusts its position based on the quality of the nectar source it has found.
* [Tabu Search (TS)](https://en.wikipedia.org/wiki/Tabu_search) uses a memory-based search strategy to avoid revisiting previously explored solutions. TS is commonly used for problems that involve finding the optimal solution in a combinatorial search space. It maintains a list of tabu moves, which are forbidden moves, to ensure that the search space is explored efficiently.

### Simulated annealing

Simulated annealing (SA) is a heuristic optimization algorithm inspired by the annealing process in metallurgy {cite}`SIMAN1983`. Simulated annealing is especially good at finding near-optimal solutions to problems with many local optima.

Simulated annealing solves complex optimization problems by randomly selecting a candidate solution, and evaluating the fitness of this candidate compared to the current best solution. The candidate solution is accepted or rejected based on a probability function; candidate solutions far away from the best solution are less likely to be selected. However, the willingness of the simulated annealing algorithm to choose a candidate far away from the best solution changes over time; in the beginning, the SA algorithm is more willing to take a chance. However, as time progresses, the SA algorithm only bets on sure things, i.e., new solutions that are strictly better than the best solution found so far.

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



<!-- ### Genetic algortihms

Genetic algorithms are heuristic optimization algorithms inspired by natural selection and genetic inheritance. Genetic algorithms solve problems by iteratively generating and evaluating a population of candidate solutions, then applying selection, crossover, and mutation operators to evolve the population toward better solutions.

Genetic algorithms can handle continuous and discrete search spaces and are often used in complex optimization problems such as scheduling, routing, and machine learning. A pseudocode implementation of a genetic algorithm is given in {prf:ref}`algo-genetic-algorithm`:

````{prf:algorithm} Genetic Algorithm
:label: algo-genetic-algorithm
:class: dropdown

**Initialize** 
1. set $\mathcal{P}\leftarrow\text{initialize_population()}$
1. set $\mathcal{F}\leftarrow\text{evaluate_fitness}(\mathcal{P})$

**Main**
1. while not termination_condition_met():
    1. set $x\leftarrow\text{select_parents()}$
    1. set $x^{\prime}\leftarrow\text{create_offspring}(x)$
    1. set $\mathcal{F}^{\prime}\leftarrow\text{evaluate_fitness}(x^{\prime})$
    1. replace_least_fit()

1. return_best()

**Initialize populattion**
// create population of individuals with random genes
1. function initialize_population():
    1. set population = []
    1. for $i\in {1,2,\dots,\text{population_size}}$:
        1. individual = create_random_individual()
        1. population.append(individual)

**Evaluate fitness**
// evaluate fitness of each individual in the population
1. function evaluate_fitness():
    1. for individual in population:
        1. set individual.fitness $\leftarrow$ calculate_fitness(individual)

**Select parents**
// select parents for reproduction
1. function select_parents():
    1. set parents $\leftarrow$ []
    1. for i $\in{1:\text{parent_selection_size}}$:
        1. set parent $\leftarrow$ tournament_selection(population, tournament_size)
        1. parents.append(parent)
1. return parents

**Create offspring**
// create offspring through crossover and mutation
1. function create_offspring():
    1. offspring $\leftarrow$ []
    1. for i in range(offspring_size):
        1. parent1, parent2 = select_parents()
        1. child = crossover(parent1, parent2)
        1. child = mutate(child)
        1. offspring.append(child)
    1. return offspring

**Replace**
// replace least fit individuals in the population with offspring
1. function replace_least_fit():
    1. set offspring_fitness = [individual.fitness for individual in offspring]
    1. for i in range(replacement_size):
        1. set least_fit_index $\leftarrow$ population_fitness.index(min(population_fitness))
        1. set population[least_fit_index] $\leftarrow$ offspring[i]
        1. set population_fitness[least_fit_index] $\leftarrow$ offspring_fitness[i]

**Best individual**
// return individual with best fitness
1. function return_best():
    1. set best_individual $\leftarrow$ max(population, key=lambda individual: individual.fitness)
1. return best_individual

**Tournament selection**
// select individual with highest fitness in tournament
1. function tournament_selection(population, tournament_size):
    1. set tournament $\leftarrow$ random.sample(population, tournament_size)
    1. set winner $\leftarrow$ max(tournament, key=lambda individual: individual.fitness)
1. return winner

**Crossover**
// combine genes of parents to create child
1. function crossover(parent1, parent2):
    1. set child $\leftarrow$ Individual()
    1. for i in range(gene_count):
        1. if random.random() < crossover_probability:
            1. child.genes[i] $\leftarrow$ parent1.genes[i]
        1. else:
            1. child.genes[i] $\leftarrow$ parent2.genes[i]
1. return child

**Mutate**
// randomly mutate genes of individual
1. function mutate(individual):
    1. for $i\in{1,2,\dots\text{gene_count}}$:
        1. if random.random() < mutation_probability:
            1. individual.genes[i] $\leftarrow\mathcal{U}$(min_gene_value, max_gene_value)
1. return individual
```` -->

<!-- 
While some problems cannot be decomposed in this way, problems that span several time points or naturally structured in stages can often be decomposed recursively. If a problem can be solved optimally by breaking it into subproblems and then recursively finding the optimal solutions to the subproblems, then it is said to have _optimal substructure_.

If dynamic programming methods are applicable, then there is a relationship between the value of the larger problem and the values of the subproblems; this relationship is called the [Bellman equation](https://en.wikipedia.org/wiki/Bellman_equation). 

Dynamic programming can be a valuable tool for solving many problems, but it can be computationally intensive and may only sometimes be the most efficient solution. When deciding whether to use dynamic programming, it is essential to consider the trade-offs between the algorithm’s efficiency and the problem’s complexity. -->

<!-- (content:references:branch-and-bound)=
## Branch and Bound

Branch and bound is an approach to solve a variety of discrete and combinatorial optimization problems. A branch-and-bound algorithm enumerates candidate solutions by means of state space search: the set of candidate solutions forms a rooted tree with the full set at the root.

Let's consider minimizing an arbitrary objective function $f$:

```{prf:algorithm} Branch and Bound Algorithm
:label: algo-branch-bound-generic
:class: dropdown

**Inputs** the objective function $f$, a branch function, and a bound function

**Outputs** the minimum function value $\mathcal{B}$

**Initialize** 
1. Set $\mathcal{B}\leftarrow\infty$ where $\mathcal{B}$ is the best solution found so far
2. Initialize $Q$, a queue holding problem variables

**Main**
1. while $Q$ is not empty
    1. Take a node $N$ from the queue: $N\leftarrow\text{dequeue}(Q)$
    1. Evaluate the objective function value of node $N$: $B_{N}\leftarrow{f(x_{N})}$  
    1. if $B_{N}<\mathcal{B}$
        1. Update best solution: $\mathcal{B}\leftarrow{B_{N}}$
    1. else
        1. Branch on node $N$ to produce new candidate nodes: $(N_{1},\dots) \leftarrow\text{branch}(N)$
        1. for N $\in\left(N_{1},\dots\right)$
            1. if bound(N) $\leq\mathcal{B}$
                1. Q $\leftarrow$ enqueue(Q, N)

**Return**
the minimum function value $\mathcal{B}$
``` -->

---

## Summary

Dynamic programming, heuristic optimization, and other combinatorial optimization techniques can all be used to solve optimization problems.
In this lecture we introduced several optimization approaches:

<!-- 
Dynamic programming is a method that solves problems by breaking them down into smaller subproblems and solving them independently, then combining the solutions to the subproblems to solve the original problem. On the other hand, heuristic optimization algorithms are a family of algorithms inspired by natural phenomena and human behavior. They explore the search space using rules of thumb, intuition, and trial-and-error methods to find the best solution to a problem. Finally, combinatorial optimization is a field that deals with discrete optimization problems, where the search space consists of discrete objects or structures. There are many different approaches to solving these optimization problems, including exact algorithms, approximation algorithms, and heuristics.

In this lecture we introduced several optimization approaches: -->

* {ref}`content:references:dynamic-programming` solves optimization problems by breaking them down into smaller subproblems, solving each subproblem once, and storing the solutions in a table or array. It is typically used for problems that can be divided into similar subproblems and for which the optimal solution can be constructed from optimal solutions to the subproblems.

* {ref}`content:references:heuristic-optimization` is a class of algorithms used to solve complex optimization problems, inspired by natural phenomena and human behavior. These algorithms are particularly useful when mathematical models are not available or are too expensive to compute.

<!-- * {ref}`content:references:branch-and-bound` is an algorithmic technique for solving combinatorial optimization problems. It involves dividing the search space into smaller subproblems and then using bounds on the optimal solutions to prune the search space and avoid considering suboptimal solutions. -->
