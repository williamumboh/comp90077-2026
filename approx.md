(sec-approx)=

# Approximation Algorithms and Reductions

We introduce the notion of approximation algorithms, and we formally
introduce the concept of reductions that we were implicitly using in
@sec-matroids and @sec-tut2. In the rest of the semester, we will focus
on approximation algorithms and we will also see reductions quite
frequently.

## Approximation Algorithms

Many interesting combinatorial problems are known to be NP-hard, and
thus are widely believed not to have algorithms that are mathematically
guaranteed to find the optimal solution on every instance in polynomial
time. However, we still have to solve these problems. One way to get
around the obstacle of NP-completeness is to relax the pursuit of
finding the optimal solution and settle on finding a near-optimal one
instead.

We begin by more precisely defining problems and instances.

::: {prf:definition label=prob-comb-2} Combinatorial Optimization
Problem

A *combinatorial optimization problem* $\Pi$ consists of instances,
where each *instance* $I \in \Pi$ consists of:

- a finite set of elements $E$;
- an *objective function* $g : 2^E \rightarrow \mathbb{R}$ (i.e. $g$ is
  a function that maps subsets of $E$ to real numbers);
- a collection $\mathcal{F} \subseteq 2^E$ of subsets of $E$. The sets
  in $\mathcal{F}$ are called *feasible sets* and also *feasible
  solutions*.

If $\Pi$ is a maximization problem, then the goal is to find a feasible
set $S \in \mathcal{F}$ that maximizes $g(S)$. If it is a minimization
problem, then the goal is to minimize $g(S)$. We can more succinctly
represent an instance as follows:
$$\max_{S \in \mathcal{F}} g(S) \qquad \text{ or } \qquad \min_{S \in \mathcal{F}} g(S).$$

:::

We use $\operatorname{OPT}(I) = \max_{S \in \mathcal{F}} g(S)$ for
maximization problems and
$\operatorname{OPT}(I) = \max_{S \in \mathcal{F}} g(S)$ for minimization
problems.

For example, an instance $I$ of the Minimum Spanning Tree problem
consists of a graph $G = (V,E)$ and weights on edges. The feasible set
is the set of spanning trees. Then, $\operatorname{OPT}(I)$ is the
weight of the minimum spanning tree.

::: {prf:definition label=def-approx} Approximation Algorithm

A *$c$-approximation algorithm* for a problem $\Pi$ is one that on
**every** instance $I \in \Pi$, runs in **polynomial time** in the input
length of $I$ and outputs a feasible solution $S$ with:

- $g(S) \leq c \cdot \operatorname{OPT}(I)$ if $\Pi$ is a minimization
  problem;
- $g(S) \geq c \cdot \operatorname{OPT}(I)/c$ if $\Pi$ is a maximization
  problem.

The number $c$ is said to be the *approximation ratio* (or
*approximation factor*) of the algorithm. When $c=1$, the algorithm is
said to be an *exact algorithm*.

:::

## Reductions

Intuitively, a reduction from problem $\Pi$ to problem $\Pi'$ that lets
us obtain an algorithm for $\Pi$ from an algorithm for problem $\Pi'$.

::: {prf:definition label=def-reduction} Reduction

A *reduction* from problem $\Pi$ to problem $\Pi'$ is a function $\phi$
that maps every instance $I$ of $\Pi$ to an instance $I'$ of $\Pi'$ such
that $\operatorname{OPT}(I) = \operatorname{OPT}(I')$. We say that $\Pi$
is *reducible to* $\Pi'$ if a reduction exists.

:::

For example, we solved the @prob-sched-deadlines problem by reducing it
to the max-weight basis problem.
