(sec-tut5)=

# Tutorial 5

In this tutorial, we will use duality to analyze a greedy algorithm for
the Interval Hitting Set problem, a special case of Set Cover. In the
following, the interval $[a,b]$ is the set of integers between $a$ and
$b$, i.e. $[a,b] = \{a, a + 1, \ldots, b-1, b+1\}$.

::: {prf:definition label=prob-interval-hitting} Interval Hitting Set

Let $I_1, \ldots, I_n$ be a collection of intervals $I_j = [a_j, b_j]$
where $a_j$ and $b_j$ are integers such that
$1 \leq a_j \leq b_j \leq n$. A subset $S \subseteq \{1, \ldots, n\}$ is
a *hitting set* if $I_j \cap S \neq \emptyset$ for every interval $I_j$.
That is, every interval contains some integer in $S$. The goal is to
find a hitting set $S$ that minimizes $|S|$.

:::

The dual problem is the following.

::: {prf:definition label=prob-interval-disjoint} Disjoint Intervals

Let $I_1, \ldots, I_n$ be a collection of intervals $I_j = [a_j, b_j]$
where $a_j$ and $b_j$ are integers such that
$1 \leq a_j \leq b_j \leq n$.. A subset of intervals $\mathcal{I}$ is
*disjoint* if for every pair of intervals $I_j, I_k \in \mathcal{I}$, we
have $I_j \cap I_k = \emptyset$. The goal is to find a disjoint subset
$\mathcal{I}$ that maximizes $|\mathcal{I}|$.

:::

First, we show that the problems are dual to each other.

::: {exercise label=ex-5-1}

Show that for every hitting set $S$ and every set of disjoint intervals
$\mathcal{I}$, we have $|\mathcal{I}| \leq |S|$.

:::

::: {solution class=dropdown} ex-5-1

Let $S$ be a hitting set and $\mathcal{I}$ be a set of disjoint
intervals. Since $\mathcal{I}$ is disjoint, no single integer can hit
more than one interval of $\mathcal{I}$. Therefore,
$|\mathcal{I}| \leq |S|$.

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

::: {exercise label=ex-5-2}

Let $S$ be the output of the Greedy Interval algorithm. Show that there
exists a subset $\mathcal{I}$ of disjoint intervals such that
$|\mathcal{I}| = |S|$.

:::

::: {solution class=dropdown} ex-5-2

Suppose $|S| = \ell$. For each iteration $1 \leq k \leq \ell$, let $b_k$
be the integer added to $S$ and $I_k = [a_k,b_k]$ be the interval for
which $b_k$ is the right endpoint.

We claim that the set of intervals $\mathcal{I}$ consisting of the
$\ell$ intervals $I_k$ for $1 \leq k \leq \ell$ is disjoint. Consider
intervals $I_k = [a_k,b_k]$ and $I_{k'} = [a_{k'}, b_{k'}]$ for
$1\leq k < k' \leq \ell$. At the start of iteration $k$, since the
interval $[a_k,b_k]$ with has the smallest right endpoint $b_k$ among
intervals not hit by $S$, we have that $b_k \leq b_{k'}$. Since $b_k$ is
added to $S$ at the end of iteration $k$, and $I_{k'}$ is the selected
interval at the later iteration $k' > k$, we have that
$b_k \notin [a_{k'},b_{k'}]$. Together with the fact that
$b_k \leq b_{k'}$, we have that $a_{k'} > b_k$ and so the two intervals
are disjoint.

Thus, we conclude that $\mathcal{I}$ is a collection of $\ell = |S|$
disjoint intervals.

:::

::: {exercise label=ex-5-3}

Conclude that $S$ is a minimum interval hitting set.

:::

::: {solution class=dropdown} ex-5-3

Let $S^*$ be a minimum interval hitting set. Let $\mathcal{I}$ be the
set of $|S|$ disjoint intervals obtained from @ex-5-2. By @ex-5-1, we
get that $|S^*| \geq |\mathcal{I}| = |S|$. Therefore, $S$ is a minimum
hitting set.

:::

::: {exercise label=ex-5-4}

Design an algorithm that finds the maximum set of disjoint intervals and
prove that it is optimal.

:::

:::::: {solution class=dropdown} ex-5-4

::: {prf:algorithm} Disjoint Interval

- Initialize $S \leftarrow \emptyset$ and
  $\mathcal{I} \leftarrow \emptyset$
- **while** $S$ not a hitting set ****do****
  - Let $I_j = [a_j,b_j]$ be the interval not hit by $S$ with the
    smallest $b_j$
  - Add $b_j$ to $S$
  - Add $I_j$ to $\mathcal{I}$
- **end while**
- **return** $\mathcal{I}$

:::

Let $\mathcal{I}^*$ be a maximum set of disjoint intervals. By @ex-5-1,
we have $|\mathcal{I}^*| \leq |S| = |\mathcal{I}|$. Thus, $\mathcal{I}$
is a maximum set of disjoint intervals as well.

::::::
