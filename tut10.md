(sec-tut10)=

# Tutorial 10

In this tutorial, we will explore the power of the primal-dual method.
In the first exercise, we will reinterpret the greedy algorithm for
Interval Hitting Set in @sec-tut5 as a primal-dual algorithm. Then, we
will extend it to handle Weighted Interval Hitting Set. Finally, we
explore further properties of the cut constraint LP.

The *Weighted Interval Hitting Set Problem* is as follows. We are given
as input a set of weights $w_i$ for each integer $1 \leq i \leq n$, and
a collection of intervals $I_1, \ldots, I_m$ where each interval
$I_j = [a_j, b_j]$ is such that $a_j$ and $b_j$ are integers satisfying
$1 \leq a_j \leq b_j \leq n$. An integer subset
$S \subseteq \{1, \ldots, n\}$ is a *hitting set* if
$I_j \cap S \neq \emptyset$ for every interval $I_j$. That is, every
interval contains some integer in $S$. The goal is to find a hitting set
$S$ with minimum total weight $w(S)$.

:::: {figure}

::: {image width=100%} ./tut10-interval.png

:::

Chosen hitting set $S=\{3,7\}$ with total weight 2. Every interval
contains at least one selected integer.

::::

The [Interval Hitting Set Problem](#prob-interval-hitting) is the
special case when the weights are $1$.

::: {exercise label=ex-10-1}

Give a primal-dual exact algorithm for the Interval Hitting Set Problem
using the primal and dual LPs in [Tutorial 9](#ex-9-3).

:::

::: {hint class=dropdown}

Reinterpret the greedy algorithm in [Tutorial 5 Exercise 1](#ex-5-1) and
the dual-based analysis of it in [Tutorial 5 Exercise 2](#ex-5-2) as a
primal-dual algorithm.

:::

::::: {solution class=dropdown} ex-10-1

Recall that the primal LP is

$$\begin{align}
\text{minimize} \quad & \sum_i x_i\\
\text{subject to} \quad & \sum_{i \in I_j} x_i \geq 1 \quad  && \forall j \in [m]\\
& x_i \geq 0 \quad  && \forall i \in [n]
\end{align}$$

and the dual LP is

$$\begin{align}
\text{maximize} \quad & \sum_j y_j\\
\text{subject to} \quad & \sum_{j : I_j \ni i} y_j \leq 1 \quad  && \forall i \in [n]\\
& y_j \geq 0 \quad  && \forall j \in [m]
\end{align}$$

The interpretation of the dual is that each interval $I_j$ "pays" $y_j$
to be covered. As before, we use the terminology that $i \in [n]$ is
*tight* when the dual constraint for $i$ is tight, i.e.
$\sum_{j : I_j \ni i} y_j = 1$. Since each integer $i \in [n]$ costs $1$
to be included in $S$, we think of $i$ being tight to mean that it is
"paid for" by $y$.

The primal-dual algorithm maintains a subset $S \subseteq [n]$ and a
dual solution $y$. These are initialized to be $S = \emptyset$ and
$y=0$.

While $S$ is not a hitting set, among the intervals not hit by $S$, we
raise the dual variable of the interval $I_j = [a_j,b_j]$ with the
smallest right endpoint $b_j$, until some dual constraint involving
$y_j$ is tight (equivalently, until some $i \in I_j$ is tight). Here, we
need to be careful since several integers $i \in I_j$ can become tight
simultaneously. Which of them do we add to $S$? Inspired by the greedy
algorithm, we add the rightmost one.

::: {prf:algorithm label=alg-pd-interval-hitting}

- Initialize $S = \emptyset$ and $y = 0$
- **while** $S$ not a hitting set **do**
  - Let $I_j = [a_j,b_j]$ be the interval not hit by $S$ with the
    smallest $b_j$
  - Raise $y_j$ until some $i \in I_j$ is tight
  - Add rightmost integer in $I_j$ that is tight
- **return** $S$

:::

We now show that $S$ is a minimum interval hitting set by showing that
$S$ and $y$ satisfy primal complementary slackness conditions

$$\begin{equation}
i \in S \implies \sum_{j : I_j \ni i} y_j = 1
\end{equation}$$

and dual complementary slackness conditions

$$\begin{equation}
y_j > 0 \implies |I_j \cap S| = 1
\end{equation}$$

The primal complementary slackness conditions are satisfied since each
$i \in S$ was added to $S$ when it is tight, and tight dual constraints
remain tight since we never decrease dual variables.

It remains to prove the dual complementary slackness conditions are also
satisfied and the dual $y$ is feasible. To this end, we first show that
in fact, in each iteration of the **while** loop, the algorithm adds
$b_j$, the right endpoint of the interval $I_j$ whose dual variable is
raised in the iteration. We show this by induction on the iterations. In
the first iteration, since the RHS of the dual constraints is $1$, we
get that $y_j$ is raised until $y_j=1$ and so every $i \in I_j$ is
tight, which in turn implies that $b_j$ is the rightmost tight integer
of $I_j$. In each subsequent iteration, the inductive hypothesis implies
that $I_j$ is disjoint from the other intervals $I_{j'}$ with
$y_{j'} > 0$. Thus, we again have that $y_j$ is raised until $y_j = 1$
and the algorithm adds $b_j$ to $S$. This completes the proof by
induction.

Thus, we get that and so for each interval $I_j$ with $y_j > 0$, exactly
one integer of $I_j$ is added to $S$, namely its right endpoint $b_j$.
(See figure below for an illustration of the argument.) This completes
the proof that dual complementary slackness conditions are satisfied.

:::: {figure}

::: {image width=100%} ./tut10-pd-unweighted.png

:::

::::

Finally, the dual solution $y$ is also feasible since in each iteration,
we stop raising $y_j$ once a dual constraint involving it is tight, and
we never raise a dual variable $y_j$ whose interval overlaps with an
interval with positive dual variable.

::: {note}

The proof of dual complementary slackness shows that the primal-dual
algorithm is equivalent to the greedy algorithm which is simpler to
state and analyze. The benefit of the primal-dual algorithm is that it
generalizes to the weighted setting where each integer $i \in [n]$ costs
$w_i$ and do not need to be the same for every $i$.

:::

:::::

::: {exercise label=ex-10-2}

Write the primal and dual LPs for the Weighted Interval Hitting Set
Problem.

:::

::: {solution class=dropdown} ex-10-2

The primal LP is

$$\begin{align}
\text{minimize} \quad & \sum_i w_i x_i\\
\text{subject to} \quad & \sum_{i \in I_j} x_i \geq 1 \quad  && \forall j \in [m]\\
& x_i \geq 0 \quad  && \forall i \in [n]
\end{align}$$

and the dual LP is

$$\begin{align}
\text{maximize} \quad & \sum_j y_j\\
\text{subject to} \quad & \sum_{j : I_j \ni i} y_j \leq w_i \quad  && \forall i \in [n]\\
& y_j \geq 0 \quad  && \forall j \in [m]
\end{align}$$

The derivation of the dual follows the same reasoning as in the
derivation for the unweighted version in [Tutorial 9 Exercise
6](#ex-9-6). The only difference in the dual is that the RHS of the dual
constraint is $w_i$ since that is the coefficient of the primal variable
$x_i$ in the primal objective.

:::

::: {exercise label=ex-10-3}

Give a primal-dual 2 approximation algorithm for the Weighted Interval
Hitting Set Problem.

:::

::: {hint class=dropdown}

Just as for the shortest path problem, we cannot take the hitting set to
be all integers $i$ that correspond to tight dual constraints, and we
will need to be careful which ones we take.

:::

::::: {solution class=dropdown} ex-10-3

For this problem, the above algorithm @alg-pd-interval-hitting does not
always satisfy dual complementary slackness. To get around this, we add
a post-processing step to the above algorithm that prunes $S$ to get a
final solution. In particular, for each $i \in S$, if we still have a
hitting set after removing $i$ from $S$, then we do so. Equivalently,
the final solution $S'$ is a minimally feasible subset of $S$: removing
any integer from $S'$ results in an infeasible solution. This is similar
to how the [shortest-path primal-dual algorithm](#alg-pd-shortest-path)
adds tight edges to $T$ and then prunes it to get a $(s,t)$-path.

::: {prf:algorithm label=alg-pd-weighted-interval-hitting}

- Initialize $S = \emptyset$ and $y = 0$
- **while** $S$ not a hitting set **do**
  - Let $I_j = [a_j,b_j]$ be the interval not hit by $S$ with the
    smallest $b_j$
  - Raise $y_j$ until some $i \in I_j$ is tight
  - Add rightmost integer in $I_j$ that is tight
- $S' = S$
- **foreach** $i \in S'$ **do**
  - **if** $S' \setminus \{i\}$ is a hitting set **then**
    - Remove $i$ from $S'$
- **return** $S'$

:::

Primal complementary slackness and dual feasibility follow from the same
arguments as in @ex-10-1.

We now show that 2-approximate dual complementary slackness is
satisfied:

$$\begin{equation}
y_j > 0 \implies |I_j \cap S'| \leq 2.
\end{equation}$$

Suppose towards a contradiction that there exists $y_j > 0$ with
$|I_j \cap S'| > 2$ and let $I_j = [a_j, b_j]$. At the start of the
iteration in which we were raising $y_j$, none of $S’ \cap I_j$ has been
added to $S$ yet (otherwise, $I_j$ was already satisfied, a
contradiction). Thus, every integer in $S’ \cap I_j$ was added to $S$
after the start of the iteration.

Since $|S' \cap I_j| > 2$, there is an integer $k \in S' \cap I_j$ that
is neither the leftmost nor rightmost integer of $S' \cap I_j$. Let
$I_k = [a_k, b_k]$ be the interval such that $k$ is the only integer in
$S’$ that is in $I_k$; in other words, the reason that we cannot remove
$k$ from $S'$ is because no other integers of $S'$ is in $I_k$.

We now argue that $I_k$ is not strictly contained in $I_j$. Since the
algorithm raises the dual variable of the unsatisfied interval with
rightmost $b_j$, if $I_k$ is strictly contained in $I_j$, then
$b_k < b_j$ and so $I_k$ would have been satisfied in an earlier
iteration. However, if $I_k$ is strictly contained in $I_j$, that would
mean that $I_j$ was satisfied before the start of the iteration, a
contradiction.

Since $I_k$ is not strictly contained in $I_j$, we have that either
$a_j \in I_k$ or $b_j \in I_k$. Therefore, $I_k$ contains either the
leftmost or rightmost integer of $S' \cap I_j$. Since $k$ is neither one
of these, we get that $k$ could have been removed from $S'$ and $I_k$
would still have been satisfied, a contradiction. See below figure for
an illustration of the argument.

:::: {figure}

::: {image width=100%} ./tut10-pd-weighted.png

:::

If $|S' \cap I_j|>2$, choose an interior point $k \in S' \cap I_j$.
Since $I_k$ is not strictly contained in $I_j$, it must contain either
$a_j$ or $b_j$, hence also the leftmost or rightmost point of
$S'\cap I_j$. Therefore removing $k$ would still satisfy $I_k$,
contradicting minimality of $S'$.

::::

We conclude that the 2-approximate dual complementary slackness
conditions are satisfied.

:::::

::: {exercise label=ex-10-4}

Recall the cut constraint LP for MST.

$$\begin{align*}
\text{minimize} \quad & \sum_{e \in E} c_e x_e\\
\text{subject to} \quad & \sum_{S : e \in \delta(S)} x_e \geq 1 && \forall \emptyset \subsetneq S \subsetneq V\\
& x \geq 0
\end{align*}$$

Show that the cut constraint LP has an integrality gap of at least
$2(1-1/n)$ for MST.

:::

::: {hint class=dropdown}

First show that it has integrality gap of $3/2$ for $n=3$.

:::

::::: {solution class=dropdown} ex-10-4

Consider the cycle graph $G$ on 3 vertices: $V = \{1, 2, 3\}$ and
$E = \{(1,2),(2,3),(1,3)\}$. Every edge has cost $c_e = 1$. The MST
takes two edges for a cost of 2. On the other hand, there is a feasible
LP solution with cost at most $3/2$: $x_e = 1/2$ for every edge
$e \in E$. See figure below for an illustration.

:::: {figure}

::: {image width=40% label=fig1} ./tut10-int-gap-graph.png

:::

::: {image width=100% label=fig2} ./tut10-int-gap.png

:::

@fig1 illustrates the graph $G$ and edge costs. The left figure of @fig2
illustrates the LP solution and the right figure illustrates that the
cut constraint for $S = \{2\}$ is satisfied.

::::

To generalize this to $n$ vertices, consider the cycle graph on $n$
vertices. Every edge has cost $c_e=1$. The MST takes $n-1$ edges for a
cost of $n-1$. On the other hand, there is a feasible LP solution with
cost at most $n/2$ which sets $x_e = 1/2$ for every edge $e \in E$.

:::::

::: {exercise label=ex-10-5}

Give an efficient separation oracle for the cut constraint LP.

:::

::: {hint class=dropdown}

Use the fact that [Min $(s,t)$-Cut](#prob-st-cut) has a polynomial-time
exact algorithm.

:::

::::: {solution class=dropdown} ex-10-5

A solution $x$ is infeasible if and only if there exists a vertex subset
$\emptyset \subsetneq S^* \subsetneq V$ such that
$\sum_{S^* : e \in \delta(S^*)} x_e < 1$. Since
$\emptyset \subsetneq S^* \subsetneq V$, there exist vertices $s \in S$
and $t \notin S$. Consider the Min $(s,t)$-Cut instance with graph $G$
and edge costs $x_e$. Then, $\sum_{S^* : e \in \delta(S^*)} x_e$ is
exactly the cost of the $(s,t)$-cut $S^*$ and so the cost of the optimal
$(s,t)$-cut is less than 1. See below figure for an illustration.

:::: {figure}

::: {image width=75%} ./tut10-sep-oracle.png

:::

::::

Thus, $x$ is infeasible if and only if there exist vertices $s,t$ such
that the cost of the optimal solution to the Min $(s,t)$-Cut instance
with graph $G$ and edge costs $x_e$ is less than $1$. We can thus
determine if such a vertex pair exists by solving the Min $(s,t)$-Cut
instance for every pair $s,t \in V$. Since there are $O(n^2)$ pairs and
Min $(s,t)$-Cut can be solved in polynomial time, we get an efficient
separation oracle.

:::::
