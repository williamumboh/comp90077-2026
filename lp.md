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

## Visualizing LPs

We now visualize a small LP with two variables to gain some intuition.

Consider the following LP
$$\label{w9-lp-example}
\text{maximize} \quad & x_1 + 6x_2  \\
\text{subject to} \quad
& x_1   &\leq 20\\
&  x_2  &\leq 30\\
& x_1 +  x_2  &\leq 40\\
& x \geq 0$$

In the following, we will express solutions as points in 2 dimensions,
with $x_1$ along the horizontal axis and $x_2$ along the vertical axis,
and use the term *feasible region* to mean the set of feasible points.

### Feasible Region

Let's first visualize the set of nonnegative $x$ satisfying the first
two constraints: $x_1 \leq 20$ and $x_2 \leq 30$.

:::: {figure label=fig-lp-viz-box}

::: {image width=50%} ./lp-viz-box-constraints.png

:::

The shaded region is the set of nonnegative $x$ satisfying the
constraints $x_1 \leq 20$ and $x_2 \leq 30$. The shaded region is
bounded by the lines $x_1 = 20$ and $x_2 = 30$.

::::

Next, we visualize the set of nonnegative $x$ satisfying the constraint
$x_1 + x_2 \leq 40$. To do this, we first draw the line
$x_1 + x_2 = 40$. The points on the line satisfy the constraint exactly.
The line also divides the nonnegative points into two regions, the
points that satisfy $x_1 + x_2 > 40$ and those that satisfy
$x_1 + x_2 < 40$. The region containing $(0,0)$ is the one that
satisfies $x_1 + x_2 < 40$.

:::: {figure label=fig-lp-viz-diagonal}

::: {image width=50%} ./lp-viz-diagonal.png

:::

The shaded region is the set of nonnegative $x$ satisfying the
constraints $x_1 + x_2 \leq 40$. The shaded region is bounded by the
line $x_1 + x_2 = 40$.

::::

The feasible region is then the intersection of the regions in
@fig-lp-viz-box and @fig-lp-viz-diagonal.

:::: {figure label=fig-lp-viz-feasible}

::: {image width=50%} ./lp-viz-feasible.png

:::

The shaded region is the feasible region. The shaded region is bounded
by the lines $x_1 = 20$, $x_2 = 30$, and $x_1 + x_2 = 40$.

::::

### Objective Function

Now, we consider the objective function $x_1 + 6x_2$. The coefficients
of the objective function yields the direction vector $(1,6)$. The
objective function remains constant if we move orthogonally to the
direction vector while any other movement either decreases the objective
or increases the objective.

:::: {figure label=fig-lp-viz-objective}

::: {image width=50%} ./lp-viz-objective.png

:::

Illustration of the objective function $x_1 + 6x_2$. The dashed lines
are orthogonal to the direction vector and hence the points on the same
line have same objective function value.

::::

### Putting it all together

We now combine @fig-lp-viz-feasible and @fig-lp-viz-objective.

:::: {figure}

::: {image width=50%} ./lp-viz-overall.png

:::

The red point $(10,30)$ is the optimal solution with objective function
value $190$.

::::

The dashed line $x_1 + 6x_2 = 30$ contains all points whose objective
value is 30. Since the dashed line intersects the feasible region, we
know that there are feasible points with objective value 30.

As we shift the dashed line in the direction perpendicular to the line,
we get feasible points with higher objective value, until we reach the
line $x_1 + 6x_2 = 190$ which intersects the feasible region at a single
point $x^* = (10,30)$. We conclude that $x^*$ is the unique optimal
solution to the LP.

## Geometry of LPs

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

:::: {aside}

The feasible region of an LP is a polytope.

::: {image width=100%} ./lp-viz-feasible.png

:::

See [Wikipedia article on convex
polytopes](https://en.wikipedia.org/wiki/Convex_polytope) for other
examples of polytopes.

::::

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

:::: {aside}

For the example LP @w9-lp-example, note that the optimal solution
$x^* = (10,30)$ indeed has $n=2$ tight constraints: $x_2 \leq 30$ and
$x_1+x_2 \leq 40$.

::: {image width=100%} ./lp-viz-overall.png

:::

::::

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
