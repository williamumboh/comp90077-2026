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

::: {exercise label=ex-10-2}

Write the primal and dual LPs for the Weighted Interval Hitting Set
Problem.

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

::: {exercise label=ex-10-4}

Recall the cut constraint LP for MST.

$$\begin{align*}
\text{minimize} \quad & \sum_{e \in E} c_e x_e\\
\text{subject to} \quad & \sum_{S : e \in \delta(S)} x_e \geq 1 && \forall \emptyset \subsetneq S \subsetneq V\\
& x \geq 0
\end{align*}$$

Show that the cut constraint LP has an integrality gap of $2(1-1/n)$ for
MST.

:::

::: {hint class=dropdown}

First show that it has integrality gap of $3/2$ for $n=3$.

:::

::: {exercise label=ex-10-5}

Give an efficient separation oracle for the cut constraint LP.

:::

::: {hint class=dropdown}

Use the fact that [Min $(s,t)$-Cut](#prob-st-cut) has a polynomial-time
exact algorithm.

:::
