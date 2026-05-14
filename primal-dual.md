(sec-primal-dual)=

# Primal-Dual Approximation Algorithms

We now turn to the more powerful primal-dual framework for approximation
algorithms. Dual-fitting gave us a way of analyzing a solution. At a
high level, the primal-dual framework constructs the primal and dual
solutions in tandem. The key to the primal-dual framework are the
complementary slackness conditions, which characterize when a pair
feasible primal and dual solutions are optimal.

## Complementary Slackness Conditions

Consider the following primal-dual pair of LPs.

$$\begin{align*}
\label{w10-lp-max}
\text{maximize} \quad & \sum_{1 \leq j \leq n} a_jx_j\\
\text{subject to} \quad & \sum_{1 \leq j \leq n} b_{ij} x_j \leq c_i && \forall 1 \leq i \leq n\\
& y \geq 0
\end{align*}$$

$$\begin{align*}
\label{w10-lp-min}
\text{minimize} \quad & \sum_{1 \leq i \leq m} c_i y_i\\
\text{subject to} \quad & \sum_{1 \leq i \leq m} b_{ij} y_i \geq a_j && \forall 1 \leq j \leq m\\
& x \geq 0
\end{align*}$$

At a high level, $x^*$ and $y^*$ are optimal for their respective LPs if
and only if:

- positive primal variables correspond to tight dual constraints,
- positive dual variables correspond to tight primal constraints

::: {prf:definition label=def-CS} Complementary Slackness

Let $x$ and $y$ be a pair of solutions to primal and dual LPs
@w10-lp-max and @w10-lp-min, respectively. We say that the pair
satisfies *primal complementary slackness* if for every
$1 \leq j \leq n$,

$$\begin{equation*}
\label{eq:primal-CS}
\tag{Primal CS}
x_j > 0 \implies \sum_{1\leq i \leq m} b_{ij} y_i = a_j.
\end{equation*}$$

We say that they satisfy *dual complementary slackness* if for every
$1 \leq i \leq m$,

$$\begin{equation*}
\label{eq:dual-CS}
\tag{Dual CS}
y_i > 0 \implies \sum_{1 \leq j \leq n} b_{ij} x_j = c_i.
\end{equation*}$$

:::

::: {prf:theorem label=thm-CS}

The solutions $x^*$ and $y^*$ are optimal for their respective LPs if
and only if they satisfy [primal and dual complementary slackness
conditions](#def-CS).

:::

The proof is outside the scope of the subject. Instead, we will get some
intuition from an example, and prove the direction that is relevant for
approximation algorithms.

### Example and Intuition

Let us try to get a feel for @thm-CS. Consider the following LP and its
dual

$$\begin{align*}
\label{w10-lp-example}
\text{maximize} \quad & x_1 + 6x_2  \\
\text{subject to} \quad
& x_1   &\leq 20\\
&  x_2  &\leq 30\\
& x_1 +  x_2  &\leq 40\\
& x \geq 0
\end{align*}$$

$$\begin{align*}
\label{w10-lp-example-dual}
\text{minimize} \quad & 20 y_1 + 30 y_2 + 40 y_3  \\
\text{subject to} \quad
& y_1 + y_3   &\geq 1\\
& y_2 + y_3  &\geq 6\\
& y \geq 0
\end{align*}$$

We can visualize LP @w10-lp-example using @w10-lp-viz.

:::: {figure label=w10-lp-viz}

::: {image width=75%} ./lp-viz-overall.png

:::

Visualization of LP @w10-lp-example. The black arrow is the direction of
the objective vector, the colored lines are the "boundaries" of the
constraints, the red point is the optimal solution.

::::

Consider the feasible solution $x^*_1 = 10, x^*_2 = 30$. By [weak LP
duality](#lp-weak-duality), to prove that it is optimal, it suffices to
find a feasible dual solution $y^*$ with the same value. How do we find
$y^*$?

Intuitively, $x^*$ is optimal if and only if its tight constraints
prevent us from moving to another feasible point with better objective
value. Looking at @w10-lp-viz, the points with better objective value
are either infeasible for the constraint $x_2 \leq 30$ or the constraint
$x_1 + x_2 \leq 40$, and these are the tight constraints for $x^*$.

Since the dual solution correspond to linear combinations of primal
constraints, it makes intuitive sense that we should only use dual
variables that correspond to tight primal constraints.

In this example, $y_1$ corresponds to $x_1 \leq 20$, $y_2$ corresponds
to $x_2 \leq 30$ and $y_3$ corresponds to $x_1 + x_2 \leq 40$. Since
$x^*$ satisfies the 2nd and 3rd constraints exactly, the above reasoning
leads us to expect to find a feasible dual $y^*$ with $y^*_1=0$. Indeed,
the dual solution $y^*_1 = 0, y^*_2 = 5, y^*_3=1$ satisfies these
properties. Since $y^*$ has the same value as $x^*$, by [weak LP
duality](#lp-weak-duality), $x^*$ and $y^*$ are optimal for their
respective LPs @w10-lp-example and @w10-lp-example-dual. Moreover, they
also satisfy the complementary slackness conditions.

### Applying Complementary Slackness Conditions

We now prove the direction of @thm-CS that is useful for approximation
algorithms.

::: {prf:lemma label=thm-CS-weak}

If $x^*$ and $y^*$ are feasible primal and dual solutions satisfying
primal and dual complementary slackness conditions, then they are
optimal.

:::

::: {prf:proof enumerated=false}

Since $x^*$ is nonnegative, the value of $x^*$ can be expressed as
follows:
$$\sum_{1 \leq j \leq n} a_jx^*_j = \sum_{j : x^*_j > 0} a_jx^*_j.$$

Primal complementary slackness (see @thm-CS) and nonnegativity of $y^*$
implies

$$\begin{align*}
\sum_{j : x^*_j > 0} a_j x^*_j
&= \sum_{j : x^*_j > 0} \left(\sum_{1 \leq i \leq m} y^*_i\right) x^*_j \\
&= \sum_{j : x^*_j > 0} \left(\sum_{i : y^*_i > 0} y^*_i\right) x^*_j \\
&= \sum_{i : y^*_i > 0} \left(\sum_{j : x^*_j > 0} x^*_j\right)y^*_i,
\end{align*}$$

where the last equality follows by swapping the order of summation.

Dual complementary slackness conditions (see @thm-CS) gives

$$\begin{align*}
\sum_{i : y^*_i > 0} \left(\sum_{j : x^*_j > 0} x^*_j\right)y^*_i
= \sum_{i : y^*_i > 0} c_i y^*_i\\
= \sum_{1 \leq i \leq m} c_i y^*_i,
\end{align*}$$

where the last equality follows from nonnegativity of $y^*$. We conclude
that the values of $x^*$ and $y^*$ are equal, and therefore, by [weak LP
duality](#lp-weak-duality), both are optimal.

:::

(sec-pd-setcover)=

## Warm-Up: Set Cover

We first show a simple application of the primal-dual framework to
obtain an $f$-approximation for Weighted Set Cover, where $f$ is the
frequency[^1] of the set system.

Recall that in Weighted Set Cover, we have a universe $U$ of $n$
elements, a collection of sets $S_1, \ldots, S_m \subseteq U$ where set
$S_i$ costs $w_i$. The goal is to find a collection of sets that cover
$U$ with minimum total cost.

The LP and its dual are:

$$\begin{align*}
\text{minimize} \quad & \sum_i w_ix_i\\
\text{subject to} \quad & \sum_{i: S_i \ni e} x_i \geq 1 \quad  && \forall e \in U\\
& x \geq 0
\end{align*}$$

$$\begin{align*}
\text{maximize} \quad & \sum_e y_e\\
\text{subject to} \quad & \sum_{e \in S_i} y_e \leq w_i \quad  && \forall i \in [m]\\
& y \geq 0
\end{align*}$$

Suppose we have an integral primal solution $X$ (i.e. $X_i = 1$ means we
buy set $S_i$) and a feasible dual solution $y$. Then, if they satisfy
primal complementary slackness conditions, we have that

$$\begin{equation}
\label{eq:set-cover:primal-cs}
X_i = 1 \implies \sum_{e \in S_i} y_e = w_i
\end{equation}$$

which we can interpret to mean that the primal solution only buys the
set $S_i$ if the dual variables of the elements in $S_i$ can pay for the
cost of $S_i$. On the other hand, if they satisfy dual complementary
slackness conditions, we have

$$\begin{equation}
y_e > 0 \implies \sum_{i : S_i \ni e} X_i = 1,
\end{equation}$$

which intuitively means that each dual variable is used to pay for at
most one set $S_i$, i.e. they are not overcharged.

We say that they satisfy $α$-approximate dual complementary slackness
if:

$$\begin{equation}
\label{eq:set-cover:dual-cs}
y_e > 0 \implies \sum_{i : S_i \ni e} X_i \leq \alpha,
\end{equation}$$

which implies that each dual variable is used to pay for at most
$\alpha$ sets and is thus overcharged at most $\alpha$ times.

Suppose $X,y$ satisfy [primal complementary
slackness](#eq:set-cover:primal-cs) and $α$-approximate dual
complementary slackness. Then, since $X_i$ is either 0 or 1, we have

$$\begin{align*}
\sum_i w_i X_i
&= \sum_i \left(\sum_{e \in S_i} y_e\right) X_i \\
&= \sum_e \left(\sum_{i : S_i \ni e} X_i\right) y_e
\leq \alpha\sum_e y_e,
\end{align*}$$

where the first equality uses [primal complementary
slackness](#eq:set-cover:primal-cs), the second follows by changing the
order of summation, and the last uses [$α$-approximate dual
complementary slackness](#eq:set-cover:dual-cs). We then conclude by
[weak LP duality](#lp-weak-duality) that $X$ costs at most $\alpha$
times the optimal LP solution and is thus an $α$-approximation.

It now remains to how to construct $X$ and $y$.

::: {prf:algorithm label=alg-primal-dual-setcover}

- Initialize $X_i = 0$ for every $i \in [m]$ and $y = 0$
- **while** there exists an uncovered element $e$, i.e.
  $\sum_{i : S_i \ni e} X_i < 1$ **do**
  - Raise $y_e$ until $\sum_{e \in S_i} y_e = w_i$ for some $i$
  - Set $X_i = 1$

:::

Since we only set $X_i = 1$ if $\sum_{e \in S_i} y_e = w_i$, [primal
complementary slackness](#eq:set-cover:primal-cs) is satisfied. We also
have that $\sum_{i : S_i \ni e} X_i \leq f$ since each element is in at
most $f$ sets, by definition of the frequency $f$. Therefore, we get
[$f$-approximate dual complementary slackness](#eq:set-cover:dual-cs).
Thus, $X$ is an $f$-approximation.

(sec-pd-framework)=

## General Primal-Dual Framework for Covering Problems

We say that a minimization problem is a *covering problem* if it is of
the form

$$\begin{align*}
\min_{S \subseteq E} \quad & \sum_{e \in S} c_e\\
\text{subject to} \quad & S \in \mathcal{F}
\end{align*}$$

where $\mathcal{F}$ satisfies the property that $S \in \mathcal{F}$
implies that every set $S' \supseteq S$ is also in $\mathcal{F}$. In
other words, if $S \in \mathcal{F}$, then it remains feasible if we add
more elements to it.

Examples of covering problems include: vertex cover, set cover, minimum
spanning subgraph, min-weight independent set containing a basis in
matroid, and any problem we can formulate as set cover (possibly with
exponentially many elements).

The LP for a covering problem has the following form

$$\begin{align*}
\text{minimize} \quad & \sum_{1 \leq i \leq n} c_i x_i\\
\text{subject to} \quad & \sum_{1 \leq i \leq n} b_{ij} x_i \geq d_j && \forall 1 \leq i \leq m\\
& x \geq 0
\end{align*}$$

where $c_i,b_{ij},d_j$ are all nonnegative. In general, such an LP is
called a *covering LP*.

The dual LP is

$$\begin{align*}
\text{maximize} \quad & \sum_{1 \leq j \leq m} d_j y_j\\
\text{subject to} \quad & \sum_{1 \leq j \leq m} b_{ij} y_j \leq c_i && \forall 1 \leq i \leq n\\
& y \geq 0
\end{align*}$$

In general such an LP where $c_i,b_{ij},d_j$ are all nonnegative is
called a *packing LP* and is the LP for a packing problem.

We say that a maximization problem is a *packing problem* if it is of
the form

$$\begin{align*}
\max_{S \subseteq E} \quad & \sum_{e \in S} w_e\\
\text{subject to} \quad & S \in \mathcal{F}
\end{align*}$$

where $\mathcal{F}$ is an [independence system](#def-indep), i.e. every
subset of a feasible set is also feasible.

Examples of packing problems include: knapsack and matching.

The basic primal-dual framework for covering problems is as follows. We
start with an infeasible integral primal solution $X = 0$ and a feasible
dual solution $y = 0$. Then, we use violated primal constraints to
determine dual variables to raise. Once a dual constraint becomes tight,
we set the corresponding primal variable to $1$.

::: {prf:algorithm label=alg-primal-dual}

- Initialize $X_i = 0$ for every $i \in [m]$ and $y = 0$
- **while** there is a violated primal constraint, i.e.
  $\sum_{1\leq i \leq n} b_{ij} < d_j$ **do**
  - Raise dual variable $y_j$ corresponding to violated primal
    constraint until some dual constraint becomes tight, i.e.
    $\sum_{e \in S_i} y_e = w_i$ for some $i$
  - Set corresponding primal variable $X_i = 1$

:::

The algorithm ensures primal complementary slackness. The main challenge
is in choosing which dual variables to raise to ensure approximate dual
complementary slackness. In the sequel, we will see primal-dual
algorithms for shortest paths and minimum spanning trees. In particular,
we will see how Dijkstra's algorithm for the former and Kruskal's
algorithm for the latter can be derived in a unified way from the
primal-dual framework.

[^1]: The most number of sets that any element belongs to.
