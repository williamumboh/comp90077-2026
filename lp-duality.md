(sec-lp-duality)=

# Linear Programming Duality

We introduce linear programming duality, show that the vertex cover and
matching LPs are dual to each other, discuss the LP duality theorems and
use them to give an alternate analysis of the 2 approximation algorithm
for vertex cover.

## Warm up

Consider the following specific linear program.
$$\label{lp-example}
\text{maximize} \quad & x_1 + 6x_2  \\
\text{subject to} \quad
& x_1   &\leq 20\\
&  x_2  &\leq 30\\
& x_1 +  x_2  &\leq 40\\
& x \geq 0$$

Let us try to derive upper bounds on $\operatorname{OPT}$ the optimal
value of the LP using its inequalities.

### Multiplying a single inequality

Multiplying the third inequality by 6 yields the inequality
$$6x_1 + 6x_2 \leq 240.$$
Since the objective is $x_1 + 6x_2$ and
$$x_1 +6x_2 \leq 6x_1 + 6x_2$$
for $x \geq 0$, we conclude that $\operatorname{OPT} \leq 240$.

### Linear combinations of inequalities

Can we do better? We can also take linear combinations of the
inequalities. For example, we can multiply the inequality $x_2 \leq 30$
by 6 and add it with the inequality $x_1 \leq 20$ to obtain
$$x+1 + 6x_2 \leq 20 + 180 = 200.$$

We can do even better by multiplying the inequality $x_2 \leq 30$ by 5
and adding it to $x_1 + x_2 \leq 40$ to obtain
$$\label{lp-example-ub}
x_1 + 6x_2 \leq 150 + 40 = 190.$$

This is the best possible upper bound on $\operatorname{OPT}$. The
feasible solution $x$ with $x_1 = 10$ and $x_2 = 30$ has objective value
$190$, which implies that $\operatorname{OPT} \geq 190$. Together with
the upper bound in Inequality @lp-example-ub, we conclude that
$\operatorname{OPT} = 190$.

### Minimum Upper Bound = Dual Linear Program

Observe that the problem of finding the minimum upper bound by taking
linear combinations is yet another linear program! The new linear
program is the dual linear program, and the original linear program is
called the primal linear program.

In the dual, we have a variable for each constraint (excluding the
nonnegativity constraints) which corresponds to the amount we multiply
the constraint (i.e. its coefficient in the linear combination):

- $y_1$ for the constraint $x_1 \leq 20$
- $y_2$ for the constraint $x_2 \leq 30$
- $y_3$ for the constraint $x_1 + x_2 \leq 40$

For example, the multiplier $y_3$ gives us the inequality
$$y_3(x_1+x_2) \leq 40y_3.$$
Observe that if $y_3$ is negative, then the inequality is flipped. Thus,
all the multipliers need to be nonnegative.

The linear combination of the constraints using coefficients
$y_1, y_2,y_3$ gives the inequality
$$y_1(x_1) + y_2(x_2) + y_3(x_1+x_2) \leq 20 y_1 + 30 y_2 + 40 y_3.$$
Rewriting gives us
$$\label{lp-example-comb}
(y_1+y_3)x_1 + (y_2+y_3)x_2 \leq 20 y_1 + 30 y_2 + 40 y_3.$$

If $y$ satisfies the inequalities
$$y_1 + y_3 \geq 1 \\
y_2 + y_3 \geq 6$$
then since $x \geq 0$, Inequality @lp-example-comb yields an upper bound
on $\operatorname{OPT}$:
$$x_1 + 6x_2
&\leq (y_1+y_3)x_1 + (y_2+y_3)x_2 \\
&\leq 20 y_1 + 30 y_2 + 40 y_3.$$

Thus, the dual LP is
$$\label{lp-example-dual}
\text{minimize} \quad & 20 y_1 + 30 y_2 + 40 y_3  \\
\text{subject to} \quad
& y_1 + y_3   &\geq 1\\
& y_2 + y_3  &\geq 6\\
& y \geq 0$$

The optimal solution to the dual is $y^*_1 =0$, $y^*_2=5$, y^\*^~3~=1\$
with value $190$.

## Duals in General

Let us now consider a general canonical LP
$$\label{lp-max}
\text{maximize} \quad & \sum_{1 \leq j \leq n} a_j x_j\\
\text{subject to} \quad & \sum_{1 \leq j \leq n} b_{ij} x_j \leq c_i && \forall 1 \leq i \leq m\\
& x \geq 0$$
and let $\operatorname{OPT-Primal}$ be its maximum value.

The dual LP is the problem of finding the minimum upper bound to
$\operatorname{OPT-Primal}$ by taking linear combinations of the
constraints of the primal LP. The dual LP has:

- variable $y_i$ for the $i$-th primal constraint
- constraint $\sum_{1 \leq i \leq m} b_{ij} y_i \geq a_j$ for the primal
  variable $x_j$

$$\label{lp-min}
\text{minimize} \quad & \sum_{1 \leq i \leq m} c_i y_i\\
\text{subject to} \quad & \sum_{1 \leq i \leq m} b_{ij} y_i \geq a_j && \forall 1 \leq j \leq n\\
& y \geq 0$$

What about the dual of minimization LPs? The same principle applies: the
dual of a minimization LP is the problem of finding the maximum lower
bound on the minimum value of the primal LP by taking linear
combinations of the primal LP constraints. In particular, the dual of LP
@lp-min is LP @lp-max. We say that LP @lp-max and LP @lp-min form a
*primal-dual pair*.

More succinctly, the following LPs are a primal-dual pair
$$\text{maximize} \quad & a^\intercal x\\
\text{subject to} \quad & Bx \leq c \\
& x \geq 0$$

$$\text{minimize} \quad & c^\intercal y\\
\text{subject to} \quad & B^\intercal y \geq a \\
& y \geq 0$$

Note that $c$, the RHS of the primal constraints, becomes the objective
vector for the dual problem.

## Example: Vertex Cover and Matching

We will now show that vertex cover and matching are dual problems in the
sense that their LPs form a primal-dual pair.

Recall the matching LP. For each vertex $v$, let $\delta(v)$ be the set
of edges incident to $v$.
$$\text{maximize} \quad & \sum_e x_e\\
\text{subject to} \quad & \sum_{e \in \delta(v)} x_e \leq 1 \quad  && \forall v \in V\\
& x \geq 0$$

To obtain the dual LP:

1.  For each primal constraint $\sum_{e \in \delta(v)} x_e \leq 1$, we
    introduce a dual variable $y_v$.
2.  Since the RHS of every primal constraint is 1, each $y_e$ has a
    coefficient of 1 in the dual objective. Thus, the dual objective is
    $\sum_e y_e$
3.  For each primal variable $x_e$ (let $e = (u,v)$), we introduce a
    dual constraint $y_u + y_v \geq 1$. This is because in the primal,
    the coefficient of $x_e$ is 1 in the objective and the variable
    $x_e$ appears with a coefficient of $1$ in the constraints for $u$
    and $v$.

Thus, the dual LP is
$$\text{minimize} \quad & \sum_v y_v\\
\text{subject to} \quad & y_u + y_v \geq 1 \quad  && \forall (u,v) \in E\\
& y\geq 0$$
This is exactly the LP for vertex cover!

## LP Duality Theorems

Consider a primal-dual pair where the primal is a maximization LP. The
Weak LP Duality Theorem formalizes the intuition that the dual gives an
upper bound on the optimal primal.

::: {prf:theorem label=lp-weak-duality} Weak LP Duality

Let $x$ and $y$ be feasible solutions to LP @lp-max and its dual LP
@lp-min, respectively. Then,
$$\sum_{1 \leq j \leq n} a_j x_j \leq \sum_{1 \leq i \leq m} c_i y_i.$$

:::

::: {prf:proof enumerated=false}

Feasibility of $x$ and $y$ gives
$$\label{lp-duality-x}
\sum_{1 \leq j \leq n} b_{ij}x_j \leq c_i \qquad \forall 1 \leq i \leq m$$
and
$$\label{lp-duality-y}
a_j \leq \sum_{1 \leq i \leq m} b_{ij}y_i \qquad \forall 1 \leq j \leq n$$
respectively.

Applying Inequality @lp-duality-y and nonnegativity of $x$, we get
$$\sum_{1 \leq j \leq n} a_j x_j
\leq \sum_{1 \leq j \leq n} \left(\sum_{1 \leq i \leq m} b_{ij}y_i\right) x_j$$
Changing the order of summation gives
$$\sum_{1 \leq j \leq n} \left(\sum_{1 \leq i \leq m} b_{ij}y_i\right) x_j
= \sum_{1 \leq i \leq m} \left(\sum_{1 \leq j \leq n} b_{ij}x_j\right)y_i.$$
Finally, we apply Inequality @lp-duality-x and nonnegativity of $y$ to
get
$$\sum_{1 \leq i \leq m} \left(\sum_{1 \leq j \leq n} b_{ij}x_j\right)y_i
\leq \sum_{1 \leq i \leq m} c_iy_i.$$

:::

It is not *a priori* clear whether there is a gap between the minimum
upper bound obtained via feasible dual solutions and the value of the
primal optimal solution.

For example, we saw that there exist graphs in which the maximum
matching is strictly smaller than the minimum vertex cover. It turns out
that the "duality gap" only holds for integral solutions, and disappears
for fractional solutions.

The main result in the theory of linear programs is that for every
primal-dual pair, the optimal values of the primal and dual coincide.

::: {prf:theorem label=lp-strong-duality} Strong LP Duality

Let $x^*$ and $y^*$ be optimal solutions to LP @lp-max and its dual LP
@lp-min, respectively. Then,
$$\sum_{1 \leq j \leq n} a_j x^*_j \leq \sum_{1 \leq i \leq m} c_i y^*_i.$$

:::

The proof is quite beautiful but outside the scope of the subject, due
to time constraints. I highly recommend [Understanding and Using Linear
Programming](https://link.springer.com/book/10.1007/978-3-540-30717-4)
as they have proofs from several different angles, including a "proof by
physics" that gives good intuition.

For approximation algorithms, we mainly need weak LP duality. In
particular, for minimization problems such as vertex cover and set
cover, we only need the fact that the dual LP gives a lower bound on the
optimal fractional solution.

## Dual-Fitting for Vertex Cover

Dual-fitting is a method to analyze the approximation ratio of an
algorithm. For minimization problems, the general idea is that to show
that the solution constructed by an algorithm has an approximation ratio
of $\alpha$, we construct a dual solution $y$ such that:

- $y$ "pays" for the primal solution constructed by the algorithm, i.e.
  the value of $y$ is at least that of the cost of the primal solution
  and
- $y$ is $α$-approximately feasible, i.e. $y$ violates the dual
  constraints by at most a factor of $\alpha$

Let us see this in action and give a dual-fitting analysis for 2
approximation algorithm for vertex cover. First, let's recall the vertex
cover LP and its dual, the matching LP
$$\text{minimize} \quad & \sum_v y_v\\
\text{subject to} \quad & y_u + y_v \geq 1 \quad  && \forall (u,v) \in E\\
& y\geq 0$$

$$\text{maximize} \quad & \sum_e x_e\\
\text{subject to} \quad & \sum_{e \in \delta(v)} x_e \leq 1 \quad  && \forall v \in V\\
& x \geq 0$$

Recall the $2$-approximation algorithm for vertex cover: find a maximal
matching $M$ and output $C$ where $C$ contains the endpoints of the
edges in $M$.

Since $C$ has exactly 2 vertices for each edge $e \in M$, we construct
the dual solution $y$ where $y_e = 2$ for $e \in M$ and $y_e = 0$ for
$e \notin M$.

The dual solution $y$ pays for $C$:
$$\label{eq-dual-vc-pays}
| C | = 2 | M | = ∑_{e ∈ E} y_e. |$$

We now show that it is $2$-approximately feasible. For each edge
$e \in E$, we have
$$\sum_{e \in \delta(v)} y_e \leq 2$$
since the edges $e$ with $y_e = 2$ is a matching.

Thus, the dual solution $y'$ where $y'_e = y_e/2$ is feasible for the
dual LP. By weak duality, $\sum_{e \in E} y'_e$ is a lower bound on the
optimal fractional vertex cover and thus
$\sum_{e \in E} y'_e \leq |C^*|$ where $C^*$ is the optimal vertex
cover.

Together with Equation @eq-dual-vc-pays, we get
$$| C |
= 2 ∑_{e ∈ E} y'_e
≤ 2|C^*|,$$
as desired.
