(sec-tut6)=

# Tutorial 6

Bipartite matchings are fundamental in combinatorics and combinatorial
optimization. Hall's Theorem provides a characterization of when there
exists a *perfect matching*: a matching in which all vertices are
matched, and there are no free vertices left.

For a set of vertices $X$ in a graph $G = (V,E)$, define the
*neighborhood* of $X$ to be the set of vertices $v$ that share an edge
$(u,v) \in E$ with a vertex $u \in X$, and denote it by $N(X)$.

::::: {aside}

::: {image width=50%} ./w6-before-xor.png

:::

In this graph, the neighborhood of $\{2,3\}$ is $\{4,5,6\}$, and the
neighborhood of $\{2\}$ is $\{4,5\}$.

::::

:::::

::: {prf:theorem label=thm-halls} Hall's Theorem

Let $G = (V,E)$ be a bipartite graph with left part $L$ and right part
$R$ such that $|L| = |R| = n$. There exists a perfect matching if and
only if for every subset $X \subseteq L$, we have $|N(X)| \geq |X|$.

:::

::: {exercise label=ex-halls-thm} Hall's Theorem

Use [strong duality of bipartite matchings and vertex
covers](#thm-bip-vc) (i.e. that in bipartite graphs the size of the
maximum matching is equal to the size of the minimum vertex cover) to
prove Hall's Theorem.

:::

::: {solution class=dropdown} ex-halls-thm

If there is a perfect matching $M$, then for every set $X \subseteq L$,
the neighborhood of $X$ includes the $|X|$ vertices that $M$ matches $X$
to. Thus, $|N(X)| \geq |X|$.

Suppose there is no perfect matching. We now show that there is a subset
$X \subseteq L$ such that $|N(X)| \leq |X|$. Let $M^*$ and $C^*$ be the
maximum matching and minimum vertex cover, respectively. Let
$R^* = C^* \cap R$. We partition $L$ into $X = L \setminus C^*$ and
$L^* = |L \cap C^*|$. Note that $|X| + |L^*| = |L| = n$. Since there is
no perfect matching, we have $|C^*| = |M^*| < n$. Together with the fact
that $|C^*| = |R^*| + |L^*|$, we get that $$|R^*| < n - |L^*| = |X|.$$
Since $C^*$ is a vertex cover, every neighbor of $X$ must be in $R^*$
and so $|N(X)| \leq |R^*|$. Thus, we get $$|N(X)| \leq |R^*| < |X|.$$

:::

::: {exercise label=ex-6-2}

A natural question is whether there is a better rounding algorithm for
the vertex cover LP. In other words, given an optimal solution $x^*$,
can we find a vertex cover $|C|$ such that
$|C| \leq \alpha \sum_v x^*_v$ for $\alpha < 2$? The best smallest
possible $\alpha$ is called the *integrality gap* of the LP.

Show that there exists a non-bipartite graph $G$ where the integrality
gap is at least $4/3$. It suffices to show that there is a feasible
solution $x$ to the vertex cover LP for $G$ such that every vertex cover
$C$ has size $|C| \geq 2 \sum_v x_v$.

:::

::: {solution class=dropdown label=sol-6-2} ex-6-2

Consider the complete graph $G$ on 3 vertices: $V = \{1, 2, 3\}$ and
$E = \{(1,2),(2,3),(1,3)\}$. Then, $x_1 = x_2 = x_3 = 1/2$ is a feasible
solution to the vertex cover LP with total sum $3/2$. On the other hand,
a vertex cover must choose at least 2 of the 3 vertices. Thus, the
integrality gap is at least $4/3$.

:::

::: {exercise label=ex-6-3}

Write the linear programs for [Interval Hitting
Set](#prob-interval-hitting) and [Disjoint
Intervals](#prob-interval-disjoint).

:::

::: {solution class=dropdown} ex-6-3

For Interval Hitting Set, we have a variable $x_i$ for each $i \in [n]$
to indicate whether or not we choose $i$ to be in the hitting set. The
LP is as follows.

$$\begin{align}
\text{minimize} \quad & \sum_i x_i\\
\text{subject to} \quad & \sum_{i \in I_j} x_i \geq 1 \quad  && \forall j \in [m]\\
& x_i \geq 0 \quad  && \forall i \in [n]
\end{align}$$

For Disjoint Intervals, we have a variable $y_j$ for each $j \in [m]$ to
indicate whether or not we choose interval $I_j$. The LP is as follows.

$$\begin{align}
\text{maximize} \quad & \sum_j y_j\\
\text{subject to} \quad & \sum_{j : I_j \ni i} y_j \leq 1 \quad  && \forall i \in [n]\\
& y_j \leq 1 \quad  && \forall j \in [m]
\end{align}$$

:::
