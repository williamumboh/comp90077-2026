(sec-pd-mst)=

# Primal-Dual for Minimum Spanning Trees

We now show how to derive Kruskal's algorithm for Minimum Spanning Trees
from the [primal-dual framework](#sec-pd-framework).

Recall that in the Minimum Spanning Tree problem, we are given as input
a graph $G = (V,E)$ and non-negative edge costs $c_e$ for each edge $e$.
The goal is to find a subset $T$ of $E$ that spans all vertices with
minimum total cost.

## Primal and Dual LPs

Recall that for the shortest $(s,t)$-path problem, we used cut
constraints to ensure that $s$ and $t$ are connected. In this problem,
we want to ensure that every pair of vertices is connected, and so we
can also try to use cut constraints. This time, we are interested in
cuts $S$ such that $\emptyset \subsetneq S \subsetneq V$, i.e. there is
at least one vertex in $S$ and at least one outside.

$$\begin{align*}
\text{minimize} \quad & \sum_{e \in E} c_e x_e\\
\text{subject to} \quad & \sum_{S : e \in \delta(S)} x_e \geq 1 && \forall \emptyset \subsetneq S \subsetneq V\\
& x \geq 0
\end{align*}$$

Unfortunately, this LP has an integrality gap of 2 ([Tutorial 10
Exercise 4](#ex-10-4)).

A stronger LP is one that uses partition constraints. Let
$\Pi = (S_1, \ldots, S_k)$ be a partition of $V$, i.e. the sets $S_i$
are disjoint and cover all of $V$. Let $\delta(\Pi)$ be the set of edges
$(u,v)$ where $u \in S_i$ and $v \in S_j$ for $i \neq j$, i.e. the set
of edges that connect two different parts of $\Pi$. A spanning subgraph
$T$ must contain at least $|\Pi|-1$ edges of $\delta(\Pi)$.

Here's the LP and its dual.

$$\begin{align*}
\text{minimize} \quad & \sum_{e \in E} c_e x_e\\
\text{subject to} \quad & \sum_{\Pi : e \in \delta(\Pi)} x_e \geq |\Pi| - 1 && \forall \Pi\\
& x \geq 0
\end{align*}$$

$$\begin{align*}
\text{maximize} \quad & \sum_{\Pi} (|\Pi| - 1) y_\Pi\\
\text{subject to} \quad & \sum_{\Pi : e \in \delta(\Pi)} y_\Pi \leq c_e && \forall e \in E\\
& y \geq 0
\end{align*}$$

### Interpretation of Dual

Let $\Pi = (S_1, \ldots, S_k)$. Extending on the interpretation of cut
constraints as a [moat-packing](#sec-moat-interpretation), the dual
variable $y_\Pi$ can be thought of as placing a moat of width $y_\Pi/2$
around each part $S_i$ of $\Pi$.

To see that a feasible dual $y$ is a lower bound on the cost of the MST,
suppose that $y_\Pi > 0$ only when $\Pi = \Pi'$ for some $\Pi'$. Then,
the dual feasibility constraints imply that every edge
$e \in \delta(\Pi')$ has cost $c_e \geq y_{\Pi'}$. Since a spanning tree
$T$ has to contain at least $|\Pi'|-1$ edges from $\delta(\Pi')$, the
spanning tree $T$ has cost at least $(|\Pi'|-1)c_e$. See
@fig-partition-width for an illustration.

:::: {figure label=fig-partition-width}

::: {image width=75%} ./partition-width.png

:::

In this example, $\Pi' = (\{s,2,6,7\},\{4,5\},\{3,t\})$, $y_{\Pi'} = 16$
and
$\delta(\Pi') = \{(2,3), (6,3), (6,5),(7,5),(7,t),(5,t),(5,3),(4,3),(4,t)\}$.
Every edge in $\delta(\Pi')$ has cost at least $y_{\Pi'}$. Observe that
the MST has to choose at least $|\Pi'| - 1 = 2$ edges from
$\delta(\Pi')$ and thus has total cost at least 32.

::::

## Simultaneous Moat-Growing

The idea is that the partition $\Pi$ whose dual variable we raise is
going to be the connected components of our current set of edges $T$.
Our algorithm can be viewed as growing multiple moats simultaneously,
one for each part of $\Pi$. It starts with the partition of $V$ into
singletons (since every vertex is isolated when $T$ is empty).

We say that an edge $e$ is tight if
$\sum_{\Pi : e \in \delta(\Pi)} y_\Pi = c_e$.

::: {prf:algorithm label=alg-pd-shortest-path}

- Initialize $T = \emptyset$ and $y = 0$
- **while** $T$ not spanning **do**
  - Let $\Pi$ be connected components of $T$
  - Raise $y_\Pi$ until some edge $e = (u,v)$ tight
  - Add $e$ to $T$
- **return** $T$

:::

We now show that $T$ is indeed a MST by showing that $T$ and $y$ satisfy
primal and dual complementary slackness conditions. The primal
complementary slackness conditions are
$$e \in T \implies \sum_{\Pi : e \in \delta(\Pi)} y_\Pi = c_e$$ and the
dual complementary slackness conditions are
$$y_\Pi > 0 \implies \sum_{e \in \delta(\Pi)} x_e = |\Pi|-1.$$

::: {prf:lemma label=lem-mst-cs}

The primal-dual pair $T$ and $y$ satisfy primal and dual complementary
slackness conditions.

:::

::: {prf:proof enumerated=false}

Primal complementary slackness is satisfied since we only add tight
edges to $T$. Since the algorithm only adds an edge that connects two
connected components, the set $T$ is acyclic. Suppose, towards a
contradiction, that $y_\Pi > 0$ and $|T \cap \delta(\Pi)| > |\Pi| - 1$.
Consider the iteration in which the algorithm grew $y_\Pi$ and let $T'$
be the state of $T$ at the start of the iteration. Since $\Pi$ is the
connected components of $T'$, we have that $T'$ already consists of at
least $n - |\Pi|$ edges. Since $|T \cap \delta(\Pi)| > |\Pi|-1$, we have
that the remaining iterations added more than $|\Pi|-1$ edges to $T'$.
Thus, $|T| > n - |\Pi| +|\Pi| - 1 = n-1$. However, this contradicts the
fact that $T$ is acyclic.

:::
