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

::: {tip}

It may help to break down the problem by considering different cases.
For example, the case when $F_2$ is contained in $F_1$ should be
straightforward.

:::

::: {hint class=dropdown hidden=true}

What can you do with an edge in $F_1 \setminus F_2$?

:::

::: {exercise}

In the example MST input shown in Lecture 1 (graph on the left of page
15 of [Lecture 1 slides](./slides-wk1.pdf)), there is more than 1 MST
due to two edges having the same weight.

Prove that if the edge weights are distinct, i.e. no two edges have the
same weight, then there is only 1 MST.

:::

::: {hint class=dropdown hidden=true}

Use [Greedy Swap Lemma](#lem-greedy-swap).

:::
