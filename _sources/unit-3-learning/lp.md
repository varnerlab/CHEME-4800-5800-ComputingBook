# Linear Programming

```{topic} Overview
Linear programming (LP) is a method to estimate the best outcome (such as maximum profit, lowest cost, or the best possible production rate) using a mathematical model composed of linear relationships subject to linear constraints. 

* {ref}`content:references:primal-linear-problem` is a linear programming problem aiming to maximize or minimize a linear objective function subject to linear constraints in the form of inequalities or equalities. The term primal refers to the original form of the linear programming problem instead of its dual form.

* {ref}`content:references:dual-linear-problem` program is a formulation derived from the primal linear program that provides an alternative perspective on the same problem by introducing a new set of variables and constraints. The objective of the dual program is to either maximize or minimize a linear function subject to a different set of constraints derived from the original primal program.

* {ref}`content:references:flux-balance-analysis` is a versatile tool that can estimate flows through various types of networks and graphs. It can be applied to social graphs, communication networks, or any other problem that can be represented as a network of graphs. 

Linear programming, a practical and widely used tool, finds its applications in diverse fields such as business, economics, engineering, and more. It is instrumental in modeling and solving real-world problems, be it resource allocation, production planning, transportation, or other complex scenarios.
```
---

(content:references:resource-allocation-linear-problem)=
## Resource allocation problems
The classic application of linear programming, as shown in ({prf:ref}`example-resource-allocation-primal`), is to solve optimal resource allocation problems. However, before we discuss the primal linear programming problem and more advanced concepts such as duality, which we'll frame in economic terms, let's look at a general resource allocation problem ({prf:ref}`defn-general-resource-allocation-struct`):

````{prf:definition} Maximum Utility Resource Allocation Problems
:label: defn-general-resource-allocation-struct
Suppose there exists $\texttt{objects}$ $X = \left\{x_{i}\right\}_{i=1}^{n}$, 
a function $U: X\rightarrow\mathbb{R}$ which assigns a utility score to collections of objects, 
and $I$ resource units, e.g., money, time, etc, to allocate amongst the objects.
Then, rational agents maximize utility subject to the budget:

$$
\begin{eqnarray*}
\text{maximize}~\mathcal{O}(\mathbf{x}) &=& U\left(x_{1},\dots,x_{n}\right) \\
\text{subject to}~\sum_{i\in{1,\dotsc,n}}c_{i}\cdot{x}_{i} & = & I\\
\text{and}~x_{i}&\geq&{0}\qquad{i=1,2,\dots,n}
\end{eqnarray*}
$$

The $c_{i}\geq{0}~\forall{i}$ denotes the cost of object $i$, and $x_{i}\geq{0}$ represents 
the amount of object $i$ purchased or consumed by the agent. If $U: X\rightarrow\mathbb{R}$ is linear in $x_{i}$, 
then this problem is a $\texttt{linear programming}$ problem; otherwise, it is a $\texttt{nonlinear programming}$ problem.

````

The problem structure in ({prf:ref}`defn-general-resource-allocation-struct`) is highly adaptable and widely used for various applications. For example, there may be multiple types of constraints on the resources, e.g., time, money, etc., or the utility function may be non-linear, or the decision variables may be discrete, e.g., $x_{i}\in{0,1}$, etc. 

Let's explore the primal and dual linear programming problems and then apply these concepts to a resource allocation problem. In the cases we explore, the decision variables are continuous, the constraints are linear, and the objective function is linear. 

(content:references:primal-linear-problem)=
## Primal linear programs
Linear programs maximize (or minimize) a _linear_ objective function $\mathcal{O}(\mathbf{x})$ subject to _linear_ constraints and bounds on the decision variables $x$. Decision variables can be continuous values $x\in\mathbb{R}$, but they can also be integers, e.g., $x\in{0,1}$. We'll start with the continuous problem ({prf:ref}`defn-primael-linear program`):  


````{prf:definition} Primal Linear Program
:label: defn-primael-linear program

Let $\mathcal{O}(\mathbf{x})$ denote a linear function of the continuous non-negative decision variables $\mathbf{x}\in\mathbb{R}^{m}$
whose values are constrained by a system of linear equations and bounded. Then, the optimal $\mathbf{x}^{\star}$ is a solution of the linear program:

$$
\begin{eqnarray*}
\text{maximize}~\mathcal{O}(\mathbf{x}) &=& \sum_{i=1}^{m} c_{i}\cdot{x}_{i}\\
\text{subject to}~\mathbf{A}\cdot\mathbf{x} &\leq&\mathbf{b}\\
\text{and}~x_{i}&\geq&{0}\qquad{i=1,2,\dots,m}
\end{eqnarray*}
$$

The constants $c_{i}$ are coefficients in the objective function, $\mathbf{A}\in\mathbb{R}^{n\times{m}}$ 
is the constraint matrix and $\mathbf{b}\in\mathbb{R}^{n}$ is a constant vector. 
Any linear program can be converted into this standard form.
````

The constraints of a linear program form a [polyhedron](https://en.wikipedia.org/wiki/Polyhedron), where the solution lies on [extreme points of the polyhedron](https://en.wikipedia.org/wiki/Extreme_point). The solution to a linear program is always at an extreme point of the feasible region, which is the region defined by the constraints. The solution can be at a vertex, edge, or face of the polyhedron. 

### Simplex algorithm
Linear programs, even those with thousands of decisions variables, can be efficently solved using simple off-the-shelf hardware, e.g., your laptop using the [simplex algorithm](https://optimization.cbe.cornell.edu/index.php?title=Simplex_algorithm). The simplex algorithm is a widely-used algorithm to solve the Linear Programming (LP) optimization problems first developed by [George Dantzig](https://en.wikipedia.org/wiki/George_Dantzig).

The [simplex algorithm](https://optimization.cbe.cornell.edu/index.php?title=Simplex_algorithm) is an iterative procedure for solving linear programming problems. The basic idea behind the simplex algorithm is to start with an initial feasible solution (i.e., a point that satisfies all constraints) and then iteratively improve it by moving along the edges of the feasible region (i.e., the region defined by the constraints). At each iteration, the algorithm selects a non-basic variable (i.e., a variable not currently part of the solution). It determines whether increasing or decreasing it will improve the objective function. If such an improvement can be made, the algorithm swaps the non-basic variable into the solution and updates the values of the basic variables (i.e., the variables already part of the solution). The process is repeated until no further improvement can be made, at which point the algorithm terminates, and the current solution is deemed optimal.

As an aside, the [simplex algorithm](https://optimization.cbe.cornell.edu/index.php?title=Simplex_algorithm) resulted from [Dantzig](https://en.wikipedia.org/wiki/George_Dantzig) showing up late to class. In 1939, near the beginning of a class, [Professor Neyman](https://en.wikipedia.org/wiki/Jerzy_Neyman), an instructor for a course that [Dantzig](https://en.wikipedia.org/wiki/George_Dantzig) was taking, wrote two problems on the blackboard. [Dantzig](https://en.wikipedia.org/wiki/George_Dantzig) arrived late and assumed they were homework problems. According to [Dantzig](https://en.wikipedia.org/wiki/George_Dantzig), the problem set seemed more complicated than usual, but a few days later, he handed in solutions for both problems, believing that his assignment was late. However, six weeks later, an excited [Professor Neyman](https://en.wikipedia.org/wiki/Jerzy_Neyman) told [Dantzig](https://en.wikipedia.org/wiki/George_Dantzig) that the homework problems he had solved by mistake were two of the most famous unsolved problems in statistics. The rest is history; [Dantzig](https://en.wikipedia.org/wiki/George_Dantzig) got his Ph.D. in 1946 and then developed the simplex algorithm in 1947, and linear programming was born!


<!-- [The development of the simplex algorithm](https://optimization.cbe.cornell.edu/index.php?title=Simplex_algorithm), an efficient approach for solving linear programs, has led to LPs being used to solve problems in many engineering applications and other diverse industries such as banking, education, forestry, energy, and logistics.  -->

### Example: Resource allocation problem
Let's explore linear programming by doing a classic problem, namely the optimal allocation of a scarce resource in a manufacturing context ({prf:ref}`example-resource-allocation-primal`):

````{prf:example} Product mix primal problem
:label: example-resource-allocation-primal
:class: dropdown

Consider a manufacturing facility that produces a mixture of five different chemical products $C_{i}$ using four processes $P_{j}$ weekly. The process yeilds for each product, and the profit for each product, are shown in the table:

| Process | Capacity | C$_{1}$ |  C$_{2}$ | C$_{3}$ | C$_{4}$ | C$_{5}$ |
| :---: | :---: | --- | --- | --- | --- | --- |
P$_{1}$ | 160 | 1.2 | 1.3 | 0.7 | 0.0 | 0.5
P$_{2}$ | 200 | 0.7 | 2.2 | 1.6 | 0.5 | 1.0 |
P$_{3}$ | 120 | 0.9 | 0.7 | 1.3 | 1.0 | 0.8 |
P$_{4}$ | 280 | 1.4 | 2.8 | 0.5 | 1.2 | 0.6 |
Unit profit $ | -- | 18 | 25 | 10 | 12 | 15

Determine the optimum production quantities for the chemical products $C_{j}$ to maximize the total profit.

__Solution__: Let's define the decision variables, the objective function, and the problem constraints, then solve the problem in [Julia](https://julialang.org) using the [JuMP](https://jump.dev/JuMP.jl/stable/) package and the [GLPK](https://github.com/jump-dev/GLPK.jl) linear programming solver.

Let $x_{i}\geq{0}$ denote the amount of compound $i$ produced per week (decision variable). The objective function $\mathcal{O}$, which is to maximize profit, is then given by:

```{math}
\mathcal{O} = 18x_{1} + 25x_{2} + 10x_{3} + 12x_{4} + 15x_{5} 
```

where the coefficients are the unit profit for each product. Each process is governed by feedstock constraints:

```{math}
\begin{eqnarray}
P_{1} & : & 1.2x_{1}+1.3x_{2}+0.7x_{3}+0.0x_{4} + 0.5x_{5} & \leq & 160 \\
P_{2} & : & 0.7x_{1}+2.2x_{2}+1.6x_{3}+0.5x_{4} + 1.0x_{5} & \leq & 200 \\
P_{3} & : & 0.9x_{1}+0.7x_{2}+1.3x_{3}+1.0x_{4} + 0.8x_{5} & \leq & 120 \\
P_{4} & : & 1.4x_{1}+2.8x_{2}+0.5x_{3}+1.2x_{4} + 0.6x_{5} & \leq & 280 \\
\end{eqnarray}
```

```julia
# include -
using JuMP
using GLPK
using LinearAlgebra

# setup some constants -
number_of_products = 5;
number_of_processes = 4;

# Setup the system A, b and c -
# Define the system matrix -
A = [
    1.2 1.3 0.7 0.0 0.5 ;
    0.7 2.2 1.6 0.5 1.0 ;
    0.9 0.7 1.3 1.0 0.8 ;
    1.4 2.8 0.5 1.2 0.6 ;
];

b = [160, 200, 120, 280]; # Setup the right hand side vector
c_vector = [18.0,25.0,10.0,12.0,15.0]; # profits for each product

# build a model -
model = Model(GLPK.Optimizer)

# add decision variables to the model -
@variable(model, x[i=1:number_of_products] >= 0) # this sets up 5 variables

# Set the objective => maximize profit
@objective(model, Max, transpose(c_vector)*x);

# Setup the constraints - add them to the model 
@constraints(model, 
    begin
        A*x .<= b
    end
);

# solve the model
optimize!(model)
solution_summary(model)

# show the solution
for i in 1:number_of_products
    println("C$i = ",value(x[i]), " units per week.")
end

# print objective value -
println("Objective value for the primal is: ", objective_value(model), " \$ per week")
```

The maximum profit per week is $2988.72 per week.

__Source__: [Unit 3 examples, CHEME-1800 GitHub repository](https://github.com/varnerlab/CHEME-1800-4800-Course-Repository-S23/tree/main/examples/unit-3-examples/resource_allocation_primal_lp).

````

(content:references:dual-linear-problem)=
## Dual linear programs
The dual linear program, derived from the primal program, is an alternative way of looking at the same problem ({prf:ref}`defn-dual-linear program`):

````{prf:definition} Dual linear program
:label: defn-dual-linear program

Let $\mathcal{O}^{\prime}(\mathbf{y})$ denote a linear function of the continuous decision variables $\mathbf{y}\in\mathbb{R}^{n}$ 
whose values are constrained by a system of linear equations and bounded. Then, the optimal 
$\mathbf{y}^{\star}$ of the dual problem is a solution of the linear program:

$$
\begin{eqnarray*}
\text{minimize}~\mathcal{O}^{\prime}(\mathbf{y}) &=& \sum_{i=1}^{n} b_{i}\cdot{y}_{i}\\
\text{subject to}~\mathbf{A}^{T}\cdot\mathbf{y} &\geq&\mathbf{c}\\
\text{and}~y_{i}&\geq&{0}\qquad{i=1,2,\dots,n}
\end{eqnarray*}
$$

Each variable in the primal linear program becomes a constraint in the dual linear program, each constraint in the primal linear program becomes a variable in the dual linear program, and the objective direction is inverted – the maximum in the primal becomes the minimum in the dual, and vice versa. Particular differences between the primal and dual linear programming problems:
* Min versus max and the vectors $\mathbf{c}$ and $\mathbf{b}$ switch places; the 
$c_{j}$ coefficients become the right-hand side vector, while the $b_{i}$ are now in the objective function.
* The primal $\leq$ constraints become $\geq$ constraints in the dual problem.
* The dual problem is an upper bound to the primal solution, i.e., $\mathcal{O}^{\prime}(\mathbf{y}^{\star})\geq\mathcal{O}(\mathbf{x}^{\star})$ (weak duality).
````

### Duality interpretation
If we interpret the primal linear program as a classical resource allocation problem, its dual can be interpreted as a resource valuation problem. Thus, the duality theorem has an economic interpretation:

* The primal problem in {prf:ref}`example-resource-allocation-primal` deals with physical quantities, i.e., with production capacity (inputs) available in limited quantities and with the amounts of products (outputs) that should be produced to maximize total revenue. 

* On the other hand, the dual problem deals with economic values. With floor guarantees on all chemical product (output) unit prices and assuming the available quantity of all inputs (production capacity) is known, the dual problem computes the input unit pricing scheme that minimizes the total expenditure.

To explore this interpretation, let's formulate and solve the dual of the resource allocation problem above ({prf:ref}`example-resource-allocation-dual`):

````{prf:example} Product mix dual problem
:label: example-resource-allocation-dual
:class: dropdown

Consider another facility with no chemical process manufacturing capacity that instead wishes to purchase the production capacity of the previous factory. The dual decision variables $y_{i}$ are the offer prices per unit capacity.  For the offer to be accepted, $\mathbf{A}^{T}\mathbf{y}\geq\mathbf{c}$, i.e., facility one makes at least as much by selling the capacity as they do by manufacturing the chemical products.

The dual of {prf:ref}`example-resource-allocation-primal`, which computes the capacity prices $y_{i}$, can be solved as:

```julia
# include 
using JuMP
using GLPK
using LinearAlgebra

# setup some constants -
number_of_products = 5;
number_of_processes = 4;

# Setuo system -
c = [18.0,25.0,10.0,12.0,15.0]; # profits for each product

# System constraint matrix
A = [
    1.2 1.3 0.7 0.0 0.5 ;
    0.7 2.2 1.6 0.5 1.0 ;
    0.9 0.7 1.3 1.0 0.8 ;
    1.4 2.8 0.5 1.2 0.6 ;
];

b = [160, 200, 120, 280]; # Setup the right hand side vector

# build a model -
model = Model(GLPK.Optimizer)

# add decision variables to the model -
@variable(model, y[i=1:number_of_processes] >= 0) # this sets up 5 variables

# Set the objective => maximize the profit -
@objective(model, Min, transpose(b)*y);

# Setup the constraints - add them to the model 
@constraints(model, 
    begin
        transpose(A)*y .>= c # what is the .>= doing?
    end
);

# solve the model
optimize!(model)
solution_summary(model)

# show the solution
for i in 1:number_of_processes
    println("y$i = ",value(y[i]), " units per week.")
end

# print objective value -
println("Objective value for the dual is: ", objective_value(model))
```

The objective value of the dual problem is $2988.72 per week

__Source__: [Unit 3 examples, CHEME-1800 GitHub repository](https://github.com/varnerlab/CHEME-1800-4800-Course-Repository-S23/tree/main/examples/unit-3-examples/resource_allocation_primal_lp).

````

(content:references:max-min-linear-problem)=
## Minimum flow problems
One important application of linear programming is in the field of network flow problems. The network flow problem is a linear programming problem whose goal is to find the optimal flow of resources through a network of nodes and edges, subject to various constraints. One common type of network flow problem is the minimum flow problem, which seeks to find the minimum cost flow through a network that satisfies certain capacity constraints ({prf:ref}`defn-min-flow-problem`):

````{prf:definition} Minimum Flow Problem
:label: defn-min-flow-problem

Suppose we have a directed graph $G = (V, E)$ with a source node $s\in{V}$ and a sink node $t\in{V}$. Each edge $(u,v)\in{E}$ has a capacity $c(u,v)>0$, flow $f(u,v)>0$, and cost $a(u,v)>0$. The goal is to find the minimum cost flow from the source $s$ to the sink $t$ that satisfies the capacity constraints. The minimum flow problem can be formulated as a linear program:

$$
\begin{eqnarray}
\text{minimize}~\mathcal{O}(E) &=& \sum_{(u,v)\in{E}} a(u,v)\cdot{f}(u,v)\\
\text{subject to}~\sum_{w\in{E}}f(u,w) &=&0\quad{u}\neq{s,t}\\
\text{and}~\sum_{w\in{E}}f(s,w) &=&d\\
\text{and}~\sum_{w\in{E}}f(w,t) &=&d\\
\text{and}~f(u,v)&\leq&c(u,v)\quad{(u,v)\in{E}}
\end{eqnarray}
$$

where $d$ is the demand at the source and sink nodes, the decision variables $f(u,v)$ are the flow rates along the edges of the graph, and the objective function $\mathcal{O}$ is the total cost of the flow. Network flow problems are widely used in transportation, communication, and other fields to model and optimize the flow of resources through networks.
````

Modifications to the minimum flow problem described in {prf:ref}`defn-min-flow-problem`  can include adding additional constraints, such as upper and lower bounds on the flow rates, changing the objective function to minimize the total flow instead of the total cost, or having multiple sources and sinks in the network.

(content:references:flux-balance-analysis)=
## Flux balance analysis
In chemical engineering, flux balance analysis is a minimum flow problem that estimates chemical reaction rates (called fluxes) in steady-state reaction networks. The standard flux balance analysis problem is written in concentration units, e.g., the reaction flux has units of mmol/volume-time. However, sometimes, it is more convenient to work in mole units instead. In either case, the flux balance analysis problem can be formulated as a linear program ({prf:ref}`defn-mol-based-units`):

````{prf:definition} Flux balance analysis (mole-based units)
:label: defn-mol-based-units

Suppose we have an open well-mixed reactor system with species $\mathcal{M}$, streams $\mathcal{S}$, and reactions $\mathcal{R}$. Further, we partition the stream set $\mathcal{S}$ into streams entering the system $\mathcal{S}^{+}$, and streams leaving the system $\mathcal{S}^{-}$.  Then, the steady-state species mole balances are given by:

```{math}
\sum_{s\in\mathcal{S}^{+}}\dot{n}_{is} - \sum_{k\in\mathcal{S}^{-}}\dot{n}_{ik} + \sum_{j\in\mathcal{R}}\sigma_{ij}\dot{\epsilon}_{j} = 0\qquad\forall{i}\in\mathcal{M}
```

The optimal (unknown) open extents $\dot{\epsilon}_{j}$ are the solution of a linear programming problem 

$$
\begin{eqnarray}
\text{minimize}~\mathcal{O}(\dot{\epsilon}) & = &  -\sum_{j\in\mathcal{R}}c_{j}\dot{\epsilon}_{j}\\
\text{subject to}~\sum_{j\in\mathcal{R}}\sigma_{ij}\dot{\epsilon}_{j}&\geq&{-\sum_{s\in\mathcal{S}^{+}}\dot{n}_{i,s}}\qquad\forall{i}\in\mathcal{M}\\
\text{and}~\sum_{k\in\mathcal{S}^{-}}\dot{n}_{i,k}&\geq&{0}\qquad\forall{i}\in\mathcal{M}\\
\text{and}~\mathcal{L}_{j}&\leq\dot{\epsilon}_{j}\leq&\mathcal{U}_{j}\qquad\forall{j}\in\mathcal{R}
\end{eqnarray}
$$
````

If we don't have [kinetic rate laws](https://en.wikipedia.org/wiki/Rate_equation), or we are not approaching this problem from the [equilibrium or energy perspective](https://en.wikipedia.org/wiki/Chemical_equilibrium), we can use flux balance analysis to estimate the open extent of reaction $\dot{\epsilon}_{j}$ in mole-based systems, or the reaction rate in concentration based systems. 

---

## Summary
In this lecture, we discussed linear programming (LP).  Linear programming is a method to estimate the best outcome (such as maximum profit, lowest cost, or the best possible production rate) using a mathematical model composed of linear relationships subject to linear constraints. Linear programming is widely used in business, economics, engineering, and other fields to model and solve real-world problems involving resource allocation, production planning, transportation, and more.

First, we introduced the mathematical structure of linear programs, the dual problem, and then moved to an important application area within chemical Engineering, namely flux balance analysis:

* {ref}`content:references:primal-linear-problem` is a linear programming problem aiming to maximize or minimize a linear objective function subject to linear constraints in the form of inequalities or equalities. The term primal refers to the original form of the linear programming problem instead of its dual form.

* {ref}`content:references:dual-linear-problem` is a formulation derived from the primal linear program that provides an alternative perspective on the same problem by introducing a new set of variables and constraints. The objective of the dual program is to either maximize or minimize a linear function subject to a different set of constraints derived from the original primal program.

* {ref}`content:references:flux-balance-analysis` is a versatile tool that can estimate flows through various types of networks and graphs. It can be applied to social graphs, communication networks, or any other problem that can be represented as a network of graphs. Flux balance analysis, a testament to the versatility of linear programming, can be implemented as a linear program.

## Additional resources
Background resources for biochemical network information and computational tools for working with flux balance analysis models:

* [Kanehisa M, Goto S. KEGG: kyoto encyclopedia of genes and genomes. Nucleic Acids Res. 2000 Jan 1;28(1):27-30. doi: 10.1093/nar/28.1.27. PMID: 10592173; PMCID: PMC102409.](https://www.genome.jp/kegg/)

* [Karp, Peter D et al. "The BioCyc collection of microbial genomes and metabolic pathways." Briefings in bioinformatics vol. 20,4 (2019): 1085-1093. doi:10.1093/bib/bbx085](https://pubmed.ncbi.nlm.nih.gov/29447345/)

* [Gama-Castro, Socorro et al. "RegulonDB version 9.0: high-level integration of gene regulation, coexpression, motif clustering and beyond." Nucleic acids research vol. 44,D1 (2016): D133-43. doi:10.1093/nar/gkv1156](https://pubmed.ncbi.nlm.nih.gov/26527724/)

* [Heirendt, Laurent et al. "Creation and analysis of biochemical constraint-based models using the COBRA Toolbox v.3.0." Nature protocols vol. 14,3 (2019): 639-702. doi:10.1038/s41596-018-0098-2](https://pubmed.ncbi.nlm.nih.gov/30787451/)