(sec-tut11)=

# Tutorial 11

::: {exercise label=ex-11-1}

The Online Vertex Cover problem is as follows: initially, we are given a
set of $n$ vertices with no edges and a vertex cover $C = \emptyset$.
Then, in each timestep, an edge $e_j = (u_j,v_j)$ arrives. If neither of
$u_j$ or $v_j$ is in $C$, then we have to choose at least one of them to
add to $C$, i.e. we have to maintain that $C$ is a vertex cover for the
set of edges that have arrived so far. The goal is to minimize $|C|$.

Design a deterministic 2 competitive online algorithm.

:::

::: {solution class=dropdown} ex-11-1

The algorithm and analysis follows along the lines for the [greedy 2
approximation for offline vertex cover](#sec-greedy-matching-vc).

When an edge $e_j = (u_j,v_j)$ arrives and neither of $u_j$ or $v_j$ is
in $C$, add both to $C$.

This is a 2 approximation. Let $M$ be the set of edges $e_j = (u_j,v_j)$
for which the algorithm added both endpoints to $C$. We have that
$|C| = 2|M|$. Moreover, $M$ is a matching and thus by [weak duality of
matchings and vertex covers](#thm-matching-vc-duality), we get that
$|M| \leq 2|C^*|$ where $C^*$ is a minimum vertex cover. We conclude
that $|C| \leq 2|C^*|$ and therefore the algorithm is 2 competitive.

:::

::: {exercise label=ex-11-2}

Show that every deterministic algorithm has competitive ratio at least 2
for Online Vertex Cover.

:::

::: {solution class=dropdown} ex-11-2

Consider a graph with 4 vertices $u,u,v,v'$. The first edge to arrive is
$(u,v)$. If the algorithm takes both endpoints, then no further edges
arrive, and the optimal solution takes only one endpoint. If the
algorithm only takes $u$, then the next and final edge that arrives is
$(v,v')$. The algorithm is forced to take one of them and has a vertex
cover of size 2 while the optimal solution takes only $v$. The case
where the algorithm only takes $v$ is similar: the next and final edge
is $(u,u')$.

:::

::: {exercise label=ex-11-3}

Write a linear program for the Tram Waiting Problem, derive its dual,
and give an interpretation to the dual variables.

:::

::: {solution class=dropdown} ex-11-3

We write an LP for offline Tram Waiting Problem, where the tram arrival
time $A$ is known. There is more than 1 LP for this problem. Here, we
first formulate the problem as a set cover problem for which we know how
to write an LP for.

There are $A-1$ elements, one for each timestep $1 \leq t \leq A-1$.
There are $A-1$ sets of cost 1, one per timestep $t$ that contain only
element corresponding to $t$. There is also a set of cost $W$ that
corresponds to walking.

Thus, we have a variable $x_W$ that indicates whether we walk, and a
variable $x_t$ for each timestep corresponding to whether we wait at
timestep $t$. The objective function is $Wx_W + \sum_t x_t$. Then for
each timestep $t$, we have the constraint $x_t + x_W \geq 1$. This gives
the following LP

$$\begin{align}
\text{minimize} \quad & Wx_W + \sum_{1 \leq t \leq A} x_t\\
\text{subject to} \quad & x_t + x_W \geq 1 \quad  && \forall 1 \leq t \leq A-1\\
& x \geq 0 \quad  &&
\end{align}$$

To derive the dual LP, for each primal constraint $1 \leq t \leq A-1$,
we have a dual variable $y_t$ with coefficient 1 in the dual objective;
for each primal variable $x_t$, we have a dual constraint $y_t \leq 1$;
and for primal variable $x_W$, we have a dual constraint
$\sum_{1\leq t \leq A-1} y_t \leq W$.

$$\begin{align}
\text{maximize} \quad & \sum_{1 \leq t \leq A-1} y_t\\
\text{subject to} \quad & y_t \leq 1 \quad  && \forall 1 \leq t \leq A-1\\
& \sum_{1 \leq t \leq A-1} y_t \leq W \quad  && \\
& y \geq 0 \quad  &&
\end{align}$$

:::
