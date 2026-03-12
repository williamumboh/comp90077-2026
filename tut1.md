(sec-tut1)=

# Tutorial 1: MSTs

This tutorial is all about the [Minimum Spanning Tree
problem](#prob-MST). Try to think about the problems on your own first
before discussing with others, and try to discuss with others before
using the hints.

::: {tip}

When stuck, it often helps to look at simple examples such as the ones
in [Lecture 1 slides](./slides-wk1.pdf).

:::

::: {exercise label=ex-prop2-Kruskal}

Complete the [proof](#prf-lem-Kruskal) of @lem-Kruskal (in @sec-MST):
Show that $T_i \setminus F_i$ is contained in $S_i$.

:::

::: {solution class=dropdown} ex-prop2-Kruskal

Let $e$ be the smallest edge in $S_{i-1}$. Note that
$S_i = S_{i-1} \setminus \{e\}$.

There are two cases to consider: whether $e$ was added in iteration $i$
or not, i.e. whether $F_i = F_{i-1} \cup \{e\}$ or $F_i = F_{i-1}$.

Suppose $F_i = F_{i-1}$. Then, $T_i = T_{i-1}$ and so
$T_i \setminus F_i = T_{i-1} \setminus F_{i-1}$. Since
$S_i = S_{i-1} \setminus \{e\}$, all we need to do is to argue that
$e \notin T_{i-1}$ since that would imply that
$T_{i-1} \setminus F_{i-1}$ is contained in
$S_{i-1} \setminus \{e\} = S_i$ and so $T_i \setminus F_i$ is contained
in $S_i$, as desired.

To see that $e \notin T_{i-1}$, observe that the algorithm did not add
$e$ only because adding $e$ to $F_{i-1}$ creates a cycle. Since
$F_{i-1}$ is contained in $T_{i-1}$, we get that if $e \in T_{i-1}$,
then $T_{i-1}$ would also contain a cycle. Since $T_{i-1}$ does not have
a cycle, we get that $e$ is not in $T_{i-1}$. This completes the
analysis of the first case.

In the second case, $F_i = F_{i-1} \cup \{e\}$. If $e \in T_{i-1}$, then
$T_i = T_{i-1}$. Since $e \in F_i$, we get that
$T_i \setminus F_i = T_{i-1} \setminus (F_{i-1} \cup \{e\}) = (T_{i-1} \setminus F_{i-1}) \setminus \{e\}$
which is contained in $S_{i-1} \setminus \{e\} = S_i$ since
$T_{i-1} \setminus F_{i-1}$ is contained in $S_{i-1}$.

Finally, we consider when $e \notin T_{i-1}$. In this case,
$T_i = T_{i-1} \cup \{e\} \setminus \{f\}$ where $f$ has weight at least
that of $e$. Thus,
$T_i \setminus F_i = (T_{i-1} \cup \{e\} \setminus \{f\}) \setminus (F_{i-1} \cup \{e\})$
which is contained in $T_{i-1} \setminus F_{i-1}$. Since
$e \notin T_{i-1}$, we get that $T_{i-1} \setminus F_{i-1}$ is contained
in $S_{i-1} \setminus \{e\} = S_i$, as desired.

:::

The next two exercises help develop familiarity with "greedy swap"-type
arguments.

::: {exercise label=ex-1-2} Greedy Augmentation

The proof of correctness of Kruskal's heavily relied on the [Greedy Swap
Lemma](#lem-greedy-swap). In this exercise, we prove a variant.

Let $F_1$ and $F_2$ be acyclic subgraphs. Show that if $F_1$ has more
edges than $F_2$, then there is an edge $e$ in $F_1$ but not $F_2$ that
we can add to $F_2$ without creating cycles. Equivalently, if
$|F_1| > |F_2|$, then there is some edge $e \in F_1 \setminus F_2$ such
that $F_2 \cup \{e\}$ is also acyclic.

:::

::: {solution class=dropdown} ex-1-2

First, observe that if an edge subset $H$ is acyclic, then the number of
connected components of $H$ (an isolated vertex is considered a
connected component by itself) is exactly $n - |H|$.[^1]

Since $|F_1| > |F_2|$, the number of connected components of $F_1$ is
less than that of $F_2$. Moreover, the set of connected components
partition the vertex set, and so there must be a connected component of
$F_1$ that contains two vertices $u,v$ that are in different components
of $F_2$. Thus, the path between $u$ and $v$ in $F_1$ must contain an
edge $e$ that connects the two components of $F_2$ containing $u$ and
$v$. Therefore, adding $e$ to $F_2$ does not create a cycle.

:::

::: {tip}

It may help to break down the problem by considering different cases.
For example, the case when $F_2$ is contained in $F_1$ should be
straightforward.

:::

::: {hint class=dropdown}

What can you do with an edge in $F_1 \setminus F_2$?

:::

::: {exercise label=ex-1-3}

In the example MST input shown in Lecture 1 (graph on the left of page
15 of [Lecture 1 slides](./slides-wk1.pdf)), there is more than 1 MST
due to two edges having the same weight.

Prove that if the edge weights are distinct, i.e. no two edges have the
same weight, then there is only 1 MST.

:::

::: {hint class=dropdown}

Use [Greedy Swap Lemma](#lem-greedy-swap).

:::

::: {solution class=dropdown} ex-1-3

It's easier to prove the contrapositive: if there are two MSTs, then
there are at least two edges with the same weight. Let $T$ and $T'$ be
the two MSTs. Let $e$ be the smallest edge contained in one of $T$ and
$T'$ but not both. Note that such an edge exists since $T \neq T'$ and
all spanning trees have the same number of edges. Suppose that $e$ is an
edge in $T$ but not in $T'$. The [Greedy Swap Lemma](#lem-greedy-swap)
implies that $T' \cup \{e\}$ contains a single cycle $C$ and that
removing any other edge from the cycle yields a spanning tree. Since
$T'$ is an MST, we get that $w_f \leq w_e$ for every other edge $f$ in
$C$; otherwise, we can swap $f$ with $e$ and get a cheaper spanning
tree.

Since $T$ does not have cycles, there is an edge $f'$ of $C$ that is
different from $e$ and is not in $T$. Since $e$ is the cheapest edge
that is contained in exactly one of $T$ and $T'$, we get that
$w_{f'} \geq w_e$. Thus, $w_{f'} = w_e$.

:::

[^1]: Imagine adding the edges of $H$ to an empty graph one at a time.
    Each edge that is added connects two connected components
    (otherwise, it would create a cycle).
