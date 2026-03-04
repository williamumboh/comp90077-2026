# Minimum Spanning Trees

We begin our journey by recalling the classic Minimum Spanning Tree
problem.

::: {prf:definition} Minimum Spanning Tree

In the Minimum Spanning Tree (MST) problem, we are given as input a
graph $G = (V,E)$ and non-negative edge weights $w_e$ for each edge $e$.
The goal is to find a subset $F$ of $E$ that spans all vertices while
minimizing its total weight $\sum_{e \in F} w_e$.

:::

While our definition allows $F$ to contain a cycle, it is not too hard
to argue that there always exists an optimal solution that does not
contain cycles and is thus a spanning tree.

::: {exercise}

Prove that for every graph $G = (V,E)$ and non-negative edge weights
$w_e$, there always exists an optimal solution to the MST problem that
is acyclic.

:::

There are [4 greedy algorithms for
MST](https://en.wikipedia.org/wiki/Minimum_spanning_tree#Classic_algorithms).
We focus on Kruskal's algorithm, defined as follows.

::: {prf:algorithm} Kruskal's Algorithm

- Initialize $S \leftarrow E$, sorted in decreasing order of weight, and
  $F \leftarrow \emptyset$
- ****while**** $S \neq \emptyset$ and $F$ not spanning ****do****
  - Remove the minimum-weight edge $e$ from $S$
  - Add $e$ to $F$ if doing so does not create a cycle and discard $e$
    otherwise
- ****end while****

:::

## Proof of correctness

We now prove the correctness of Kruskal's algorithm.

::: {prf:theorem label=thm-kruskal}

Kruskal's algorithm finds the optimal solution to the MST problem.

:::

We will need the following Greedy Swap property.

::: {prf:lemma label=lem-greedy-swap} Greedy Swap

Let $T$ be a spanning tree and $e$ be an edge not in $T$. Then
$T \cup \{e\}$ has exactly one cycle $C$ and for every other edge
$e' \in C$, we have that $T \cup \{e\} \setminus \{e'\}$ is a spanning
tree.

:::

::: {prf:proof enumerated=false}

First observe that if a graph has two or more cycles, then we need to
remove at least two edges to get rid of the cycles. To see why, consider
the two cases: when the two cycles $C_1$ and $C_2$ share an edge and
when they do not. The latter is easy. For the former, suppose they share
the edge $f = (u,v)$. Then, removing $(u,v)$ does not get rid of the two
cycles because we can still walk from $u$ to $v$ along
$C_1 \setminus \{f\}$, and then $v$ back to $u$ along
$C_2 \setminus \{f\}$.

Since removing $e$ from $T \cup \{e\}$ removes all cycles, we get that
$T \cup \{e\}$ has exactly one cycle $C$. Let $e' \neq e$ be any other
edge in $C$. Removing $e'$ from $T \cup \{e\}$ yields a spanning tree
since it eliminates the only cycle in $T \cup \{e\}$, and eliminating
cycles cannot cause vertices to be disconnected. This proves the
"moreover" part of the lemma statement.

:::

The following lemma implies that in the last iteration of Kruskal's
algorithm, the spanning tree $F$ is contained in a minimum spanning tree
and is thus a minimum spanning tree, proving @thm-kruskal.

::: {prf:lemma label=lem-Kruskal}

In every iteration of the algorithm, there is a minimum spanning tree
$T$ that contains $F$, and $T \setminus F$ is contained in $S$.
Equivalently, every edge of $T$ that is not yet selected by the
algorithm has not been discarded.

:::

::: {prf:proof enumerated=false}

We prove the lemma statement by induction. At the start of the first
iteration of the ****while**** loop, we have that $F = \emptyset$ and
$S = E$, every minimum spanning tree $T$ satisfies the desired
properties.

Next, suppose the statement holds at the start of iteration $i > 1$. We
will show that it continues to hold at the end of the iteration.
Consider $S$ and $F$ at the start of iteration $i$ and let $T$ be the
minimum spanning tree that contains $F$ and $T \setminus F$ is contained
in $S$.

If $F$ is spanning, then no edge is added to $F$ and the statement
continues to hold at the end of the iteration. Suppose $F$ is not
spanning. Let $e$ be the minimum-weight edge $e$ in $S$. If
$F \cup \{e\}$ has a cycle, then no edge is added to $F$ as well.

The only remaining case is then that $F$ is not spanning and there is no
cycle in $F \cup \{e\}$. If $e \in T$, then clearly the statement holds.
Suppose $e \notin T$. Then, we need to show that there is a different
minimum spanning tree $T'$ such that $T'$ contains $F \cup \{e\}$ and
$T' \setminus F$ is contained in $S \setminus \{e\}$.

By @lem-greedy-swap, we have that $T \cup \{e\}$ has exactly one cycle
$C$ and removing any other edge from $T \cup \{e\}$ yields a spanning
tree. Since the algorithm goes through $E$ in increasing order of
weight, we have that every edge of $C \setminus (\{e\} \cup F)$ has
weight at least that of $e$. Let $e'$ be one of those edges. Then,
$T' = T \cup \{e\} \setminus \{e'\}$ is a spanning tree containing
$F \cup \{e\}$. It is also a minimum spanning tree since its weight is
at most that of $T$, which is a minimum spanning tree.

:::

## Time complexity

The majority of this subject concerns problems that are NP-hard and for
which we are happy to settle with polynomial time algorithms with
approximation guarantees. We will define approximation guarantees later
on. The point is that in this subject, we are content with running time
analyses that show the algorithm is polynomial time.

::: {prf:theorem}

Kruskal's algorithm is polynomial time.

:::

::: {prf:proof enumerated=false}

Initializing $S$ and sorting takes $O(m \log m)$ time. Each iteration of
the loop removes one edge from $S$, which initially has all $m$ edges.
Thus, there are at most $m$ iterations.

Next, let us analyze the time complexity of each iteration. There are
two steps. The first step is finding the minimum-weight edge $e$ in $S$
and removing it from $S$; each of these can be done in $O(m)$ time by
going through all of $S$. The second step involves checking if
$F \cup \{e\}$ has a cycle. This can be done by doing say, a depth-first
search, which takes $O(m)$ time. Adding $e$ to $F$ also takes $O(m)$
time. Finally, we need to check if $S$ is empty ($O(1)$ time) and if $F$
is spanning (can be done with say, breadth-first search, which takes
$O(m)$ time). Thus, the time complexity of each iteration is $O(m)$.

Therefore, the total running time is $O(m) + O(m^2)$, which is
polynomial time.

:::
