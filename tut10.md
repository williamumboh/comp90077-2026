(sec-tut10)=

# Tutorial 10

In this tutorial, we will explore the power of the primal-dual method.
In the first two exercises, we will reinterpret the greedy algorithm for
Interval Hitting Set in @sec-tut5 as a primal-dual algorithm. In the
remaining exercises, we will see that the primal-dual method generalizes
Dijkstra's and Kruskal's in the sense that it is able to handle the
Steiner Tree problem, which generalizes shortest-path and minimum
spanning tree.

::: {exercise label=ex-10-1}

Give a primal-dual exact algorithm for the [Interval Hitting Set
problem](#prob-interval-hitting) using the primal and dual LPs in
[Tutorial 9](#ex-9-3).

:::

::: {hint class=dropdown}

Reinterpret the greedy algorithm in [Tutorial 5 Exercise 1](#ex-5-1) and
the dual-based analysis of it in [Tutorial 5 Exercise 2](#ex-5-2) as a
primal-dual algorithm.

:::

::: {exercise label=ex-10-2}

Recall the cut constraint LP for MST.

$$\begin{align*}
\text{minimize} \quad & \sum_{e \in E} c_e x_e\\
\text{subject to} \quad & \sum_{S : e \in \delta(S)} x_e \geq 1 && \forall \emptyset \subsetneq S \subsetneq V\\
& x \geq 0
\end{align*}$$

Show that the cut constraint LP has an integrality gap of $2$ for MST.

:::

::: {exercise label=ex-10-3}

The *Steiner Tree problem* is a generalization of the MST. In the
Steiner tree problem, we are given a graph $G=(V,E)$ with edge costs
$c_e$ and a set of terminals $S \subseteq V$. The goal is to find a
min-cost subgraph $F$ that connects the terminals, i.e. for every pair
of terminals $t,t' \in S$, there is a $(t,t')$-path in $F$.

The Steiner tree problem generalizes the shortest-path and the minimum
spanning tree problems as follows. When $X = (s,t)$, then the problem is
exactly the shortest $(s,t)$-path problem, and when $X=V$, it is exactly
the minimum spanning tree problem.

Write a linear program using cut constraints.

:::

::: {exercise label=ex-10-4}

Design a 2 approximation algorithm for the Steiner Tree problem using
the primal-dual method.

:::
