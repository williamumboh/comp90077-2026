(sec-lp)=

# Linear Programming

So far, we have discussed how to use linear programs as a black box to
design approximation algorithms. We will now open the black box and give
an overview of the theory of linear programs.

## LP Basics

A linear program is an optimization problem in which we are given a
linear objective function and linear constraints, and the goal is to
find a non-negative real vector $x$ satisfying the constraints that
minimizes the objective function. Formally, an LP with $n$ variables and
$m$ constraints consists of coefficients $a_j$, $b_{ij}$ and $c_i$ for
$1 \leq i \leq m$ and $1 \leq j \leq n$ and the goal is to find an
optimal solution to the following optimization problem:
$$\label{canonical-form}
\text{maximize} \quad & \sum_{1 \leq j \leq n} a_j x_j\\
\text{subject to} \quad & \sum_{1 \leq j \leq n} b_{ij} x_j \leq c_i && \forall 1 \leq i \leq m\\
& x_j \geq 0 && \forall 1 \leq j \leq n$$

::: {aside}

The LP in @canonical-form called a *canonical LP*.[^1] Every linear
program including minimization ones can be converted to an equivalent
canonical LP. In particular, there is a mechanical procedure that does
the conversion. For example, converting minimization to maximization can
be done easily by multiplying the objective coefficients $a_j$ by $-1$.
We skip the rest of the details of the procedure and the proof as they
are somewhat tedious.

:::

We will often abbreviate the above by expressing the coefficients $a_j$
using the vector $a \in \mathbb{R}^n$, the constraints $b_{ij}$ as the
matrix $B \in \mathbb{R}^{m \times n}$, and $c_i$ as the vector
$c \in \mathbb{R}^m$.
$$\text{maximize} \quad & a^\intercal x\\
\text{subject to} \quad & Bx \leq c \\
& x \geq 0$$

::: {prf:definition label=def-tight}

Let $x$ be a solution to an LP. We say that constraint $i$ is *tight* if
it is satisfied exactly, i.e.
$$\sum_{1 \leq j \leq n} b_{ij} x_j = c_i.$$
and *slack* otherwise, i.e.
$$\sum_{1 \leq j \leq n} b_{ij} x_j < c_i.$$

:::

::: {prf:definition label=def-infeasible-unbounded}

An LP is *infeasible* if there is no feasible solution, i.e. there is no
$x \in \mathbb{R}^n$ satisfying every constraint
$\sum_{1 \leq j \leq n} b_{ij} x_j \leq c_i$.

It is *unbounded* if the value of the optimal solution is $\infty$.

:::

### Notational conventions for LPs

Unless otherwise specified, we will use:

- $n$ to mean the number of variables and $j$ to index variables, and
- $m$ to denote the number of constraints and $i$ to index constraints.

## Geometry of LPs

See pages 7, 8, 9 of [Annotated Slides](./slides-post-w9.pdf) for
examples of the definitions below.

::: {prf:definition label=def-polytope}

Let $b \in \mathbb{R}^n$ and $c \in \mathbb{R}$. Then, the set of points
$x \in \mathbb{R}^n$ satisfying the inequality $b^\intercal x \leq c$ is
called a *halfspace* and those that satisfy it exactly, i.e.
$b^\intercal x = c$ is a *hyperplane*.

A *polytope*[^2] is the set of points in the intersection of halfspaces.
The boundary of the polytope is a hyperplane or an intersection of
hyperplanes.

Equivalently, a polytope is the set of points satisfying a system of
linear inequalities[^3]. A point is on the boundary of the polytope if
and only if it satisfies at least one linear inequality exactly.

:::

See [Wikipedia article on convex
polytopes](https://en.wikipedia.org/wiki/Convex_polytope) for other
examples of polytopes.

Given an LP, we will also refer to solutions as "points", the set of
feasible solutions as the "feasible region" and the "polytope of the
LP".

## Intuition for Solving LPs

To give some intuition for solving LPs and what the optimal solution
looks like (which will be helpful for duality later), we now describe a
continuous process to solve LPs.

Consider a canonical LP @canonical-form with objective vector $a$ and
constraints $Bx \leq c, x \geq 0$. The continuous process is as follows:

1.  start from an initial point $x_0$ in the polytope
    $\{x : Bx \leq c, x \geq 0\}$;
2.  continuously move in a direction that improves our objective
    function while remaining inside the polytope until we cannot, at
    which point the process stops.

Initially, if no constraints are tight for the initial point $x_0$, then
we move in the direction $a$ until we hit the boundary of the polytope,
i.e. some constraints become tight. Then, we try to move along the
boundary while improving the objective function, i.e. the tight
constraints remain tight. As the process continues, the set of tight
constraints grows until finally we reach a point where we cannot improve
the objective function without moving out of the polytope and the
process terminates.

The proof that the final point is an optimal solution to the LP is
outside the scope of this subject. The main takeaway point is that the
final point has $n$ tight constraints.

::: {prf:definition label=def-basic-feas} Basic feasible solutions

A solution $x$ to an LP is a *basic feasible solution* if it has exactly
$n$ tight constraints.

:::

::: {prf:theorem label=thm-lp-bfs}

If an LP is feasible and bounded, then there is always an optimal
solution that is basic feasible.

:::

## Algorithms for LPs

We briefly discuss two important algorithms for solving LPs: Simplex
Method and Ellipsoid Method.

### Simplex Method

At a high level, the Simplex Method works as follows:

1.  Start with some basic feasible solution
2.  Move to a better "neighboring" basic feasible solution.
3.  If no better neighbor exists, return current solution.

In practice, the Simplex Method is very fast. However, it is not known
how to execute step 2 (called the "pivot step") so that the overall
running time is polynomial in the worst case. Finding a good theoretical
framework that explains the practical effectiveness of Simplex and other
algorithms that work much better in practice than predicted by
worst-case analysis is a major research direction in Algorithms.

### Ellipsoid Method

The Ellipsoid Method is completely different. First, suppose that we
have a procedure that given $k$, outputs a feasible solution whose
objective value is at least $k$ if one exists, and outputs "No"
otherwise. Then, we can solve the LP by doing a binary search for the
optimal value of the LP.

Next, observe that a feasible solution $x$ with objective value at least
$k$ satisfies the inequalities
$$a^\intercal x \geq k\\
Bx \leq c\\
x \geq 0$$
which in turn is yet another polytope! Thus, to solve the LP, it
suffices to be able to have a procedure that outputs a point in the
polytope if the polytope is nonempty, or outputs that the polytope is
empty.

At the heart of the Ellipsoid Method is a separation oracle for the LP.

::: {prf:definition label=def-separation}

A *separation oracle* for an LP takes as input a point $x \geq 0$ and
determines if $x$ is feasible, and if it is not feasible, outputs a
violated constraint.

:::

At a high level, the Ellipsoid Method uses the separation oracle to do
some sort of binary search: Further details of the Ellipsoid Method and
its analysis is beyond the scope of this subject. The main takeaway is
that we can efficiently solve an LP with exponentially many constraints
as long as we can give an efficient separation oracle.

::: {prf:theorem label=thm-separation}

Given a linear program with $n$ variables and a separation oracle with
running time $T(n)$, an optimal solution can be found in time polynomial
in $n$ and $T(n)$.

:::

An immediate application is that for the Min $(s,t)$-Cut
[problem](#prob-st-cut), we can efficiently solve
$\operatorname{Path-LP}$ described in [Exercise 1 of Tutorial
8](#ex-8-1) by using a shortest-path algorithm (e.g. Dijkstra's) as a
separation oracle.

::: {prf:theorem label=thm-path-separation}

There is a polynomial-time separation oracle for
$\operatorname{Path-LP}$.

:::

::: {prf:proof enumerated=false}

Let $x$ be a solution to $\operatorname{Path-LP}$. We interpret $x_e$ as
the length of edge $e$. The constraints of the LP are that
$$\sum_{e \in P} x_e \geq 1$$
for every $(s,t)$-path $P$. Thus, there is a violated constraint if and
only if the shortest $(s,t)$-path $P$ has length at most $1$. Therefore,
we can simply compute the shortest $(s,t)$-path $P^*$ and output it as a
violated constraint if its length is less than $1$, and otherwise, $x$
is feasible.

:::

[^1]: Also called *standard LP*. Ironically, the naming is neither
    standard nor canonical.

[^2]: Also called a *polyhedron* (singular), *polyhedra* (plural).

[^3]: Here, "system" means "set".
