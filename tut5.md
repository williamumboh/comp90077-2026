(sec-tut5)=

# Tutorial 5

In this tutorial, we will use duality to analyze a greedy algorithm for
the Interval Hitting Set problem, a special case of Set Cover.

::: {prf:definition} Interval Hitting Set

Let $I_1, \ldots, I_n$ be a collection of intervals $I_j = [a_j, b_j]$
where $a_j$ and $b_j$ are integers such that
$1 \leq a_j \leq b_j \leq n$. A subset $S \subseteq \{1, \ldots, n\}$ is
a *hitting set* if $I_j \cap S \neq \emptyset$ for every interval $I_j$.
That is, every interval contains some integer in $S$. The goal is to
find an interval hitting set $S$ that minimizes $|S|$.

:::

The dual problem is the following.

::: {prf:definition} Disjoint Intervals

Let $I_1, \ldots, I_n$ be a collection of intervals $I_j = [a_j, b_j]$
where $a_j$ and $b_j$ are integers such that
$1 \leq a_j \leq b_j \leq n$.. A subset of intervals $\mathcal{I}$ is
*disjoint* if for every pair of intervals $I_j, I_k \in \mathcal{I}$, we
have $I_j \cap I_k = \emptyset$. The goal is to find a disjoint subset
$\mathcal{I}$ that maximizes $|\mathcal{I}|$.

:::

First, we show that the problems are dual to each other.

::: {exercise}

Show that for every hitting set $S$ and every set of disjoint intervals
$\mathcal{I}$, we have $|\mathcal{I}| \leq |S|$.

:::

Next, consider the following natural greedy algorithm for Interval
Hitting Set.

::: {prf:algorithm} Greedy Interval

- Initialize $S \leftarrow \emptyset$
- **while** $S$ not a hitting set ****do****
  - Let $I_j = [a_j,b_j]$ be the interval not hit by $S$ with the
    smallest $b_j$
  - Add $b_j$ to $S$
- **end while**
- **return** $S$

:::

::: {exercise}

Let $S$ be the output of the Greedy Interval algorithm. Show that there
exists a subset $\mathcal{I}$ of disjoint intervals such that
$|\mathcal{I}| = |S|$.

:::

::: {exercise}

Conclude that $S$ is a minimum interval hitting set.

:::

::: {exercise}

Design an algorithm that finds the maximum set of disjoint intervals and
prove that it is optimal.

:::
