(sec-bipartite)=

# Bipartite Matching

In this section, we will present the algorithm for [maximum
matching](#prob-matching) in bipartite graphs. At the end, we will see
how to use it to compute a [minimum vertex cover](#prob-vc) as well.

Let $G = (V,E)$ be a [bipartite graph](#def-bipartite) where $L$ and $R$
are the left and right parts, respectively. At a high level, our
algorithm starts with the empty matching $M = \emptyset$ and iteratively
"augments" $M$ by removing some edges and adding one more edge than was
removed. Note that it differs from greedy in the sense that greedy would
never remove an edge that it has previously added.

The algorithm adds and remove edges along "augmenting paths", which we
now define.

::: {prf:definition label=def-augmenting} Augmenting paths

Let $M$ be a matching in a bipartite graph $G$. A vertex $v$ is
$M$-*free* if it is not incident to any edge in $M$. A path in $G$ is an
$M$-*alternating path* if the path alternates between an edge in $M$ and
an edge not in $M$. Moreover, an $M$-alternating path is an
$M$-*augmenting path* if it starts and ends at free vertices.

:::

To avoid clutter, we simply write *free*, *alternating path*, and
*augmenting path*, when there is no ambiguity.

## Augmenting Paths Gonna Augment

Given two subgraphs $F$ and $H$, their *symmetric difference* is the
subgraph $F \oplus H$ obtained by replacing the edges in $F$ that are in
common with $H$ with the edges of $H$ not in $F$, i.e.
$$F \oplus H = (F \setminus H) \cup (H \setminus F).$$

::::: {aside}

:::: {figure}

(fig-before-xor)=

::: {image width=50%} ./w6-before-xor.png

:::

(fig-after-xor)=

::: {image width=50%} ./w6-after-xor.png

:::

@fig-before-xor shows a matching (edges in blue) and @fig-after-xor
shows the symmetric difference (edges in red) between the matching and
the path consisting of edges $e_1, e_3, e_2$.

::::

:::::

::: {prf:observation label=obs-augmenting}

Let $M$ be a matching, $P$ be an augmenting path, and $M' = M \oplus M$.
Then $M'$ is a matching of size $|M'| = |M|+1$.

:::

::: {prf:proof enumerated=false}

Recall that $P \setminus M$ is the set of edges that are being added,
and $M \setminus P$ are those being removed. Since $P$ is an augmenting
path, we get the following: for every edge $(u,v)$ of $P \setminus M$,
either $u$ is free or there is an edge incident to $u$ in
$M \setminus P$, and the same holds for $v$. Thus, $M'$ is indeed a
matching.

Since $P$ is an augmenting path, the first and last edges of $P$ is not
in $M$. Moreover, $P$ is alternating and so
$|P \setminus M| = |M \setminus P| + 1$. Thus, we get that
$|M'| = |M| + 1$.

:::

Armed with this observation, we can now specify our algorithm.

::: {prf:algorithm label=alg-bipartite} Bipartite Matching

- Initialize $M \leftarrow \emptyset$
- ****while**** there is augmenting path $P$ ****do****
  - $M \leftarrow M \oplus P$
- ****end while****
- ****return**** $M$

:::

There are two tasks left. First, we need to show that the algorithm is
optimal. Later we show how to find an augmenting path, if one exists.

## Optimality Conditions

::: {prf:theorem label=thm-bip-matching} Characterization of Optimality

Let $G$ be bipartite graph. Then, $M$ is a maximum matching if and only
if there is no $M$-augmenting path.

:::

::: {prf:proof enumerated=false}

@obs-augmenting implies that if there is an augmenting path, then $M$ is
not a maximum matching. In the rest of the proof, we assume that $M$ is
not a maximum matching and show that there is an augmenting path.

Let $M^*$ be a maximum matching, and $H = M^* \oplus M$. Since every
vertex has degree at most $1$ in $M^*$ and $M$, each vertex has degree
at most $2$ in $H$. Thus, $H$ consists of alternating paths and cycles
that are vertex-disjoint, i.e. they do not have any vertex in common.
Observe that a bipartite graph cannot have odd-length cycles. Thus, $H$
can be decomposed into 3 sets (possibly empty): even-length alternating
cycles, even-length alternating paths, and odd-length alternating paths.

Since $|M^*| > |M|$, we also have $|M^* \cap H| > |M \cap H|$, i.e.
there are more edges in $H$ that belong to $M^*$ than to $M$. Observe
that for every even-length alternating path $P$, we have
$|M^* \cap P| = |M \cap P|$. Ditto for cycles. Thus, there must be an
odd-length alternating path $P$ such that $|M^* \cap P| > |M \cap P|$.
Since $P$ is alternating, it starts and ends with edges of
$M^* \setminus M$, and thus it starts and ends at free vertices.
Therefore, $P$ is an augmenting path. We conclude that when $M$ is not a
maximum matching, then there exists an augmenting path, completing the
proof.

:::

## Finding Augmenting Paths

Finally, we show how to find an augmenting path, if one exists. The key
idea is to do a BFS that explores along alternating paths. To make this
more precise, we use the following notion of a residual network.

::: {prf:definition label=def-residual} Residual Network

Let $G$ be a bipartite graph and $M$ be a matching. The *residual
network* $G_M$ is a directed graph obtained from $G$ as follows:

- for each edge $e \in M$, direct it right to left
- for each edge $e \notin M$, direct it left to left
- add a new vertex $s$ and a directed edge from $s$ to each free vertex
  in $L$
- add a new vertex $t$ and a directed edge to $t$ from each free vertex
  in $R$

:::

::::: {aside}

:::: {figure}

(fig-before-xor-2)=

::: {image width=40%} ./w6-before-xor.png

:::

(fig-residual)=

::: {image width=80%} ./w6-residual.png

:::

@fig-before-xor-2 shows a matching (edges in blue) and @fig-residual
shows its residual network.

::::

:::::

::: {prf:lemma label=lem-augmenting}

Given a bipartite graph $G$ and a matching $M$, there exists an
augmenting path if and only if there exists an $s$-$t$ path in $G_M$.

:::

:::::: {prf:proof enumerated=false}

Let $u \in L$ and $v \in R$ be free vertices. Since edges not in $M$ are
directed left to right and edges in $M$ are directed right to left,
there is a one-to-one correspondence between alternating paths and
directed paths in $G_M$. Thus, if there is an augmenting path $P$
between a free vertex $u \in L$ and a free vertex $v \in R$, then we
obtain an $s$-$t$ path in $G_M$ by going from $s$ to $u$, following $P$,
and then from $v$ to $t$. On the other hand, if we have an $s$-$t$ path
$Q$, then dropping $s$ and $t$ from the path gives an augmenting path.

::::::

Therefore, we can efficiently determine if an augmenting path exists,
and to find one if it does, by running BFS to find an $s$-$t$ path in
$G_M$.

## Vertex Cover

We now show how to find a minimum vertex cover in bipartite graphs.

::: {prf:theorem label=thm-bip-vc}

Let $G$ be a bipartite graph and $M^*$ be a maximum matching. Let $L^*$
and $R^*$ be the left and right endpoints of $M^*$, respectively. Let
$Q$ be the set of vertices that are reachable via an alternating path
from a vertex in $L$. Then,
$$C^* = (L^* \setminus Q) \cup (R^* \cap Q)$$ is a minimum vertex cover.

:::

:::::: {aside}

::: {image width=50%} ./w6-vertex-cover.png

:::

In this example, $M^*$ is the set of blue edges, $L^* = \{2,3\}$,
$R^* = \{5,6\}$, the free vertices are $\{1,4\}$, $Q = \{1,5,2\}$, $C^*$
is the red vertices $\{5,3\}$.

::::::

:::::: {prf:proof enumerated=false}

Let $P$ be an alternating path starting from a free vertex in $L$. Since
its first edge is not in $M^*$, if it ends in a vertex in $R$, then its
last edge is not in $M^*$; if it ends in a vertex in $L$, then its last
edge is in $M^*$.

:::: {figure label=fig-vertex-cover}

::: {image width=70%} ./w6-vertex-cover-2.jpeg

:::

Illustration of the cases below. The edges in red are not in $M^*$ and
the ones in blue are in $M^*$.

::::

We first show that $C^*$ is a vertex cover. Let $(u,v)$ be an edge where
$u \in L$ and $v \in R$. The following are all the possible cases:

1.  ($u \in L \setminus L^*$ and $v \in R \setminus R^*$). This case
    cannot happen since such an edge can be added to $M^*$ and $M^*$ is
    maximum. In the remaining cases, we will show that either
    $u \in C^*$, or $v \in C^*$.
2.  ($u \in L \setminus L^*$ and $v \in R^*$). In this case, $v \in Q$
    since it can be reached from the free vertex $u$ in $L$ via the edge
    $(u,v)$. Thus, $v \in C^*$.
3.  ($u \in L^*$ and $v \in R \setminus R^*$). In this case, an
    alternating path from a free vertex in $L$ to $u$ can be extended
    into an augmenting path using the edge $(u,v)$. Since $M^*$ is
    maximum, @thm-bip-matching implies that $u \notin Q$ and so
    $u \in C^*$.
4.  ($u \in L^*$ and $v \in R^*$, $(u,v) \notin M^*$). In this case, if
    $u \in C^*$, then we are done. Otherwise, $u \in Q$ and so $v \in Q$
    since we can extend the path to $u$ with the edge $(u,v)$; thus,
    $v \in C^*$.
5.  ($(u,v) \in M^*$). In this case, if $v \in C^*$, then we are done.
    Otherwise, $v \notin Q$. Since $(u,v) \in M^*$, all the other edges
    incident to $u$ are not in $M^*$. Thus, the only way to reach $u$ is
    by first reaching $v$ and then following the $(u,v)$ edge.
    Therefore, $v \notin Q$ implies that $u \notin Q$ and so
    $u \in C^*$.

Now that we have shown $C^*$ is a vertex cover, it remains to show
$|C^*| = |M^*|$. We will do this by showing that exactly one endpoint of
each edge of $M^*$ is in $C^*$. Suppose, towards a contradiction, that
$C^*$ contains both endpoints of an edge $(u,v) \in M^*$. If
$v \in C^*$, then $v \in Q$ and so we can extend the path to $v$ by the
edge $(u,v)$ and reach $u$ as well. Thus, $u \in Q$ and so
$u \notin C^*$. This gives the desired contradiction and so we get that
$C^*$ contains exactly one endpoint of each edge $(u,v) \in M^*$.

::::::
