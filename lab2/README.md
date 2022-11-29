## A Genetic Algorithm for Set Covering Problem

<h4 align="center">
Abstract
</h4>
In this implementation I present a genetic algorithm based heuristic for the set covering problem. 
I propose a generic GA algorithm using numpy array as data structures, such that some procedures of the GA perform better in 
terms of time execution.

<h5>Introduction</h5>
The set covering problem is a NP-hard problem. In the formulation used in this implementation it consists in the following task:

Given a number $N$ and some lists of integers $P = (L_0, L_1, L_2, ..., L_n)$,
determine, if possible, $S = (L_{s_0}, L_{s_1}, L_{s_2}, ..., L_{s_n})$
such that each number between $0$ and $N-1$ appears in at least one list

$$
\forall n \in [0, N-1] \ \exists i : n \in L_{s_i}
$$

and that the total numbers of elements in all $L_{s_i}$ is minimum.

<h5>Genetic Algorithms</h5>
A genetic algorithm (GA) can be seen as an intelligent probabilistic search algorithm which can be applied to a variety of combinational optimizzation problems.
The idea of GAs is based on evolutionary process of biological organisms in nature. During the course of the evolution, populations evolve according to the principles of natural selection and survival of the fittest. Individual which are more successfully in adapting to their environment will have a better chance of surviving and reproducing, while individuals who are less fit will be eliminated. In this way, genes from highly fit individuals will be spread to an increasing number of individuals in each successive generation. The combination of good characteristics from highly adapted ancestor may produce even more fit offspring, such that species evolve to become more and more well adapted to their environment.

A GA emulates these processes starting by an initial population of individuals ad applying genetic operators in each reproduction. Each individual of the population is encoded onto a vector of integers or genome which represents a possible solution to a given problem. The fitness of an individual is evaluated with respect to a given objective function. Highly fit individuals or solutions are given the possibility to reproduce by exchanging pieces of their genetic information in a crossover procedure, with other highly fit individuals. This produces new offspring solutions, which share some characteristics taken from both parents. Mutation is often applied after crossover by altering some genes in the vector. The offspring population can either replace the whole population (generational approach) or replace less fit individuals (steady-state approach). This evaluation, selection, reproduction cycle is repeated until a satisfactory solution is found.

**Basic Steps of GA:**

1. Generate an initial population
2. Evaluate fitness of individuals in the population
3. repeat
   1. Select parents from the population
   2. Recombine parents to produce children
   3. Evaluate fitness of the children
   4. Replace some or all of the population by the children
4. until a satisfactory solution has been found

<h5>The GA description</h5>

**Representation and fitness function**

The first step is designing a genetic algorithm for this particular problem. The 0-1 binary representation is a good choice for the set covering problem since it represents the underlying 0-1 integer variables. I used a $n$-bit binary vector as the genome structure where $n$ is the cardinality of $P (n=|P|)$. A value of 1 for the $i$-th bit implies that the list $L_i$ is in the solution.

The binary representation of an individual's genome for the set covering problem is illustrated below.

<img src= genome_representation.jpg title="Genome Representation">

The fitness of an individual is related to its objective function value. With the binary representation the fitness $f_i$ of an individual is computed in this way:

$f_i$ = $(CE_i, N-c_i)$

Where $CE_i$ is the total number of unique element covered by the solution, $N$ is the problem size and $c_i$ is the sum of the length of the lists considered by the solution, in this case $N -c_i$ measures how far we are from the ideal solution.

An important issue concerning the use of the binary representation is that when applying genetic operators to the binary vectors, the resulting solutions are no longer guaranteed to be feasible, that's why I decided to include the total number of covered element in the fitness function. In fact this allows that there couldn't be solutions with $CE_i < N$.

**Parent selection techniques**

Parent selection is the task off assigning reproductive opportunities to each individual in the population. There are different methods used to select parents. I decided to implement the tournament selection. It works by forming two pools of individual, each consisting of T individuals drawn from the population randomly. Two individuals with the best fitness, each taken from one of the two tournament pools, are choosen for mating. Using a larger value for T has the effect of increasing selection pressure on the more fit individuals.

I chose the binary tournament selection (T=2) as the method for parent selection because it's efficient to implement.

**Crossover Operator**

The crossover operator works by randomly generating one or more crossover point(s) and the swapping segments ot the two parents vectors to produce two child vectors. The one-point crossover has been adopted in this implementation of the GA algorithm. Once the crossover point is generated $0<k<n$, the child reported as solution of the crossover operator is:  $C = P_1[0],...,P_1[k-1],P_2[k],...,P_2[n]$.

**Mutation Rate**

Mutation is applied to each child after crossover. It works by inverting some bit in the solution. Mutation is generally seen as a background operator which provides a small amount of random search. In this implementation I chose to use a fixed muation rate.

**Population Replacement**

Once a new feasible child solution has been generated, the child be added to the offspring generation and then the offspring for each generation will replace chosen member of the population (usually the one less fit). This type of replacement is called steady state replacement.

The advantages of this method is the fact that the best solutions are always kept in the population and the newly generated solution is immediately available for selection and reproduction.

When using a steady-state approach, care must be taken to prevent a duplicate solution from entering in the population. Allowing duplicate solutions to exist in the population may be undesirable because a population could come to consist of N identical solutions.

<h5>Parameters Tuning and Model Evaluation</h5>
In this implementation of the GA algorithm, some parameter have been tuned to provide better results. This is a list of tuned parameters: POPULATION_SIZE, N_GENERATIONS, MUTATION_RATE.

To evaluate the model I decided to use as evaluation metrics the number of total covered element considering the duplicates. This metric is called WEIGHT.

<h5> Computational Results </h5>


| N    | WEIGHT | BLOAT | POPULATION_SIZE | N_GENERATIONS | MUTATION_RATE |
| ---- | ------ | ----- | --------------- | ------------- | ------------- |
| 5    | 5      | 0%    | 10              | 400           | 0.3           |
| 10   | 10     | 0%    | 40              | 400           | 0.3           |
| 20   | 24     | 20%   | 80              | 500           | 0.2           |
| 100  | 181    | 81%   | 200             | 500           | 0.35          |
| 500  | 1491   | 198%  | 1000            | 800           | 0.35          |
| 1000 | 3542   | 254%  | 1000            | 900           | 0.35          |

<h5>Future Works </h5>

1. Implement a crossover operators fitness-based
2. Use a variable mutation rate and a variable number of bit mutation
