(sec-tut11)=

# Tutorial 11

::: {exercise label=ex-11-1}

The Online Vertex Cover problem is as follows: initially, we are given a
set of $n$ vertices with no edges and a vertex cover $C = \emptyset$.
Then, in each timestep, an edge $e_j = (u_j,v_j)$ arrives. If neither of
$u_j$ or $v_j$ is in $C$, then we have to choose one of them to add to
$C$, i.e. we have to maintain that $C$ is a vertex cover for the set of
edges that have arrived so far. The goal is to minimize $|C|$.

Design a deterministic 2 competitive online algorithm.

:::

::: {exercise label=ex-11-2}

Show that every deterministic algorithm has competitive ratio at least 2
for Online Vertex Cover.

:::

::: {exercise label=ex-11-3}

Write a linear program for the Tram Waiting Problem, derive its dual,
and give an interpretation to the dual variables.

:::
