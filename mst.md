(sec-MST)=

# Minimum Spanning Trees

We begin our journey by recalling the classic Minimum Spanning Tree
problem.

::: {prf:definition label=prob-MST} Minimum Spanning Tree

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

Let $F_i$ and $S_i$ be $F$ and $S$, respectively, at the end of
iteration $i$. For every iteration $i$ of the algorithm, there is a
minimum spanning tree $T_i$ that satisfies the following properties:
(prop-Kruskal)=

1.  $T_i$ contains $F_i$;
2.  $T_i \setminus F_i$ is contained in $S_i$. Equivalently, every edge
    of $T$ that is not yet selected by the algorithm has not been
    processed by the algorithm.

:::

::: {prf:proof enumerated=false label=prf-lem-Kruskal}

We prove the lemma statement by induction on $i$. In the base case
($i=0$), we have $F_0 = \emptyset$ and $S_0 = E$, and so we can choose
$T_0$ to be any minimum spanning tree $T$.

Next, suppose the statement holds for $i - 1$. We will show that
[property 1](#prop-Kruskal) also holds for $i$. Let $e$ be the
minimum-weight edge in $S_{i-1}$. If the algorithm discards $e$, then
$F_i = F_{i-1}$ so choosing $T_i = T_{i-1}$ works. Suppose the algorithm
adds $e$, i.e. $F_i = F_{i-1} \cup \{e\}$. If $e \in T_{i-1}$, then
again, choosing $T_i = T_{i-1}$ works.

The only remaining case is then that $F_i = F_{i-1} \cup \{e\}$ and
$e \notin T_{i-1}$. We now use @lem-greedy-swap to construct a new MST
$T_i$ that satisfies the desired properties. @lem-greedy-swap implies
that $T_{i-1} \cup \{e\}$ has exactly one cycle $C$ and removing any
other edge from $T \cup \{e\}$ yields a spanning tree.

Next, we show that there is an edge $f \in C$ that is not in $F_i$ and
whose weight is at least that of $e$. This is where [property
2](#prop-Kruskal) of $T_{i-1}$ guaranteed by the inductive hypothesis
comes in handy. Since $F_i$ is acyclic, there is at least one edge in
$C \setminus F_i$. By definition of $F_i$, we have
$C \setminus F_i = C \setminus (F_{i-1} \cup \{e\})$. By definition of
$C$, every edge of $C$ is in $T_{i-1}$, except $e$. Therefore
$C \setminus F_i$ is contained in
$T_{i-1} \setminus F_{i-1} \subseteq S_{i-1}$. Since $e$ is the
minimum-weight edge of $S_{i-1}$, we get that every edge of
$C \setminus F_i$ has weight at least $w_e$.

Let $f$ be any edge in $C \setminus F_i$. Since $f$ is not in $F_i$ and
has weight at least $w_e$, adding $e$ and removing $f$ from $T_{i-1}$
yields a spanning tree $T_i$ whose total weight is at most that of
$T_{i-1}$ and is thus also a MST. Moreover, $T_i$ contains $F_i$. The
proof that $T_i \setminus F_i$ is contained in $S_i$ is left as a
[tutorial exercise](#ex-prop2-Kruskal).

:::

## Time complexity

The majority of this subject concerns problems that are NP-hard and for
which we are happy to settle with polynomial time algorithms with
approximation guarantees. We will define approximation guarantees later
on. The point is that in this subject, we are content with running time
analyses that show the algorithm is polynomial time. We will not worry
about optimizing the algorithm and analysis to get better polynomials.

::: {prf:theorem}

Kruskal's algorithm is polynomial time.

:::

::: {prf:proof enumerated=false}

Initializing $S$ and sorting takes $O(m \log m)$ time. Each iteration of
the loop removes one edge from $S$, which initially has all $m$ edges.
Thus, there are at most $m$ iterations.

Next, let us analyze the time complexity of each iteration. There are
three steps. The first step is to check if $S$ is empty ($O(1)$ time)
and if $F$ is spanning (can be done with say, breadth-first search,
which takes $O(m)$ time). The second step involves finding the
minimum-weight edge $e$ in $S$ and removing it from $S$; each of these
can be done in $O(m)$ time by going through all of $S$. Finally, we need
to check if $F \cup \{e\}$ has a cycle. This can be done by doing say, a
depth-first search, which takes $O(m)$ time. Adding $e$ to $F$ also
takes $O(m)$ time. Thus, the time complexity of each iteration is
$O(m)$.

Therefore, the total running time is $O(m) + O(m^2)$, which is
polynomial time.

:::

## Improving running time to $O(m \log m)$

We mention two improvements to get the running time down to
$O(m \log m)$:

1.  Checking if $F$ is spanning can be done in $O(1)$ time by keeping a
    count of the number of edges in $F$. This uses the fact that the
    algorithm maintains the invariant that $F$ is acyclic and an acyclic
    subgraph is spanning if and only if it has $n-1$ edges.
2.  Checking if adding an edge $e$ to $F$ can also be sped up by using a
    [disjoint-set data
    structure](https://en.wikipedia.org/wiki/Disjoint-set_data_structure).
    The key idea is that since $F$ is acyclic, adding $e$ to $F$ creates
    a cycle if and only if the two endpoints of $e$ are in the same
    connected component of $F$. Thus, it suffices to maintain a data
    structure that keeps track of the connected components of $F$, which
    is what the disjoint-set data structure lets us do.

::: {exercise label=ex-MST-size}

Prove that an acyclic subgraph is spanning if and only if it has $n-1$
edges.

:::

::: {exercise label=ex-MST-comp-cycle}

Prove that adding an edge $e = (u,v)$ to an acyclic subgraph of $F$
creates a cycle if and only if $u$ and $v$ are in the same component of
$F$.

:::

::: {note}

The disjoint-set data structure is outside the scope of the subject and
not assessable. @ex-MST-size and @ex-MST-comp-cycle are assessable.

:::

## Even faster? (Not assessable)

In reply to a question asked during the lecture, I guessed that it is
not possible to do better than $O(m \log m)$. This turned out to be
wrong. There is an almost-linear time algorithm and a randomized
linear-time algorithm. See
[Wikipedia](https://en.wikipedia.org/wiki/Minimum_spanning_tree#Faster_algorithms)
for details.
